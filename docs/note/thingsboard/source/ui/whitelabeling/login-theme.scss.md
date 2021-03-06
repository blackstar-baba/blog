```
/**
 * ThingsBoard, Inc. ("COMPANY") CONFIDENTIAL
 *
 * Copyright © 2016-2021 ThingsBoard, Inc. All Rights Reserved.
 *
 * NOTICE: All information contained herein is, and remains
 * the property of ThingsBoard, Inc. and its suppliers,
 * if any.  The intellectual and technical concepts contained
 * herein are proprietary to ThingsBoard, Inc.
 * and its suppliers and may be covered by U.S. and Foreign Patents,
 * patents in process, and are protected by trade secret or copyright law.
 *
 * Dissemination of this information or reproduction of this material is strictly forbidden
 * unless prior written permission is obtained from COMPANY.
 *
 * Access to the source code contained herein is hereby forbidden to anyone except current COMPANY employees,
 * managers or contractors who have executed Confidentiality and Non-disclosure agreements
 * explicitly covering such access.
 *
 * The copyright notice above does not evidence any actual or intended publication
 * or disclosure  of  this source code, which includes
 * information that is confidential and/or proprietary, and is a trade secret, of  COMPANY.
 * ANY REPRODUCTION, MODIFICATION, DISTRIBUTION, PUBLIC  PERFORMANCE,
 * OR PUBLIC DISPLAY OF OR THROUGH USE  OF THIS  SOURCE CODE  WITHOUT
 * THE EXPRESS WRITTEN CONSENT OF COMPANY IS STRICTLY PROHIBITED,
 * AND IN VIOLATION OF APPLICABLE LAWS AND INTERNATIONAL TREATIES.
 * THE RECEIPT OR POSSESSION OF THIS SOURCE CODE AND/OR RELATED INFORMATION
 * DOES NOT CONVEY OR IMPLY ANY RIGHTS TO REPRODUCE, DISCLOSE OR DISTRIBUTE ITS CONTENTS,
 * OR TO MANUFACTURE, USE, OR SELL ANYTHING THAT IT  MAY DESCRIBE, IN WHOLE OR IN PART.
 */
 @import '_theming';
 @import 'login-theme-constants';

$primary-extends: $mat-##primary-palette##; // extends
$primary-color-map: (##primary-colors##); // color map

$accent-extends: $mat-##accent-palette##; // extends
$accent-color-map: (##accent-colors##); // color map

$dark-foreground: ##dark-foreground##;

$tb-primary-colors: map_merge($primary-extends, $primary-color-map);
$tb-accent-colors: map_merge($accent-extends, $accent-color-map);

$backgroundContrastColor: map-get($tb-primary-colors, if($dark-foreground, 900, 200));
$backgroundColor: map-get($tb-primary-colors, 500);

$tb-primary-palette-colors: map_merge($tb-primary-colors, (
  500: $backgroundContrastColor
));

$tb-background-palette-colors: map_merge($tb-primary-palette-colors, (
  800: $backgroundColor
));

$tb-primary: mat-palette($tb-primary-palette-colors);
$tb-accent: mat-palette($tb-accent-colors);

$tb-dark-theme-background: (
  status-bar: black,
  app-bar:    map_get($tb-background-palette-colors, 900),
  background: map_get($tb-background-palette-colors, 800),
  hover:      rgba(white, 0.04),
  card:       map_get($tb-background-palette-colors, 800),
  dialog:     map_get($tb-background-palette-colors, 800),
  disabled-button: rgba(white, 0.12),
  raised-button: map-get($tb-background-palette-colors, 50),
  focused-button: $light-focused,
  selected-button: map_get($tb-background-palette-colors, 900),
  selected-disabled-button: map_get($tb-background-palette-colors, 800),
  disabled-button-toggle: black,
  unselected-chip: map_get($tb-background-palette-colors, 700),
  disabled-list-option: black,
  tooltip: map_get($mat-grey, 700),
);

@function get-tb-dark-theme($primary, $accent, $dark-foreground: false, $warn: mat-palette($mat-red)) {
  @return (
    primary: $primary,
    accent: $accent,
    warn: $warn,
    is-dark: true,
    foreground: if($dark-foreground, $mat-light-theme-foreground, $mat-dark-theme-foreground),
    background: $tb-dark-theme-background,
  );
}

$tb-theme: get-tb-dark-theme(
    $tb-primary,
    $tb-accent,
    $dark-foreground
);

.tb-dark {
  @include angular-material-theme($tb-theme);
}
```