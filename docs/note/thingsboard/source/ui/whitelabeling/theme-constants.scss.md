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

$mat-tb-primary: map_merge($mat-teal, (
  500: map-get($mat-teal, 800)
));

$mat-tb-accent: map_merge($mat-deep-orange, (
));

@mixin mat-fab-toolbar-theme($theme) {
  $primary: map-get($theme, primary);
  $accent: map-get($theme, accent);
  $warn: map-get($theme, warn);
  $background: map-get($theme, background);
  $foreground: map-get($theme, foreground);

  mat-fab-toolbar {
    .mat-fab-toolbar-background {
      background: mat-color($background, app-bar);
      color: mat-color($foreground, text);
    }
    &.mat-primary {
      .mat-fab-toolbar-background {
        @include _mat-toolbar-color($primary);
      }
    }
    &.mat-accent {
      .mat-fab-toolbar-background {
        @include _mat-toolbar-color($accent);
      }
    }
    &.mat-warn {
      .mat-fab-toolbar-background {
        @include _mat-toolbar-color($warn);
      }
    }
  }
}

@mixin _mat-toolbar-inverse-color($palette) {
  background: #fff;
  color: $dark-primary-text;
}

@mixin mat-fab-toolbar-inverse-theme($theme) {
  $primary: map-get($theme, primary);
  $accent: map-get($theme, accent);
  $warn: map-get($theme, warn);
  $background: map-get($theme, foreground);
  $foreground: map-get($theme, background);

  mat-fab-toolbar {
    .mat-fab-toolbar-background {
      background: #fff;
      color: mat-color($foreground, text);
    }
    &.mat-primary {
      .mat-fab-toolbar-background {
        @include _mat-toolbar-inverse-color($primary);
      }
    }
    mat-toolbar {
      &.mat-primary {
        @include _mat-toolbar-inverse-color($primary);
        button.mat-icon-button {
          mat-icon {
            color: mat-color($primary);
          }
        }
      }
    }
    .mat-fab {
      &.mat-primary {
        background: #fff;
        color: mat-color($primary);
      }
    }
  }

}

@mixin tb-components-theme($theme) {
  $primary: map-get($theme, primary);

  mat-toolbar{
    &.mat-hue-3 {
      background-color: mat-color($primary, 'A100');
    }
  }

  @include mat-fab-toolbar-theme($theme);

  div.tb-dashboard-page.mobile-app {
    @include mat-fab-toolbar-inverse-theme($tb-theme);
  }
}
```