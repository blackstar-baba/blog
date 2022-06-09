```
package org.thingsboard.server.common.data.wl;

import org.apache.commons.lang3.StringUtils;

public class Favicon {
  private String url;
  
  private String type;
  
  public void setUrl(String url) {
    this.url = url;
  }
  
  public void setType(String type) {
    this.type = type;
  }
  
  public String toString() {
    return "Favicon(url=" + getUrl() + ", type=" + getType() + ")";
  }
  
  public boolean equals(Object o) {
    if (o == this)
      return true; 
    if (!(o instanceof Favicon))
      return false; 
    Favicon other = (Favicon)o;
    if (!other.canEqual(this))
      return false; 
    Object this$url = getUrl(), other$url = other.getUrl();
    if ((this$url == null) ? (other$url != null) : !this$url.equals(other$url))
      return false; 
    Object this$type = getType(), other$type = other.getType();
    return !((this$type == null) ? (other$type != null) : !this$type.equals(other$type));
  }
  
  protected boolean canEqual(Object other) {
    return other instanceof Favicon;
  }
  
  public int hashCode() {
    int PRIME = 59;
    result = 1;
    Object $url = getUrl();
    result = result * 59 + (($url == null) ? 43 : $url.hashCode());
    Object $type = getType();
    return result * 59 + (($type == null) ? 43 : $type.hashCode());
  }
  
  public String getUrl() {
    return this.url;
  }
  
  public String getType() {
    return this.type;
  }
  
  public Favicon() {}
  
  public Favicon(String url) {
    this(url, extractTypeFromDataUrl(url));
  }
  
  public Favicon(String url, String type) {
    this.url = url;
    this.type = type;
  }
  
  private static String extractTypeFromDataUrl(String dataUrl) {
    String type = null;
    if (!StringUtils.isEmpty(dataUrl)) {
      String[] parts = dataUrl.split(";");
      if (parts != null && parts.length > 0) {
        String part = parts[0];
        String[] typeParts = part.split(":");
        if (typeParts != null && typeParts.length > 1)
          type = typeParts[1]; 
      } 
    } 
    return type;
  }
}
```