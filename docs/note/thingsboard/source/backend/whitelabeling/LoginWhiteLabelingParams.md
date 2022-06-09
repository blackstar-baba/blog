```
package org.thingsboard.server.common.data.wl;

public class LoginWhiteLabelingParams extends WhiteLabelingParams {
  private String pageBackgroundColor;
  
  private boolean darkForeground;
  
  private String domainName;
  
  private String baseUrl;
  
  private boolean prohibitDifferentUrl;
  
  private String adminSettingsId;
  
  private Boolean showNameBottom;
  
  public void setPageBackgroundColor(String pageBackgroundColor) {
    this.pageBackgroundColor = pageBackgroundColor;
  }
  
  public void setDarkForeground(boolean darkForeground) {
    this.darkForeground = darkForeground;
  }
  
  public void setDomainName(String domainName) {
    this.domainName = domainName;
  }
  
  public void setBaseUrl(String baseUrl) {
    this.baseUrl = baseUrl;
  }
  
  public void setProhibitDifferentUrl(boolean prohibitDifferentUrl) {
    this.prohibitDifferentUrl = prohibitDifferentUrl;
  }
  
  public void setAdminSettingsId(String adminSettingsId) {
    this.adminSettingsId = adminSettingsId;
  }
  
  public void setShowNameBottom(Boolean showNameBottom) {
    this.showNameBottom = showNameBottom;
  }
  
  public String toString() {
    return "LoginWhiteLabelingParams(pageBackgroundColor=" + getPageBackgroundColor() + ", darkForeground=" + isDarkForeground() + ", domainName=" + getDomainName() + ", baseUrl=" + getBaseUrl() + ", prohibitDifferentUrl=" + isProhibitDifferentUrl() + ", adminSettingsId=" + getAdminSettingsId() + ", showNameBottom=" + getShowNameBottom() + ")";
  }
  
  public boolean equals(Object o) {
    if (o == this)
      return true; 
    if (!(o instanceof LoginWhiteLabelingParams))
      return false; 
    LoginWhiteLabelingParams other = (LoginWhiteLabelingParams)o;
    if (!other.canEqual(this))
      return false; 
    if (isDarkForeground() != other.isDarkForeground())
      return false; 
    if (isProhibitDifferentUrl() != other.isProhibitDifferentUrl())
      return false; 
    Object this$showNameBottom = getShowNameBottom(), other$showNameBottom = other.getShowNameBottom();
    if ((this$showNameBottom == null) ? (other$showNameBottom != null) : !this$showNameBottom.equals(other$showNameBottom))
      return false; 
    Object this$pageBackgroundColor = getPageBackgroundColor(), other$pageBackgroundColor = other.getPageBackgroundColor();
    if ((this$pageBackgroundColor == null) ? (other$pageBackgroundColor != null) : !this$pageBackgroundColor.equals(other$pageBackgroundColor))
      return false; 
    Object this$domainName = getDomainName(), other$domainName = other.getDomainName();
    if ((this$domainName == null) ? (other$domainName != null) : !this$domainName.equals(other$domainName))
      return false; 
    Object this$baseUrl = getBaseUrl(), other$baseUrl = other.getBaseUrl();
    if ((this$baseUrl == null) ? (other$baseUrl != null) : !this$baseUrl.equals(other$baseUrl))
      return false; 
    Object this$adminSettingsId = getAdminSettingsId(), other$adminSettingsId = other.getAdminSettingsId();
    return !((this$adminSettingsId == null) ? (other$adminSettingsId != null) : !this$adminSettingsId.equals(other$adminSettingsId));
  }
  
  protected boolean canEqual(Object other) {
    return other instanceof LoginWhiteLabelingParams;
  }
  
  public int hashCode() {
    int PRIME = 59;
    result = 1;
    result = result * 59 + (isDarkForeground() ? 79 : 97);
    result = result * 59 + (isProhibitDifferentUrl() ? 79 : 97);
    Object $showNameBottom = getShowNameBottom();
    result = result * 59 + (($showNameBottom == null) ? 43 : $showNameBottom.hashCode());
    Object $pageBackgroundColor = getPageBackgroundColor();
    result = result * 59 + (($pageBackgroundColor == null) ? 43 : $pageBackgroundColor.hashCode());
    Object $domainName = getDomainName();
    result = result * 59 + (($domainName == null) ? 43 : $domainName.hashCode());
    Object $baseUrl = getBaseUrl();
    result = result * 59 + (($baseUrl == null) ? 43 : $baseUrl.hashCode());
    Object $adminSettingsId = getAdminSettingsId();
    return result * 59 + (($adminSettingsId == null) ? 43 : $adminSettingsId.hashCode());
  }
  
  public String getPageBackgroundColor() {
    return this.pageBackgroundColor;
  }
  
  public boolean isDarkForeground() {
    return this.darkForeground;
  }
  
  public String getDomainName() {
    return this.domainName;
  }
  
  public String getBaseUrl() {
    return this.baseUrl;
  }
  
  public boolean isProhibitDifferentUrl() {
    return this.prohibitDifferentUrl;
  }
  
  public String getAdminSettingsId() {
    return this.adminSettingsId;
  }
  
  public Boolean getShowNameBottom() {
    return this.showNameBottom;
  }
  
  public LoginWhiteLabelingParams merge(LoginWhiteLabelingParams otherWlParams) {
    Integer prevLogoImageHeight = this.logoImageHeight;
    merge(otherWlParams);
    if (prevLogoImageHeight == null)
      this.logoImageHeight = null; 
    if (this.showNameBottom == null)
      this.showNameBottom = otherWlParams.showNameBottom; 
    return this;
  }
}
```