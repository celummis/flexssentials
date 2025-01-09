# Flexssentials SCSS 

This documentation details an overview of the **Flexssentials SCSS** system. It offers a comprehensive approach to styling the essentials (layout, colors, typography and spacing) of an application or website. 

The system uses the SCSS partials  `_variables`, `_functions` and `_mixins` to handle all the styling needs for the following:

* Grid columns
* Utility container sizes
* Flexbox
* Theme colors
* Typography
* Margins and Padding
* Responsive Breakpoints

Focusing on core styles, reduces the need for style adjustments when using third-party component libraries.



### _Variables

1. **Grid column sizes map** (`$column-sizes`):

   * Purpose: define map of grid column sizes based on a 12-column layout.

   * Use: the `column()` function gets the width for the specific number of columns.

   * Structure: key-value pairs where keys are `columns-x` ("x" is the number of columns).
     ```scss
     $column-sizes: (
       columns-12: calc(100% * 12 / 12),
       columns-11: calc(100% * 11 / 12),
       columns-10: calc(100% * 10 / 12),
       columns-9: calc(100% * 9 / 12),
       columns-8: calc(100% * 8 / 12),
       columns-7: calc(100% * 7 / 12),
       columns-6: calc(100% * 6 / 12),
       columns-5: calc(100% * 5 / 12),
       columns-4: calc(100% * 4 / 12),
       columns-3: calc(100% * 3 / 12),
       columns-2: calc(100% * 2 / 12),
       columns-1: calc(100% * 1 / 12)
     );
     ```

2. **Utility sizes map** (`$utility-sizes`):

   * Purpose: provide a range of container width percentages.

   * Use: set container widths using the `container()` function.

   * Structure: key-value pairs where keys are `container-x` ("x" is the width percentage).
     ```scss
     $utility-sizes: (
       container-100: 100%,
       container-95: 95%,
       container-90: 90%,
       container-85: 85%,
       container-80: 80%,
       container-75: 75%,
       container-70: 70%,
       container-65: 65%,
       container-60: 60%,
       container-55: 55%,
       container-50: 50%,
       container-45: 45%,
       container-40: 40%,
       container-35: 35%,
       container-30: 30%,
       container-25: 25%,
       container-20: 20%,
       container-15: 15%,
       container-10: 10%
     );
     ```

3. **Theme colors map** (`$theme-colors`):

   * Purpose: define colors for consistent theming across the application or website.

   * Use: the `apply-color()` function applies the theme color, or lightens and darkens to create a theme color variation.

   * Structure: key-value pairs where keys are theme colors (primary, secondary, etc) and uses positive percentages (60%) to lighten the theme color or negative percentages (-30%) to darken the theme color.
     ```scss
     $theme-colors: (
       primary: #55B785,
       secondary: #808181,
       success: #30DF3E,
       alert: #DF3030,
       warning: #FB8609,
       black: #000000,
       white: #FFFFFF
     );
     ```

4. **Typography sizes map** (`$font-sizes`):

   * Purpose: predefined font sizes for typographic elements.

   * Use: the `apply-typography()` mixin uses the typographic variables (body, h1, h2, h3, h4, h5. h6).

   * Structure: key value pairs where keys are (body, h1, h2, h3, h4, h5. h6).
     ```scss
     $font-sizes: (
       body: 1rem,
       h1: 3rem,
       h2: 2.5rem,
       h3: 2rem,
       h4: 1.75rem,
       h5: 1.5rem, 
       h6: 1.25rem
     );
     ```

5. **Spacing sizes map** (`$spacing-sizes`):

   * Purpose: defines set of standard spacing sizes to use with margin and padding.

   * Use: the `apply-spacing()` mixin applies consistent spacing for any margin or padding used.

   * Structure: key value pairs where keys are size labels (xs, sm, md, etc).
     ```scss
     $spacing-sizes: (
       none: 0,
       xs: 0.25rem,
       sm: 0.5rem,
       md: 1rem,
       lg: 1.5rem,
       xl: 2rem,
       xxl: 3rem
     );
     ```

