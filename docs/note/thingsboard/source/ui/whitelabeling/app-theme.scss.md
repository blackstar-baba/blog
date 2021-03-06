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
 @import 'datetimepicker-theme';
 @import 'theme-constants';

$primary-extends: $mat-##primary-palette##; // extends
$primary-color-map: (##primary-colors##); // color map

$accent-extends: $mat-##accent-palette##; // extends
$accent-color-map: (##accent-colors##); // color map

$tb-primary-colors: map_merge($primary-extends, $primary-color-map);
$tb-accent-colors: map_merge($accent-extends, $accent-color-map);

$tb-primary: mat-palette($tb-primary-colors);
$tb-accent: mat-palette($tb-accent-colors);

$background: (background: map_get($mat-grey, 200));

$tb-theme-background: map_merge($mat-light-theme-background, $background);

$tb-mat-theme: mat-light-theme(
    $tb-primary,
    $tb-accent
);

$tb-theme: map_merge($tb-mat-theme, (background: $tb-theme-background));

.tb-default {
  @include angular-material-theme($tb-theme);
  @include mat-datetimepicker-theme($tb-theme);
  @include tb-components-theme($tb-theme);
}

.tb-default, .tb-dark {
  .tb-primary-background {
    background-color: mat-color($tb-primary);
  }
}
```