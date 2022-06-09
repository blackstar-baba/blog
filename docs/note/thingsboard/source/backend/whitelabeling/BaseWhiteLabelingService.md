```java
package org.thingsboard.server.dao.wl;

import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.JsonNode;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.node.ObjectNode;
import com.google.common.util.concurrent.Futures;
import com.google.common.util.concurrent.ListenableFuture;
import com.google.common.util.concurrent.MoreExecutors;
import java.io.IOException;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.UUID;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.util.StringUtils;
import org.thingsboard.server.common.data.AdminSettings;
import org.thingsboard.server.common.data.Customer;
import org.thingsboard.server.common.data.EntityType;
import org.thingsboard.server.common.data.Tenant;
import org.thingsboard.server.common.data.id.AdminSettingsId;
import org.thingsboard.server.common.data.id.CustomerId;
import org.thingsboard.server.common.data.id.EntityId;
import org.thingsboard.server.common.data.id.EntityIdFactory;
import org.thingsboard.server.common.data.id.TenantId;
import org.thingsboard.server.common.data.kv.AttributeKvEntry;
import org.thingsboard.server.common.data.kv.BaseAttributeKvEntry;
import org.thingsboard.server.common.data.kv.KvEntry;
import org.thingsboard.server.common.data.kv.StringDataEntry;
import org.thingsboard.server.common.data.wl.LoginWhiteLabelingParams;
import org.thingsboard.server.common.data.wl.WhiteLabelingParams;
import org.thingsboard.server.dao.attributes.AttributesService;
import org.thingsboard.server.dao.customer.CustomerService;
import org.thingsboard.server.dao.exception.IncorrectParameterException;
import org.thingsboard.server.dao.settings.AdminSettingsService;
import org.thingsboard.server.dao.tenant.TenantService;

@Service
public class BaseWhiteLabelingService implements WhiteLabelingService {
  private static final Logger log = LoggerFactory.getLogger(BaseWhiteLabelingService.class);
  
  private static final ObjectMapper objectMapper = new ObjectMapper();
  
  private static final String LOGIN_WHITE_LABEL_PARAMS = "loginWhiteLabelParams";
  
  private static final String LOGIN_WHITE_LABEL_DOMAIN_NAME_PREFIX = "loginWhiteLabelDomainNamePrefix";
  
  private static final String WHITE_LABEL_PARAMS = "whiteLabelParams";
  
  private static final String ALLOW_WHITE_LABELING = "allowWhiteLabeling";
  
  private static final String ALLOW_CUSTOMER_WHITE_LABELING = "allowCustomerWhiteLabeling";
  
  @Autowired
  private AdminSettingsService adminSettingsService;
  
  @Autowired
  private AttributesService attributesService;
  
  @Autowired
  private TenantService tenantService;
  
  @Autowired
  private CustomerService customerService;
  
  public LoginWhiteLabelingParams getSystemLoginWhiteLabelingParams(TenantId tenantId) {
    AdminSettings loginWhiteLabelParamsSettings = this.adminSettingsService.findAdminSettingsByKey(tenantId, "loginWhiteLabelParams");
    String json = null;
    if (loginWhiteLabelParamsSettings != null)
      json = loginWhiteLabelParamsSettings.getJsonValue().get("value").asText(); 
    return constructLoginWlParams(json);
  }
  
  public WhiteLabelingParams getSystemWhiteLabelingParams(TenantId tenantId) {
    AdminSettings whiteLabelParamsSettings = this.adminSettingsService.findAdminSettingsByKey(tenantId, "whiteLabelParams");
    String json = null;
    if (whiteLabelParamsSettings != null)
      json = whiteLabelParamsSettings.getJsonValue().get("value").asText(); 
    return constructWlParams(json, true);
  }
  
  public ListenableFuture<WhiteLabelingParams> getTenantWhiteLabelingParams(TenantId tenantId) {
    return getEntityWhiteLabelParams(tenantId, (EntityId)tenantId);
  }
  
  public ListenableFuture<WhiteLabelingParams> getCustomerWhiteLabelingParams(TenantId tenantId, CustomerId customerId) {
    return getEntityWhiteLabelParams(tenantId, (EntityId)customerId);
  }
  
  public WhiteLabelingParams getMergedSystemWhiteLabelingParams(TenantId tenantId, String logoImageChecksum, String faviconChecksum) {
    WhiteLabelingParams result = getSystemWhiteLabelingParams(tenantId);
    result.prepareImages(logoImageChecksum, faviconChecksum);
    return result;
  }
  
  public LoginWhiteLabelingParams getTenantLoginWhiteLabelingParams(TenantId tenantId) throws Exception {
    return (LoginWhiteLabelingParams)getEntityLoginWhiteLabelParams(tenantId, (EntityId)tenantId).get();
  }
  
  public LoginWhiteLabelingParams getCustomerLoginWhiteLabelingParams(TenantId tenantId, CustomerId customerId) throws Exception {
    return (LoginWhiteLabelingParams)getEntityLoginWhiteLabelParams(tenantId, (EntityId)customerId).get();
  }
  
  public LoginWhiteLabelingParams getMergedLoginWhiteLabelingParams(TenantId tenantId, String domainName, String logoImageChecksum, String faviconChecksum) throws Exception {
    LoginWhiteLabelingParams result;
    AdminSettings loginWhiteLabelSettings = this.adminSettingsService.findAdminSettingsByKey(tenantId, constructLoginWhileLabelKey(domainName));
    if (loginWhiteLabelSettings != null) {
      String strEntityType = loginWhiteLabelSettings.getJsonValue().get("entityType").asText();
      String strEntityId = loginWhiteLabelSettings.getJsonValue().get("entityId").asText();
      EntityId entityId = EntityIdFactory.getByTypeAndId(strEntityType, strEntityId);
      result = (LoginWhiteLabelingParams)getEntityLoginWhiteLabelParams(tenantId, entityId).get();
      if (entityId.getEntityType().equals(EntityType.CUSTOMER)) {
        Customer customer = this.customerService.findCustomerById(tenantId, (CustomerId)entityId);
        if (customer.isSubCustomer())
          result.merge(getCustomerHierarchyLoginWhileLabelingParams(tenantId, customer.getParentCustomerId(), result)); 
      } 
      result.merge(getSystemLoginWhiteLabelingParams(tenantId));
    } else {
      result = getSystemLoginWhiteLabelingParams(tenantId);
    } 
    result.merge(getSystemWhiteLabelingParams(tenantId));
    result.prepareImages(logoImageChecksum, faviconChecksum);
    return result;
  }
  
  private LoginWhiteLabelingParams getCustomerHierarchyLoginWhileLabelingParams(TenantId tenantId, CustomerId customerId, LoginWhiteLabelingParams childCustomerWLLParams) throws Exception {
    LoginWhiteLabelingParams entityLoginWhiteLabelParams = (LoginWhiteLabelingParams)getEntityLoginWhiteLabelParams(tenantId, (EntityId)customerId).get();
    childCustomerWLLParams.merge(entityLoginWhiteLabelParams);
    Customer customer = this.customerService.findCustomerById(tenantId, customerId);
    if (customer.isSubCustomer())
      return getCustomerHierarchyLoginWhileLabelingParams(tenantId, customer.getParentCustomerId(), childCustomerWLLParams); 
    return childCustomerWLLParams;
  }
  
  public WhiteLabelingParams getMergedTenantWhiteLabelingParams(TenantId tenantId, String logoImageChecksum, String faviconChecksum) throws Exception {
    WhiteLabelingParams result = (WhiteLabelingParams)getTenantWhiteLabelingParams(tenantId).get();
    result.merge(getSystemWhiteLabelingParams(tenantId));
    result.prepareImages(logoImageChecksum, faviconChecksum);
    return result;
  }
  
  public WhiteLabelingParams getMergedCustomerWhiteLabelingParams(TenantId tenantId, CustomerId customerId, String logoImageChecksum, String faviconChecksum) throws Exception {
    WhiteLabelingParams result = (WhiteLabelingParams)getCustomerWhiteLabelingParams(tenantId, customerId).get();
    Customer customer = this.customerService.findCustomerById(tenantId, customerId);
    if (customer.isSubCustomer())
      result.merge(getMergedCustomerHierarchyWhileLabelingParams(tenantId, customer.getParentCustomerId(), result)); 
    result.merge((WhiteLabelingParams)getTenantWhiteLabelingParams(tenantId).get()).merge(getSystemWhiteLabelingParams(tenantId));
    result.prepareImages(logoImageChecksum, faviconChecksum);
    return result;
  }
  
  private WhiteLabelingParams getMergedCustomerHierarchyWhileLabelingParams(TenantId tenantId, CustomerId customerId, WhiteLabelingParams childCustomerWLParams) throws Exception {
    WhiteLabelingParams entityWhiteLabelParams = (WhiteLabelingParams)getEntityWhiteLabelParams(tenantId, (EntityId)customerId).get();
    childCustomerWLParams.merge(entityWhiteLabelParams);
    Customer customer = this.customerService.findCustomerById(tenantId, customerId);
    if (customer.isSubCustomer())
      return getMergedCustomerHierarchyWhileLabelingParams(tenantId, customer.getParentCustomerId(), childCustomerWLParams); 
    return childCustomerWLParams;
  }
  
  public WhiteLabelingParams saveSystemWhiteLabelingParams(WhiteLabelingParams whiteLabelingParams) {
    String json;
    whiteLabelingParams = prepareChecksums(whiteLabelingParams);
    AdminSettings whiteLabelParamsSettings = this.adminSettingsService.findAdminSettingsByKey(TenantId.SYS_TENANT_ID, "whiteLabelParams");
    if (whiteLabelParamsSettings == null) {
      whiteLabelParamsSettings = new AdminSettings();
      whiteLabelParamsSettings.setKey("whiteLabelParams");
      ObjectNode node = objectMapper.createObjectNode();
      whiteLabelParamsSettings.setJsonValue((JsonNode)node);
    } 
    try {
      json = objectMapper.writeValueAsString(whiteLabelingParams);
    } catch (JsonProcessingException e) {
      log.error("Unable to convert White Labeling Params to JSON!", (Throwable)e);
      throw new IncorrectParameterException("Unable to convert White Labeling Params to JSON!");
    } 
    ((ObjectNode)whiteLabelParamsSettings.getJsonValue()).put("value", json);
    this.adminSettingsService.saveAdminSettings(TenantId.SYS_TENANT_ID, whiteLabelParamsSettings);
    return getSystemWhiteLabelingParams(TenantId.SYS_TENANT_ID);
  }
  
  public LoginWhiteLabelingParams saveSystemLoginWhiteLabelingParams(LoginWhiteLabelingParams loginWhiteLabelingParams) {
    String json;
    loginWhiteLabelingParams = prepareChecksums(loginWhiteLabelingParams);
    AdminSettings loginWhiteLabelParamsSettings = this.adminSettingsService.findAdminSettingsByKey(TenantId.SYS_TENANT_ID, "loginWhiteLabelParams");
    if (loginWhiteLabelParamsSettings == null) {
      loginWhiteLabelParamsSettings = new AdminSettings();
      loginWhiteLabelParamsSettings.setKey("loginWhiteLabelParams");
      ObjectNode node = objectMapper.createObjectNode();
      loginWhiteLabelParamsSettings.setJsonValue((JsonNode)node);
    } 
    try {
      json = objectMapper.writeValueAsString(loginWhiteLabelingParams);
    } catch (JsonProcessingException e) {
      log.error("Unable to convert Login White Labeling Params to JSON!", (Throwable)e);
      throw new IncorrectParameterException("Unable to convert Login White Labeling Params to JSON!");
    } 
    ((ObjectNode)loginWhiteLabelParamsSettings.getJsonValue()).put("value", json);
    this.adminSettingsService.saveAdminSettings(TenantId.SYS_TENANT_ID, loginWhiteLabelParamsSettings);
    return getSystemLoginWhiteLabelingParams(TenantId.SYS_TENANT_ID);
  }
  
  public LoginWhiteLabelingParams saveTenantLoginWhiteLabelingParams(TenantId tenantId, LoginWhiteLabelingParams loginWhiteLabelingParams) throws Exception {
    saveEntityLoginWhiteLabelingParams(tenantId, (EntityId)tenantId, loginWhiteLabelingParams);
    return getTenantLoginWhiteLabelingParams(tenantId);
  }
  
  public LoginWhiteLabelingParams saveCustomerLoginWhiteLabelingParams(TenantId tenantId, CustomerId customerId, LoginWhiteLabelingParams loginWhiteLabelingParams) throws Exception {
    saveEntityLoginWhiteLabelingParams(tenantId, (EntityId)customerId, loginWhiteLabelingParams);
    return getCustomerLoginWhiteLabelingParams(tenantId, customerId);
  }
  
  private void saveEntityLoginWhiteLabelingParams(TenantId tenantId, EntityId entityId, LoginWhiteLabelingParams loginWhiteLabelParams) {
    loginWhiteLabelParams = prepareChecksums(loginWhiteLabelParams);
    String loginWhiteLabelKey = constructLoginWhileLabelKey(loginWhiteLabelParams.getDomainName());
    AdminSettings existentAdminSettingsByKey = this.adminSettingsService.findAdminSettingsByKey(tenantId, loginWhiteLabelKey);
    if (StringUtils.isEmpty(loginWhiteLabelParams.getAdminSettingsId())) {
      if (existentAdminSettingsByKey == null) {
        existentAdminSettingsByKey = saveLoginWhiteLabelSettings(tenantId, entityId, loginWhiteLabelKey);
        loginWhiteLabelParams.setAdminSettingsId(((AdminSettingsId)existentAdminSettingsByKey.getId()).getId().toString());
      } else {
        log.error("Current domain name [{}] already registered in the system!", loginWhiteLabelParams.getDomainName());
        throw new IncorrectParameterException("Current domain name [" + loginWhiteLabelParams.getDomainName() + "] already registered in the system!");
      } 
    } else {
      AdminSettings existentLoginWhiteLabelSettingsById = this.adminSettingsService.findAdminSettingsById(tenantId, new AdminSettingsId(
            
            UUID.fromString(loginWhiteLabelParams.getAdminSettingsId())));
      if (existentLoginWhiteLabelSettingsById == null) {
        log.error("Admin setting ID is already set in login white labeling object, but doesn't exist in the database");
        throw new IllegalStateException("Admin setting ID is already set in login white labeling object, but doesn't exist in the database");
      } 
      if (!existentLoginWhiteLabelSettingsById.getKey().equals(loginWhiteLabelKey))
        if (existentAdminSettingsByKey == null) {
          this.adminSettingsService.deleteAdminSettingsByKey(tenantId, existentLoginWhiteLabelSettingsById.getKey());
          existentLoginWhiteLabelSettingsById = saveLoginWhiteLabelSettings(tenantId, entityId, loginWhiteLabelKey);
          loginWhiteLabelParams.setAdminSettingsId(((AdminSettingsId)existentLoginWhiteLabelSettingsById.getId()).getId().toString());
        } else {
          log.error("Current domain name [{}] already registered in the system!", loginWhiteLabelParams.getDomainName());
          throw new IncorrectParameterException("Current domain name [" + loginWhiteLabelParams.getDomainName() + "] already registered in the system!");
        }  
    } 
    saveEntityWhiteLabelParams(tenantId, entityId, (WhiteLabelingParams)loginWhiteLabelParams, "loginWhiteLabelParams");
  }
  
  private AdminSettings saveLoginWhiteLabelSettings(TenantId tenantId, EntityId currentEntityId, String loginWhiteLabelKey) {
    AdminSettings loginWhiteLabelSettings = new AdminSettings();
    loginWhiteLabelSettings.setKey(loginWhiteLabelKey);
    ObjectNode node = objectMapper.createObjectNode();
    loginWhiteLabelSettings.setJsonValue((JsonNode)node);
    ((ObjectNode)loginWhiteLabelSettings.getJsonValue()).put("entityType", currentEntityId.getEntityType().name());
    ((ObjectNode)loginWhiteLabelSettings.getJsonValue()).put("entityId", currentEntityId.getId().toString());
    return this.adminSettingsService.saveAdminSettings(tenantId, loginWhiteLabelSettings);
  }
  
  public ListenableFuture<WhiteLabelingParams> saveTenantWhiteLabelingParams(TenantId tenantId, WhiteLabelingParams whiteLabelingParams) {
    whiteLabelingParams = prepareChecksums(whiteLabelingParams);
    ListenableFuture<List<Void>> listListenableFuture = saveEntityWhiteLabelParams(tenantId, (EntityId)tenantId, whiteLabelingParams, "whiteLabelParams");
    return Futures.transformAsync(listListenableFuture, list -> getTenantWhiteLabelingParams(tenantId), MoreExecutors.directExecutor());
  }
  
  public ListenableFuture<WhiteLabelingParams> saveCustomerWhiteLabelingParams(TenantId tenantId, CustomerId customerId, WhiteLabelingParams whiteLabelingParams) {
    whiteLabelingParams = prepareChecksums(whiteLabelingParams);
    ListenableFuture<List<Void>> listListenableFuture = saveEntityWhiteLabelParams(tenantId, (EntityId)customerId, whiteLabelingParams, "whiteLabelParams");
    return Futures.transformAsync(listListenableFuture, list -> getCustomerWhiteLabelingParams(tenantId, customerId), MoreExecutors.directExecutor());
  }
  
  public WhiteLabelingParams mergeSystemWhiteLabelingParams(WhiteLabelingParams whiteLabelingParams) {
    return whiteLabelingParams;
  }
  
  public WhiteLabelingParams mergeTenantWhiteLabelingParams(TenantId tenantId, WhiteLabelingParams whiteLabelingParams) {
    return whiteLabelingParams.merge(getSystemWhiteLabelingParams(tenantId));
  }
  
  public WhiteLabelingParams mergeCustomerWhiteLabelingParams(TenantId tenantId, WhiteLabelingParams whiteLabelingParams) {
    try {
      WhiteLabelingParams otherWlParams = (WhiteLabelingParams)getTenantWhiteLabelingParams(tenantId).get();
      return whiteLabelingParams.merge(otherWlParams)
        .merge(getSystemWhiteLabelingParams(tenantId));
    } catch (Exception e) {
      log.error("Unable to merge Customer White Labeling Params!", e);
      throw new RuntimeException("Unable to merge Customer White Labeling Params!", e);
    } 
  }
  
  public void deleteDomainWhiteLabelingByEntityId(TenantId tenantId, EntityId entityId) {
    try {
      LoginWhiteLabelingParams params = (LoginWhiteLabelingParams)getEntityLoginWhiteLabelParams(tenantId, entityId).get();
      if (!StringUtils.isEmpty(params.getDomainName())) {
        String loginWhiteLabelKey = constructLoginWhileLabelKey(params.getDomainName());
        this.adminSettingsService.deleteAdminSettingsByKey(tenantId, loginWhiteLabelKey);
      } 
    } catch (Exception e) {
      log.error("Unable to get entity Login White Labeling Params!", e);
      throw new RuntimeException("Unable to get entity Login White Labeling Params!", e);
    } 
  }
  
  private WhiteLabelingParams constructWlParams(String json, boolean isSystem) {
    WhiteLabelingParams result = null;
    if (!StringUtils.isEmpty(json))
      try {
        result = (WhiteLabelingParams)objectMapper.readValue(json, WhiteLabelingParams.class);
        if (isSystem) {
          JsonNode jsonNode = objectMapper.readTree(json);
          if (!jsonNode.has("helpLinkBaseUrl"))
            result.setHelpLinkBaseUrl("https://thingsboard.io"); 
          if (!jsonNode.has("enableHelpLinks"))
            result.setEnableHelpLinks(Boolean.valueOf(true)); 
        } 
      } catch (IOException e) {
        log.error("Unable to read White Labeling Params from JSON!", e);
        throw new IncorrectParameterException("Unable to read White Labeling Params from JSON!");
      }  
    if (result == null) {
      result = new WhiteLabelingParams();
      if (isSystem) {
        result.setHelpLinkBaseUrl("https://thingsboard.io");
        result.setEnableHelpLinks(Boolean.valueOf(true));
      } 
    } 
    return result;
  }
  
  private ListenableFuture<WhiteLabelingParams> getEntityWhiteLabelParams(TenantId tenantId, EntityId entityId) {
    ListenableFuture<String> jsonFuture;
    if (isWhiteLabelingAllowed(tenantId, entityId)) {
      jsonFuture = getEntityAttributeValue(tenantId, entityId, "whiteLabelParams");
    } else {
      jsonFuture = Futures.immediateFuture("");
    } 
    return Futures.transform(jsonFuture, json -> constructWlParams(json, false), MoreExecutors.directExecutor());
  }
  
  private LoginWhiteLabelingParams constructLoginWlParams(String json) {
    LoginWhiteLabelingParams result = null;
    if (!StringUtils.isEmpty(json))
      try {
        result = (LoginWhiteLabelingParams)objectMapper.readValue(json, LoginWhiteLabelingParams.class);
      } catch (IOException e) {
        log.error("Unable to read Login White Labeling Params from JSON!", e);
        throw new IncorrectParameterException("Unable to read Login White Labeling Params from JSON!");
      }  
    if (result == null)
      result = new LoginWhiteLabelingParams(); 
    return result;
  }
  
  private ListenableFuture<LoginWhiteLabelingParams> getEntityLoginWhiteLabelParams(TenantId tenantId, EntityId entityId) {
    ListenableFuture<String> jsonFuture;
    if (isWhiteLabelingAllowed(tenantId, entityId)) {
      jsonFuture = getEntityAttributeValue(tenantId, entityId, "loginWhiteLabelParams");
    } else {
      jsonFuture = Futures.immediateFuture("");
    } 
    return Futures.transform(jsonFuture, this::constructLoginWlParams, MoreExecutors.directExecutor());
  }
  
  public boolean isWhiteLabelingAllowed(TenantId tenantId, EntityId entityId) {
    if (entityId.getEntityType().equals(EntityType.CUSTOMER)) {
      Customer customer = this.customerService.findCustomerById(tenantId, (CustomerId)entityId);
      if (customer == null)
        return false; 
      if (customer.isSubCustomer()) {
        if (isWhiteLabelingAllowed(tenantId, (EntityId)customer.getParentCustomerId()))
          return isWhiteLabelingAllowed(tenantId, (EntityId)customer.getCustomerId()); 
        return false;
      } 
      if (isCustomerWhiteLabelingAllowed(customer.getTenantId())) {
        JsonNode allowWhiteLabelJsonNode = customer.getAdditionalInfo().get("allowWhiteLabeling");
        if (allowWhiteLabelJsonNode == null)
          return true; 
        return allowWhiteLabelJsonNode.asBoolean();
      } 
      return false;
    } 
    if (entityId.getEntityType().equals(EntityType.TENANT)) {
      Tenant tenant = this.tenantService.findTenantById((TenantId)entityId);
      JsonNode allowWhiteLabelJsonNode = (tenant.getAdditionalInfo() != null) ? tenant.getAdditionalInfo().get("allowWhiteLabeling") : null;
      if (allowWhiteLabelJsonNode == null)
        return true; 
      return allowWhiteLabelJsonNode.asBoolean();
    } 
    log.error("Unsupported entity type [{}]!", entityId.getEntityType().name());
    throw new IncorrectParameterException("Unsupported entity type [" + entityId.getEntityType().name() + "]!");
  }
  
  public boolean isCustomerWhiteLabelingAllowed(TenantId tenantId) {
    Tenant tenant = this.tenantService.findTenantById(tenantId);
    JsonNode allowWhiteLabelJsonNode = (tenant.getAdditionalInfo() != null) ? tenant.getAdditionalInfo().get("allowWhiteLabeling") : null;
    if (allowWhiteLabelJsonNode == null)
      return true; 
    if (allowWhiteLabelJsonNode.asBoolean()) {
      JsonNode allowCustomerWhiteLabelJsonNode = (tenant.getAdditionalInfo() != null) ? tenant.getAdditionalInfo().get("allowCustomerWhiteLabeling") : null;
      if (allowCustomerWhiteLabelJsonNode == null)
        return true; 
      return allowCustomerWhiteLabelJsonNode.asBoolean();
    } 
    return false;
  }
  
  private ListenableFuture<String> getEntityAttributeValue(TenantId tenantId, EntityId entityId, String key) {
    ListenableFuture<List<AttributeKvEntry>> attributeKvEntriesFuture;
    try {
      attributeKvEntriesFuture = this.attributesService.find(tenantId, entityId, "SERVER_SCOPE", Arrays.asList(new String[] { key }));
    } catch (Exception e) {
      log.error("Unable to read White Labeling Params from attributes!", e);
      throw new IncorrectParameterException("Unable to read White Labeling Params from attributes!");
    } 
    return Futures.transform(attributeKvEntriesFuture, attributeKvEntries -> {
          if (attributeKvEntries != null && !attributeKvEntries.isEmpty()) {
            AttributeKvEntry kvEntry = attributeKvEntries.get(0);
            return kvEntry.getValueAsString();
          } 
          return "";
        }MoreExecutors.directExecutor());
  }
  
  private ListenableFuture<List<Void>> saveEntityWhiteLabelParams(TenantId tenantId, EntityId entityId, WhiteLabelingParams whiteLabelingParams, String attributeKey) {
    String json;
    try {
      json = objectMapper.writeValueAsString(whiteLabelingParams);
    } catch (JsonProcessingException e) {
      log.error("Unable to convert White Labeling Params to JSON!", (Throwable)e);
      throw new IncorrectParameterException("Unable to convert White Labeling Params to JSON!");
    } 
    return saveEntityAttribute(tenantId, entityId, attributeKey, json);
  }
  
  private ListenableFuture<List<Void>> saveEntityAttribute(TenantId tenantId, EntityId entityId, String key, String value) {
    List<AttributeKvEntry> attributes = new ArrayList<>();
    long ts = System.currentTimeMillis();
    attributes.add(new BaseAttributeKvEntry((KvEntry)new StringDataEntry(key, value), ts));
    try {
      return this.attributesService.save(tenantId, entityId, "SERVER_SCOPE", attributes);
    } catch (Exception e) {
      log.error("Unable to save White Labeling Params to attributes!", e);
      throw new IncorrectParameterException("Unable to save White Labeling Params to attributes!");
    } 
  }
  
  private <T extends WhiteLabelingParams> T prepareChecksums(T whiteLabelingParams) {
    String logoImageChecksum = "";
    String logoImageUrl = whiteLabelingParams.getLogoImageUrl();
    if (!StringUtils.isEmpty(logoImageUrl))
      logoImageChecksum = calculateSha1Checksum(logoImageUrl); 
    whiteLabelingParams.setLogoImageChecksum(logoImageChecksum);
    String faviconChecksum = "";
    if (whiteLabelingParams.getFavicon() != null && !StringUtils.isEmpty(whiteLabelingParams.getFavicon().getUrl()))
      faviconChecksum = calculateSha1Checksum(whiteLabelingParams.getFavicon().getUrl()); 
    whiteLabelingParams.setFaviconChecksum(faviconChecksum);
    return whiteLabelingParams;
  }
  
  private static String calculateSha1Checksum(String data) {
    MessageDigest digest;
    try {
      digest = MessageDigest.getInstance("SHA1");
    } catch (NoSuchAlgorithmException e) {
      return "";
    } 
    byte[] inputBytes = data.getBytes();
    byte[] hashBytes = digest.digest(inputBytes);
    StringBuffer sb = new StringBuffer("");
    for (int i = 0; i < hashBytes.length; i++)
      sb.append(Integer.toString((hashBytes[i] & 0xFF) + 256, 16).substring(1)); 
    return sb.toString();
  }
  
  private String constructLoginWhileLabelKey(String domainName) {
    String loginWhiteLabelKey;
    if (StringUtils.isEmpty(domainName)) {
      loginWhiteLabelKey = "loginWhiteLabelParams";
    } else {
      loginWhiteLabelKey = "loginWhiteLabelDomainNamePrefix_" + domainName;
    } 
    return loginWhiteLabelKey;
  }
}

```