```
package org.thingsboard.server.common.data.wl;

import org.apache.commons.lang3.StringUtils;

public class WhiteLabelingParams {
  protected String logoImageUrl;
  
  protected String logoImageChecksum;
  
  protected Integer logoImageHeight;
  
  protected String appTitle;
  
  protected Favicon favicon;
  
  protected String faviconChecksum;
  
  protected PaletteSettings paletteSettings;
  
  protected String helpLinkBaseUrl;
  
  protected Boolean enableHelpLinks;
  
  public void setLogoImageUrl(String logoImageUrl) {
    this.logoImageUrl = logoImageUrl;
  }
  
  public void setLogoImageChecksum(String logoImageChecksum) {
    this.logoImageChecksum = logoImageChecksum;
  }
  
  public void setLogoImageHeight(Integer logoImageHeight) {
    this.logoImageHeight = logoImageHeight;
  }
  
  public void setAppTitle(String appTitle) {
    this.appTitle = appTitle;
  }
  
  public void setFavicon(Favicon favicon) {
    this.favicon = favicon;
  }
  
  public void setFaviconChecksum(String faviconChecksum) {
    this.faviconChecksum = faviconChecksum;
  }
  
  public void setPaletteSettings(PaletteSettings paletteSettings) {
    this.paletteSettings = paletteSettings;
  }
  
  public void setHelpLinkBaseUrl(String helpLinkBaseUrl) {
    this.helpLinkBaseUrl = helpLinkBaseUrl;
  }
  
  public void setEnableHelpLinks(Boolean enableHelpLinks) {
    this.enableHelpLinks = enableHelpLinks;
  }
  
  public void setWhiteLabelingEnabled(boolean whiteLabelingEnabled) {
    this.whiteLabelingEnabled = whiteLabelingEnabled;
  }
  
  public void setShowNameVersion(Boolean showNameVersion) {
    this.showNameVersion = showNameVersion;
  }
  
  public void setPlatformName(String platformName) {
    this.platformName = platformName;
  }
  
  public void setPlatformVersion(String platformVersion) {
    this.platformVersion = platformVersion;
  }
  
  public void setCustomCss(String customCss) {
    this.customCss = customCss;
  }
  
  public String toString() {
    return "WhiteLabelingParams(logoImageUrl=" + getLogoImageUrl() + ", logoImageChecksum=" + getLogoImageChecksum() + ", logoImageHeight=" + getLogoImageHeight() + ", appTitle=" + getAppTitle() + ", favicon=" + getFavicon() + ", faviconChecksum=" + getFaviconChecksum() + ", paletteSettings=" + getPaletteSettings() + ", helpLinkBaseUrl=" + getHelpLinkBaseUrl() + ", enableHelpLinks=" + getEnableHelpLinks() + ", whiteLabelingEnabled=" + isWhiteLabelingEnabled() + ", showNameVersion=" + getShowNameVersion() + ", platformName=" + getPlatformName() + ", platformVersion=" + getPlatformVersion() + ", customCss=" + getCustomCss() + ")";
  }
  
  public boolean equals(Object o) {
    if (o == this)
      return true; 
    if (!(o instanceof WhiteLabelingParams))
      return false; 
    WhiteLabelingParams other = (WhiteLabelingParams)o;
    if (!other.canEqual(this))
      return false; 
    if (isWhiteLabelingEnabled() != other.isWhiteLabelingEnabled())
      return false; 
    Object this$logoImageHeight = getLogoImageHeight(), other$logoImageHeight = other.getLogoImageHeight();
    if ((this$logoImageHeight == null) ? (other$logoImageHeight != null) : !this$logoImageHeight.equals(other$logoImageHeight))
      return false; 
    Object this$enableHelpLinks = getEnableHelpLinks(), other$enableHelpLinks = other.getEnableHelpLinks();
    if ((this$enableHelpLinks == null) ? (other$enableHelpLinks != null) : !this$enableHelpLinks.equals(other$enableHelpLinks))
      return false; 
    Object this$showNameVersion = getShowNameVersion(), other$showNameVersion = other.getShowNameVersion();
    if ((this$showNameVersion == null) ? (other$showNameVersion != null) : !this$showNameVersion.equals(other$showNameVersion))
      return false; 
    Object this$logoImageUrl = getLogoImageUrl(), other$logoImageUrl = other.getLogoImageUrl();
    if ((this$logoImageUrl == null) ? (other$logoImageUrl != null) : !this$logoImageUrl.equals(other$logoImageUrl))
      return false; 
    Object this$logoImageChecksum = getLogoImageChecksum(), other$logoImageChecksum = other.getLogoImageChecksum();
    if ((this$logoImageChecksum == null) ? (other$logoImageChecksum != null) : !this$logoImageChecksum.equals(other$logoImageChecksum))
      return false; 
    Object this$appTitle = getAppTitle(), other$appTitle = other.getAppTitle();
    if ((this$appTitle == null) ? (other$appTitle != null) : !this$appTitle.equals(other$appTitle))
      return false; 
    Object this$favicon = getFavicon(), other$favicon = other.getFavicon();
    if ((this$favicon == null) ? (other$favicon != null) : !this$favicon.equals(other$favicon))
      return false; 
    Object this$faviconChecksum = getFaviconChecksum(), other$faviconChecksum = other.getFaviconChecksum();
    if ((this$faviconChecksum == null) ? (other$faviconChecksum != null) : !this$faviconChecksum.equals(other$faviconChecksum))
      return false; 
    Object this$paletteSettings = getPaletteSettings(), other$paletteSettings = other.getPaletteSettings();
    if ((this$paletteSettings == null) ? (other$paletteSettings != null) : !this$paletteSettings.equals(other$paletteSettings))
      return false; 
    Object this$helpLinkBaseUrl = getHelpLinkBaseUrl(), other$helpLinkBaseUrl = other.getHelpLinkBaseUrl();
    if ((this$helpLinkBaseUrl == null) ? (other$helpLinkBaseUrl != null) : !this$helpLinkBaseUrl.equals(other$helpLinkBaseUrl))
      return false; 
    Object this$platformName = getPlatformName(), other$platformName = other.getPlatformName();
    if ((this$platformName == null) ? (other$platformName != null) : !this$platformName.equals(other$platformName))
      return false; 
    Object this$platformVersion = getPlatformVersion(), other$platformVersion = other.getPlatformVersion();
    if ((this$platformVersion == null) ? (other$platformVersion != null) : !this$platformVersion.equals(other$platformVersion))
      return false; 
    Object this$customCss = getCustomCss(), other$customCss = other.getCustomCss();
    return !((this$customCss == null) ? (other$customCss != null) : !this$customCss.equals(other$customCss));
  }
  
  protected boolean canEqual(Object other) {
    return other instanceof WhiteLabelingParams;
  }
  
  public int hashCode() {
    int PRIME = 59;
    result = 1;
    result = result * 59 + (isWhiteLabelingEnabled() ? 79 : 97);
    Object $logoImageHeight = getLogoImageHeight();
    result = result * 59 + (($logoImageHeight == null) ? 43 : $logoImageHeight.hashCode());
    Object $enableHelpLinks = getEnableHelpLinks();
    result = result * 59 + (($enableHelpLinks == null) ? 43 : $enableHelpLinks.hashCode());
    Object $showNameVersion = getShowNameVersion();
    result = result * 59 + (($showNameVersion == null) ? 43 : $showNameVersion.hashCode());
    Object $logoImageUrl = getLogoImageUrl();
    result = result * 59 + (($logoImageUrl == null) ? 43 : $logoImageUrl.hashCode());
    Object $logoImageChecksum = getLogoImageChecksum();
    result = result * 59 + (($logoImageChecksum == null) ? 43 : $logoImageChecksum.hashCode());
    Object $appTitle = getAppTitle();
    result = result * 59 + (($appTitle == null) ? 43 : $appTitle.hashCode());
    Object $favicon = getFavicon();
    result = result * 59 + (($favicon == null) ? 43 : $favicon.hashCode());
    Object $faviconChecksum = getFaviconChecksum();
    result = result * 59 + (($faviconChecksum == null) ? 43 : $faviconChecksum.hashCode());
    Object $paletteSettings = getPaletteSettings();
    result = result * 59 + (($paletteSettings == null) ? 43 : $paletteSettings.hashCode());
    Object $helpLinkBaseUrl = getHelpLinkBaseUrl();
    result = result * 59 + (($helpLinkBaseUrl == null) ? 43 : $helpLinkBaseUrl.hashCode());
    Object $platformName = getPlatformName();
    result = result * 59 + (($platformName == null) ? 43 : $platformName.hashCode());
    Object $platformVersion = getPlatformVersion();
    result = result * 59 + (($platformVersion == null) ? 43 : $platformVersion.hashCode());
    Object $customCss = getCustomCss();
    return result * 59 + (($customCss == null) ? 43 : $customCss.hashCode());
  }
  
  public String getLogoImageUrl() {
    return this.logoImageUrl;
  }
  
  public String getLogoImageChecksum() {
    return this.logoImageChecksum;
  }
  
  public Integer getLogoImageHeight() {
    return this.logoImageHeight;
  }
  
  public String getAppTitle() {
    return this.appTitle;
  }
  
  public Favicon getFavicon() {
    return this.favicon;
  }
  
  public String getFaviconChecksum() {
    return this.faviconChecksum;
  }
  
  public PaletteSettings getPaletteSettings() {
    return this.paletteSettings;
  }
  
  public String getHelpLinkBaseUrl() {
    return this.helpLinkBaseUrl;
  }
  
  public Boolean getEnableHelpLinks() {
    return this.enableHelpLinks;
  }
  
  protected boolean whiteLabelingEnabled = true;
  
  protected Boolean showNameVersion;
  
  protected String platformName;
  
  protected String platformVersion;
  
  protected String customCss;
  
  public boolean isWhiteLabelingEnabled() {
    return this.whiteLabelingEnabled;
  }
  
  public Boolean getShowNameVersion() {
    return this.showNameVersion;
  }
  
  public String getPlatformName() {
    return this.platformName;
  }
  
  public String getPlatformVersion() {
    return this.platformVersion;
  }
  
  public String getCustomCss() {
    return this.customCss;
  }
  
  public WhiteLabelingParams merge(WhiteLabelingParams otherWlParams) {
    if (StringUtils.isEmpty(this.logoImageUrl)) {
      this.logoImageUrl = otherWlParams.logoImageUrl;
      this.logoImageChecksum = otherWlParams.logoImageChecksum;
    } 
    if (this.logoImageHeight == null)
      this.logoImageHeight = otherWlParams.logoImageHeight; 
    if (StringUtils.isEmpty(this.appTitle))
      this.appTitle = otherWlParams.appTitle; 
    if (this.favicon == null || StringUtils.isEmpty(this.favicon.getUrl())) {
      this.favicon = otherWlParams.favicon;
      this.faviconChecksum = otherWlParams.faviconChecksum;
    } 
    if (this.paletteSettings == null) {
      this.paletteSettings = otherWlParams.paletteSettings;
    } else if (otherWlParams.paletteSettings != null) {
      this.paletteSettings.merge(otherWlParams.paletteSettings);
    } 
    if (otherWlParams.helpLinkBaseUrl != null)
      this.helpLinkBaseUrl = otherWlParams.helpLinkBaseUrl; 
    if (otherWlParams.enableHelpLinks != null)
      this.enableHelpLinks = otherWlParams.enableHelpLinks; 
    if (this.showNameVersion == null) {
      this.showNameVersion = otherWlParams.showNameVersion;
      this.platformName = otherWlParams.platformName;
      this.platformVersion = otherWlParams.platformVersion;
    } 
    if (!StringUtils.isEmpty(otherWlParams.customCss))
      if (StringUtils.isEmpty(this.customCss)) {
        this.customCss = otherWlParams.customCss;
      } else {
        this.customCss = otherWlParams.customCss + "\n" + otherWlParams.customCss;
      }  
    return this;
  }
  
  public void prepareImages(String logoImageChecksum, String faviconChecksum) {
    if (!StringUtils.isEmpty(logoImageChecksum) && logoImageChecksum.equals(this.logoImageChecksum))
      this.logoImageUrl = null; 
    if (!StringUtils.isEmpty(faviconChecksum) && faviconChecksum.equals(this.faviconChecksum))
      this.favicon = null; 
  }
}
```