6. **Breakpoints map** (`$breakpoints`):

   * Purpose: sets media query breakpoints for responsive design.

   * Use: the `responsive()` mixin applies styles for the breakpoint used.

   * Structure: key value pairs where keys are size labels (sm, md, lg, etc).
     ```scss
     $breakpoints: (
       'sm': 'screen and (min-width: 576px)',
       'md': 'screen and (min-width: 768px)',
       'lg': 'screen and (min-width: 992px)',
       'xl': 'screen and (min-width: 1200px)'
     );
     ```

### _Functions

1.  `columns()` function:  

   * Purpose: gets the width for a specified number of columns.

   * Parameters: `$size` - number of columns defined in `$column-sizes`.

   * Returns: the calculated width of the specified columns or warning if invalid size is passed.
     ```scss
     @function columns($size) {
       @if map-has-key($column-sizes, $size) {
         @return map-get($column-sizes, $size);
       } @else {
         @warn "Invalid grid size: `#{$size}`. Please use a valid size.";
         @return null;
       }
     }
     ```

2. `container()` function:

   * Purpose: gets the width for a specified utility sizes.

   * Parameters: `$size` - container size defines in `$utility-sizes`.

   * Returns: the corresponding container width or warning if invalid size is passed.
     ```scss
     @function container($size) {
       @if map-has-key($utility-sizes, $size) {
         @return map-get($utility-sizes, $size);
       } @else {
         @warn "Invalid utility size: `#{$size}`. Please use a valid size.";
         @return null;
       }
     }
     ```

3. `apply-color()` function: 

   * Purpose: adjusts the lightness or darkness of theme colors or uses the theme color as defined.

   * Parameters: `$color-name` - color key from `$theme-colors`, and `$percentage` to lighten or darken color.

   * Returns: the theme color or adjusted theme color, or warning if invalid color variable or color codes passed.
     ```scss
     @function apply-color($color-name, $percentage: null) {
       // Retrieve color from theme colors or use the provided color value
       $color: if(map-has-key($theme-colors, $color-name), map-get($theme-colors, $color-name), $color-name);
     
       // Validate the input color
       @if type-of($color) != 'color' {
         @warn "Invalid color or color name: #{$color}";
         @return null;
       }
     
       // If no percentage is provided, return the original color
       @if $percentage == null or $percentage == 0% {
         @return $color;
       }
     
       // Validate the percentage
       @if type-of($percentage) != 'number' or not unit($percentage) == '%' {
         @warn "Percentage should be a number with a '%' unit, but was #{$percentage}";
         @return null;
       }
     
       // Check if the percentage is within the valid range
       @if $percentage < -100% or $percentage > 100% {
         @warn "Percentage must be between -100% and 100%, but was #{$percentage}";
         @return null;
       }
     
       // Adjust color
       @if $percentage > 0% {
         @return lighten($color, $percentage);
       } @else if $percentage < 0% {
         @return darken($color, abs($percentage));
       }
     
       @return $color;
     }
     ```

4. `apply-spacing()` function:

   * Purpose: gets the spacing sizes used for margins or padding.

   * Parameters: `$size` - spacing size key from `$spacing-sizes`.

   * Returns: the spacing size or warning if invalid size passed.
     ```scss
     @function spacing($size) {
       @if map-has-key($spacing-sizes, $size) {
         @return map-get($spacing-sizes, $size);
       } @else {
         @warn "Invalid spacing size: `#{$size}`. Please use a valid size.";
         @return null;
       }
     }
     ```

### _Mixins

1. `apply-flexbox()`: 

   * Purpose: applies flexbox properties to a container.

   * Parameters: optional flexbox properties for `$direction`, `$justify`, `$items`, `$content` and `$wrap` .
     ```scss
     @mixin apply-flexbox($direction: null, $justify: null, $items: null, $content: null, $wrap: null) {
       display: flex;
       
       @if $direction != null {
         flex-direction: $direction;
       }
       
       @if $justify != null {
         justify-content: $justify;
       }
       
       @if $items != null {
         align-items: $items;
       }
     
       @if $content != null {
         align-content: $content;
       }
       
       @if $wrap != null {
         flex-wrap: $wrap;
       }
     }
     ```

2. `apply-flex-item()`:

   * Purpose: applies flexbox properties to an item within a flexbox container.

   * Parameters: optional flex properties for `$grow`, `$shrink`, `$basis`, `$align-self` and `$order`.
     ```scss
     @mixin apply-flex-item($grow: null, $shrink: null, $basis: null, $align-self: null, $order: null) {
       @if $grow != null {
         flex-grow: $grow;
       }
       
       @if $shrink != null {
         flex-shrink: $shrink;
       }
       
       @if $basis != null {
         flex-basis: $basis;
       }
     
       @if $align-self != null {
         align-self: $align-self;
       }
       
       @if $order != null {
         order: $order;
       }
     }
     ```

3. `apply-typography()`:

   * Purpose: applies typography styles.

   * Parameters: optional typography for `$size`, `$weight`, `$style`, `$underline`, `$capitalize`, `$uppercase` and `$lowercase`.
     ```scss
     @mixin apply-typography($size: null, $weight: null, $style: null, $underline: false, $capitalize: false, $uppercase: false, $lowercase: false) {
       @if $size != null {
         @if map-has-key($font-sizes, $size) {
           font-size: map-get($font-sizes, $size);
         } @else {
           @warn "Invalid font-size key: #{$size}";
         }
       }
     
       @if $weight != null {
         font-weight: $weight;
       }
     
       @if $style != null {
         font-style: $style;
       }
     
       @if $underline {
         text-decoration: underline;
       }
     
       @if $capitalize {
         text-transform: capitalize;
       }
     
       @if $uppercase {
         text-transform: uppercase;
       }
     
       @if $lowercase {
         text-transform: lowercase;
       }
     }
     ```

4. `apply-spacing()`: 

   * Purpose: applies margin or padding.

   * Parameters: `$type`, `$all`, `$vertical`, `$horizontal`, `$top`, `$right`, `$bottom` and `$left`.
     ```scss
     @mixin apply-spacing($type: margin, $all: null, $vertical: null, $horizontal: null, $top: null, $right: null, $bottom: null, $left: null) {
       @if $all != null {
         #{$type}: spacing($all);
       } @else {
         @if $vertical != null {
           #{$type}-top: spacing($vertical);
           #{$type}-bottom: spacing($vertical);
         } @else if $top != null or $bottom != null {
           @if $top != null { #{$type}-top: spacing($top); }
           @if $bottom != null { #{$type}-bottom: spacing($bottom); }
         }
     
         @if $horizontal != null {
           #{$type}-left: spacing($horizontal);
           #{$type}-right: spacing($horizontal);
         } @else if $left != null or $right != null {
           @if $left != null { #{$type}-left: spacing($left); }
           @if $right != null { #{$type}-right: spacing($right); }
         }
       }
     }
     ```

5. `apply-responsive()`: 

   * Purpose: applies responsive styles by screen size breakpoints.

   * Parameters: `$breakpoint` - breakpoint key from `$breakpoints`.
     ```scss
     @mixin apply-responsive($breakpoint) {
       @if map-has-key($breakpoints, $breakpoint) {
         @media #{map-get($breakpoints, $breakpoint)} {
           @content;
         }
       } @else {
         @warn "Invalid breakpoint: `#{$breakpoint}`. Please use a valid breakpoint.";
       }
     }
     ```



### Example using Flexssentials

This is an example of an Angular application component using **Flexssentials SCSS**.

```scss
@import 'variables';
@import 'functions';
@import 'mixins';

.topbar {
  @include apply-flexbox($items: center);
  @include apply-spacing(padding, $all: md);
  background-color: apply-color(primary);

  h1 {
    width: columns(columns-6);
    @include apply-typography(h6);
    color: apply-color(white);
  }

  ul {
    width: columns(columns-6);
    @include apply-flexbox($justify: flex-end);
    gap: 1rem;

    a {
      color: apply-color(white);
    }
  }
}
```

This is the compiled CSS

```css
.topbar {
    display: flex;
    align-items: center;
    padding: 1rem;
    background-color: #55B785;
}

.topbar h1 {
  width: 50%;
  font-size: 1.25rem;
  color: #FFFFFF;
}

.topbar ul {
  width: 50%;
  display: flex;
  justify-content: flex-end;
  gap: 1rem;
}

.topbar ul a {
  color: #FFFFFF;
}
```









 

