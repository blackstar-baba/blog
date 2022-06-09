```
package org.thingsboard.server.common.data.wl;

import org.apache.commons.lang3.StringUtils;

public class PaletteSettings {
  private Palette primaryPalette;
  
  private Palette accentPalette;
  
  public void setPrimaryPalette(Palette primaryPalette) {
    this.primaryPalette = primaryPalette;
  }
  
  public void setAccentPalette(Palette accentPalette) {
    this.accentPalette = accentPalette;
  }
  
  public String toString() {
    return "PaletteSettings(primaryPalette=" + getPrimaryPalette() + ", accentPalette=" + getAccentPalette() + ")";
  }
  
  public boolean equals(Object o) {
    if (o == this)
      return true; 
    if (!(o instanceof PaletteSettings))
      return false; 
    PaletteSettings other = (PaletteSettings)o;
    if (!other.canEqual(this))
      return false; 
    Object this$primaryPalette = getPrimaryPalette(), other$primaryPalette = other.getPrimaryPalette();
    if ((this$primaryPalette == null) ? (other$primaryPalette != null) : !this$primaryPalette.equals(other$primaryPalette))
      return false; 
    Object this$accentPalette = getAccentPalette(), other$accentPalette = other.getAccentPalette();
    return !((this$accentPalette == null) ? (other$accentPalette != null) : !this$accentPalette.equals(other$accentPalette));
  }
  
  protected boolean canEqual(Object other) {
    return other instanceof PaletteSettings;
  }
  
  public int hashCode() {
    int PRIME = 59;
    result = 1;
    Object $primaryPalette = getPrimaryPalette();
    result = result * 59 + (($primaryPalette == null) ? 43 : $primaryPalette.hashCode());
    Object $accentPalette = getAccentPalette();
    return result * 59 + (($accentPalette == null) ? 43 : $accentPalette.hashCode());
  }
  
  public Palette getPrimaryPalette() {
    return this.primaryPalette;
  }
  
  public Palette getAccentPalette() {
    return this.accentPalette;
  }
  
  public PaletteSettings merge(PaletteSettings otherPaletteSettings) {
    if (this.primaryPalette == null || StringUtils.isEmpty(this.primaryPalette.getType()))
      this.primaryPalette = otherPaletteSettings.primaryPalette; 
    if (this.accentPalette == null || StringUtils.isEmpty(this.accentPalette.getType()))
      this.accentPalette = otherPaletteSettings.accentPalette; 
    return this;
  }
}
```