```java
package org.thingsboard.server.dao.wl;

import com.google.common.util.concurrent.ListenableFuture;
import org.thingsboard.server.common.data.id.CustomerId;
import org.thingsboard.server.common.data.id.EntityId;
import org.thingsboard.server.common.data.id.TenantId;
import org.thingsboard.server.common.data.wl.LoginWhiteLabelingParams;
import org.thingsboard.server.common.data.wl.WhiteLabelingParams;

public interface WhiteLabelingService {
  WhiteLabelingParams getSystemWhiteLabelingParams(TenantId paramTenantId);
  
  LoginWhiteLabelingParams getSystemLoginWhiteLabelingParams(TenantId paramTenantId);
  
  ListenableFuture<WhiteLabelingParams> getTenantWhiteLabelingParams(TenantId paramTenantId);
  
  ListenableFuture<WhiteLabelingParams> getCustomerWhiteLabelingParams(TenantId paramTenantId, CustomerId paramCustomerId);
  
  WhiteLabelingParams getMergedSystemWhiteLabelingParams(TenantId paramTenantId, String paramString1, String paramString2);
  
  WhiteLabelingParams getMergedTenantWhiteLabelingParams(TenantId paramTenantId, String paramString1, String paramString2) throws Exception;
  
  WhiteLabelingParams getMergedCustomerWhiteLabelingParams(TenantId paramTenantId, CustomerId paramCustomerId, String paramString1, String paramString2) throws Exception;
  
  LoginWhiteLabelingParams getTenantLoginWhiteLabelingParams(TenantId paramTenantId) throws Exception;
  
  LoginWhiteLabelingParams getCustomerLoginWhiteLabelingParams(TenantId paramTenantId, CustomerId paramCustomerId) throws Exception;
  
  LoginWhiteLabelingParams getMergedLoginWhiteLabelingParams(TenantId paramTenantId, String paramString1, String paramString2, String paramString3) throws Exception;
  
  WhiteLabelingParams saveSystemWhiteLabelingParams(WhiteLabelingParams paramWhiteLabelingParams);
  
  ListenableFuture<WhiteLabelingParams> saveTenantWhiteLabelingParams(TenantId paramTenantId, WhiteLabelingParams paramWhiteLabelingParams);
  
  ListenableFuture<WhiteLabelingParams> saveCustomerWhiteLabelingParams(TenantId paramTenantId, CustomerId paramCustomerId, WhiteLabelingParams paramWhiteLabelingParams);
  
  LoginWhiteLabelingParams saveSystemLoginWhiteLabelingParams(LoginWhiteLabelingParams paramLoginWhiteLabelingParams);
  
  LoginWhiteLabelingParams saveTenantLoginWhiteLabelingParams(TenantId paramTenantId, LoginWhiteLabelingParams paramLoginWhiteLabelingParams) throws Exception;
  
  LoginWhiteLabelingParams saveCustomerLoginWhiteLabelingParams(TenantId paramTenantId, CustomerId paramCustomerId, LoginWhiteLabelingParams paramLoginWhiteLabelingParams) throws Exception;
  
  WhiteLabelingParams mergeSystemWhiteLabelingParams(WhiteLabelingParams paramWhiteLabelingParams);
  
  WhiteLabelingParams mergeTenantWhiteLabelingParams(TenantId paramTenantId, WhiteLabelingParams paramWhiteLabelingParams);
  
  WhiteLabelingParams mergeCustomerWhiteLabelingParams(TenantId paramTenantId, WhiteLabelingParams paramWhiteLabelingParams);
  
  void deleteDomainWhiteLabelingByEntityId(TenantId paramTenantId, EntityId paramEntityId);
  
  boolean isWhiteLabelingAllowed(TenantId paramTenantId, EntityId paramEntityId);
  
  boolean isCustomerWhiteLabelingAllowed(TenantId paramTenantId);
}
```