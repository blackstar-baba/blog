```
package org.thingsboard.server.common.data.wl;

import com.fasterxml.jackson.annotation.JsonProperty;
import java.util.Map;

public class Palette {
  private String type;
  
  @JsonProperty("extends")
  private String extendsPalette;
  
  private Map<String, String> colors;
  
  public void setType(String type) {
    this.type = type;
  }
  
  @JsonProperty("extends")
  public void setExtendsPalette(String extendsPalette) {
    this.extendsPalette = extendsPalette;
  }
  
  public void setColors(Map<String, String> colors) {
    this.colors = colors;
  }
  
  public String toString() {
    return "Palette(type=" + getType() + ", extendsPalette=" + getExtendsPalette() + ", colors=" + getColors() + ")";
  }
  
  public boolean equals(Object o) {
    if (o == this)
      return true; 
    if (!(o instanceof Palette))
      return false; 
    Palette other = (Palette)o;
    if (!other.canEqual(this))
      return false; 
    Object this$type = getType(), other$type = other.getType();
    if ((this$type == null) ? (other$type != null) : !this$type.equals(other$type))
      return false; 
    Object this$extendsPalette = getExtendsPalette(), other$extendsPalette = other.getExtendsPalette();
    if ((this$extendsPalette == null) ? (other$extendsPalette != null) : !this$extendsPalette.equals(other$extendsPalette))
      return false; 
    Object<String, String> this$colors = (Object<String, String>)getColors(), other$colors = (Object<String, String>)other.getColors();
    return !((this$colors == null) ? (other$colors != null) : !this$colors.equals(other$colors));
  }
  
  protected boolean canEqual(Object other) {
    return other instanceof Palette;
  }
  
  public int hashCode() {
    int PRIME = 59;
    result = 1;
    Object $type = getType();
    result = result * 59 + (($type == null) ? 43 : $type.hashCode());
    Object $extendsPalette = getExtendsPalette();
    result = result * 59 + (($extendsPalette == null) ? 43 : $extendsPalette.hashCode());
    Object<String, String> $colors = (Object<String, String>)getColors();
    return result * 59 + (($colors == null) ? 43 : $colors.hashCode());
  }
  
  public String getType() {
    return this.type;
  }
  
  public String getExtendsPalette() {
    return this.extendsPalette;
  }
  
  public Map<String, String> getColors() {
    return this.colors;
  }
}
```