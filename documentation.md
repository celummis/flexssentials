# Flexssentials SCSS 

This documentation details an overview of the **Flexssentials SCSS** system. It offers a comprehensive approach to styling the essentials (layout, colors, typography and spacing) of an application or website. 

## Getting Started

### Installation

To use Flexssentials in your project:

1. First, ensure the `scss` directory containing the framework files is included in your project structure:
   ```
   your-project/
   ├── scss/
   │   ├── framework.scss      (main file that imports all partials)
   │   ├── _variables.scss
   │   ├── _functions.scss
   │   ├── _mixins.scss
   │   └── ...other partials
   ├── styles.scss             (Angular) or App.scss (React)
   └── ...other project files
   ```

2. Then, in your main SCSS file (styles.scss in Angular or App.scss in React), simply add:

   ```scss
   @use 'sass:color';
   @use 'scss/framework.scss' as *; // removes the need to use "framework." before each function
   ```

This imports all the necessary functions and mixins with no namespace prefix, allowing you to use them directly in your stylesheets. The path in the `@use` statement is relative to your project's root stylesheet.

If your project structure differs, adjust the path to match the location of the scss directory, for example:
```scss
@use 'sass:color';
@use './assets/scss/framework.scss' as *;
```

## Core Features

The framework handles all the styling needs for the following:

* Grid columns
* Utility container sizes
* Flexbox
* Theme colors
* Typography
* Margins and Padding
* Responsive Breakpoints

Focusing on core styles reduces the need for style adjustments when using third-party component libraries.

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

This is an example of using **Flexssentials SCSS** in a component:

```scss
@use 'sass:color';
@use 'scss/framework.scss' as *;

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

This is the compiled CSS:

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

## Best Practices Review

This SCSS framework follows modern best practices:

1. **Modular Design**: Separates variables, functions, and mixins for better organization
2. **Configuration via Maps**: Uses Sass maps for easy configuration of colors, spacing, and breakpoints
3. **Consistent Naming**: Clear naming conventions with prefixes like `apply-` for mixins
4. **Optional Parameters**: Most mixins have null defaults for maximum flexibility
5. **Error Handling**: Provides warnings when invalid parameters are passed
6. **Modern Sass Features**: Uses the @use syntax instead of the deprecated @import
7. **Extensible**: Easy to customize for specific project needs

### Recommendations for Improvement

For even better flexibility, consider:

1. Using CSS custom properties (variables) alongside SCSS for runtime theming changes
2. Adding more utility functions for common tasks (e.g., border-radius, transitions)
3. Creating a configuration file that can be overridden by projects
4. Adding more shorthand mixins for common patterns

## Usage Examples

Below are comprehensive examples of how to use the Flexssentials SCSS framework for common UI patterns.

### Grid Layout Example

Creating a responsive grid layout with equal-width columns that stack on mobile:

```scss
.grid-container {
  @include apply-flexbox($wrap: wrap);
  gap: spacing(md);
  
  .grid-item {
    width: columns(columns-12);
    
    @include apply-responsive(md) {
      width: columns(columns-6);
    }
    
    @include apply-responsive(lg) {
      width: columns(columns-4);
    }
  }
}
```

### Container Widths Example

Creating different container widths for different sections:

```scss
.page-container {
  width: container(container-100);
  @include apply-spacing(padding, $horizontal: md);
  
  .content-area {
    width: container(container-80);
    margin: 0 auto;
    
    @include apply-responsive(lg) {
      width: container(container-70);
    }
  }
  
  .sidebar {
    width: container(container-20);
    
    @include apply-responsive(sm) {
      width: container(container-30);
    }
  }
}
```

### Flexbox Patterns Example

Creating a card layout with flexbox:

```scss
.card {
  @include apply-flexbox($direction: column);
  border: 1px solid apply-color(secondary, 30%);
  border-radius: 4px;
  overflow: hidden;
  
  .card-header {
    @include apply-flexbox($justify: space-between, $items: center);
    @include apply-spacing(padding, $all: md);
    background-color: apply-color(primary, 10%);
    
    h3 {
      @include apply-typography($size: h6, $weight: 600);
      margin: 0;
    }
  }
  
  .card-body {
    @include apply-spacing(padding, $all: md);
    @include apply-flex-item($grow: 1);
  }
  
  .card-footer {
    @include apply-flexbox($justify: flex-end, $items: center);
    @include apply-spacing(padding, $all: md);
    background-color: apply-color(secondary, 90%);
    gap: spacing(sm);
  }
}
```

### Theme Colors Example

Using theme colors with variations:

```scss
.alerts-container {
  .alert {
    @include apply-spacing(padding, $all: md);
    border-radius: 4px;
    
    &.alert-primary {
      background-color: apply-color(primary, 80%);
      border: 1px solid apply-color(primary);
      color: apply-color(primary, -50%);
    }
    
    &.alert-warning {
      background-color: apply-color(warning, 85%);
      border: 1px solid apply-color(warning);
      color: apply-color(warning, -40%);
    }
    
    &.alert-alert {
      background-color: apply-color(alert, 90%);
      border: 1px solid apply-color(alert);
      color: apply-color(alert, -30%);
    }
  }
}
```

### Typography Example

Applying typography styles to different elements:

```scss
.article {
  .article-title {
    @include apply-typography($size: h2, $weight: 700);
    @include apply-spacing(margin, $bottom: lg);
    color: apply-color(primary, -20%);
  }
  
  .article-metadata {
    @include apply-typography($size: body, $style: italic);
    @include apply-spacing(margin, $bottom: md);
    color: apply-color(secondary);
  }
  
  .article-summary {
    @include apply-typography($weight: 600);
    @include apply-spacing(margin, $bottom: lg);
  }
  
  .article-content {
    h3 {
      @include apply-typography($size: h4, $weight: 600);
      @include apply-spacing(margin, $top: xl, $bottom: md);
    }
    
    p {
      @include apply-spacing(margin, $bottom: md);
      line-height: 1.6;
    }
    
    .highlight {
      @include apply-typography($weight: 700, $uppercase: true);
      color: apply-color(primary);
    }
  }
}
```

### Spacing Example

Applying consistent spacing:

```scss
.form-group {
  @include apply-spacing(margin, $bottom: lg);
  
  label {
    @include apply-spacing(margin, $bottom: xs);
    display: block;
    @include apply-typography($weight: 600);
  }
  
  input, select, textarea {
    width: 100%;
    @include apply-spacing(padding, $vertical: sm, $horizontal: md);
    border: 1px solid apply-color(secondary, 60%);
    border-radius: 4px;
    
    &:focus {
      outline: none;
      border-color: apply-color(primary);
    }
  }
  
  .helper-text {
    @include apply-spacing(margin, $top: xs);
    @include apply-typography($size: 0.875rem);
    color: apply-color(secondary, 20%);
  }
}
```

### Responsive Design Example

Creating a responsive navigation menu:

```scss
.navbar {
  @include apply-flexbox($justify: space-between, $items: center);
  @include apply-spacing(padding, $all: md);
  background-color: apply-color(primary);
  
  .logo {
    @include apply-typography($size: h5, $weight: 700);
    color: apply-color(white);
  }
  
  .nav-links {
    // Mobile-first: hidden by default
    display: none;
    
    @include apply-responsive(md) {
      @include apply-flexbox($items: center);
      gap: spacing(lg);
    }
    
    a {
      color: apply-color(white, 20%);
      text-decoration: none;
      
      &:hover, &.active {
        color: apply-color(white);
      }
    }
  }
  
  .mobile-toggle {
    @include apply-spacing(padding, $all: sm);
    background: none;
    border: none;
    color: apply-color(white);
    
    @include apply-responsive(md) {
      display: none;
    }
  }
  
  &.open .nav-links {
    @include apply-flexbox($direction: column);
    position: absolute;
    top: 60px;
    left: 0;
    right: 0;
    background-color: apply-color(primary, -10%);
    @include apply-spacing(padding, $vertical: md);
    
    a {
      @include apply-spacing(padding, $vertical: sm, $horizontal: md);
      width: 100%;
      text-align: center;
      
      &:hover {
        background-color: apply-color(primary, -20%);
      }
    }
  }
}
```

### Combining Features Example

This example showcases a complete product card combining multiple Flexssentials features:

```scss
.product-card {
  @include apply-flexbox($direction: column);
  width: columns(columns-12);
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  
  @include apply-responsive(sm) {
    width: columns(columns-6);
  }
  
  @include apply-responsive(lg) {
    width: columns(columns-4);
  }
  
  &:hover {
    transform: translateY(-5px);
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
  }
  
  .product-image {
    height: 200px;
    overflow: hidden;
    
    img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: transform 0.5s ease;
      
      &:hover {
        transform: scale(1.05);
      }
    }
  }
  
  .product-info {
    @include apply-flexbox($direction: column);
    @include apply-flex-item($grow: 1);
    @include apply-spacing(padding, $all: md);
  }
  
  .product-category {
    @include apply-typography($size: 0.875rem, $uppercase: true);
    @include apply-spacing(margin, $bottom: xs);
    color: apply-color(secondary);
  }
  
  .product-name {
    @include apply-typography($size: h5, $weight: 600);
    @include apply-spacing(margin, $bottom: sm);
    color: apply-color(black);
  }
  
  .product-description {
    @include apply-typography($size: 0.9375rem);
    @include apply-spacing(margin, $bottom: md);
    color: apply-color(secondary, -30%);
    line-height: 1.5;
  }
  
  .product-footer {
    @include apply-flexbox($justify: space-between, $items: center);
    @include apply-spacing(margin, $top: auto);
    
    .product-price {
      @include apply-typography($size: h6, $weight: 700);
      color: apply-color(primary, -20%);
    }
    
    .add-to-cart {
      @include apply-spacing(padding, $vertical: sm, $horizontal: md);
      background-color: apply-color(primary);
      color: apply-color(white);
      border: none;
      border-radius: 4px;
      cursor: pointer;
      transition: background-color 0.3s ease;
      
      &:hover {
        background-color: apply-color(primary, -15%);
      }
    }
  }
  
  .product-badge {
    position: absolute;
    top: spacing(sm);
    right: spacing(sm);
    @include apply-spacing(padding, $vertical: xs, $horizontal: sm);
    @include apply-typography($size: 0.75rem, $weight: 700, $uppercase: true);
    background-color: apply-color(success);
    color: apply-color(white);
    border-radius: 4px;
  }
}
```

## Improved Framework.scss Structure

Below is an enhanced version of `framework.scss` that implements several best practices for better flexibility, maintainability, and feature coverage:

```scss
// framework.scss - Main entry point for Flexssentials SCSS framework

// Import Sass modules
@use 'sass:color';
@use 'sass:map';
@use 'sass:math';

// Import configuration with defaults that can be overridden
@use 'config' as config;

// Forward all partials to make their members available when importing framework
// This allows users to access all functionality with a single import
@forward 'variables' with (
  // Merge default variable maps with any custom configuration
  $theme-colors: map.merge(config.$theme-colors-default, config.$theme-colors-custom),
  $spacing-sizes: map.merge(config.$spacing-sizes-default, config.$spacing-sizes-custom),
  $breakpoints: map.merge(config.$breakpoints-default, config.$breakpoints-custom),
  $font-sizes: map.merge(config.$font-sizes-default, config.$font-sizes-custom),
  $column-sizes: map.merge(config.$column-sizes-default, config.$column-sizes-custom),
  $utility-sizes: map.merge(config.$utility-sizes-default, config.$utility-sizes-custom)
);
@forward 'functions';
@forward 'mixins';
@forward 'reset';
@forward 'utilities';
@forward 'print';
@forward 'accessibility';
@forward 'dark-mode';

// This enables the CSS reset by default, but can be turned off
@if config.$enable-reset {
  @include reset.base-reset();
}

// Generate utility classes if enabled
@if config.$enable-utilities {
  @include utilities.generate();
}
```

### Explanation of Improvements

1. **Sass Modules**:
   - Added additional Sass built-in modules like `map` and `math` to enable more sophisticated operations
   - Rationale: Provides more powerful tools for manipulating and calculating values

2. **Configuration System**:
   - Created a dedicated `_config.scss` file with default settings that can be overridden
   - Rationale: Allows developers to customize the framework without modifying the source files, making updates easier

3. **@forward Instead of @use**:
   - Changed from `@use` to `@forward` for partials
   - Rationale: Makes all the members of the partials available when importing framework.scss, keeping the DRY principle

4. **Map Merging for Customization**:
   - Implemented map.merge to combine default values with custom configurations
   - Rationale: Provides an elegant way to override only specific values while keeping the rest of the defaults

5. **Additional Functional Modules**:
   - Added new modules for reset, utilities, print styles, accessibility, and dark mode
   - Rationale:
     - `_reset.scss`: Ensures consistent rendering across different browsers
     - `_utilities.scss`: Provides common helper classes to reduce repetitive CSS
     - `_print.scss`: Optimizes layouts for printing
     - `_accessibility.scss`: Improves user experience for people with disabilities
     - `_dark-mode.scss`: Supports modern dark/light theme preferences

6. **Feature Toggles**:
   - Added conditional compilation with `@if` statements
   - Rationale: Allows developers to opt out of features they don't need, reducing CSS bloat

### Example Config File Structure

```scss
// _config.scss - Configuration file for Flexssentials

// Feature toggles
$enable-reset: true !default;
$enable-utilities: true !default;
$enable-print-styles: true !default;
$enable-dark-mode: true !default;

// Default values
$theme-colors-default: (
  primary: #55B785,
  secondary: #808181,
  success: #30DF3E,
  alert: #DF3030,
  warning: #FB8609,
  black: #000000,
  white: #FFFFFF
);

// Custom overrides (empty by default, to be customized by users)
$theme-colors-custom: () !default;

// Similar patterns for other variable maps
$spacing-sizes-default: (
  none: 0,
  xs: 0.25rem,
  sm: 0.5rem,
  md: 1rem,
  lg: 1.5rem,
  xl: 2rem,
  xxl: 3rem
);
$spacing-sizes-custom: () !default;

$breakpoints-default: (
  'sm': 'screen and (min-width: 576px)',
  'md': 'screen and (min-width: 768px)',
  'lg': 'screen and (min-width: 992px)',
  'xl': 'screen and (min-width: 1200px)'
);
$breakpoints-custom: () !default;

$font-sizes-default: (
  body: 1rem,
  h1: 3rem,
  h2: 2.5rem,
  h3: 2rem,
  h4: 1.75rem,
  h5: 1.5rem, 
  h6: 1.25rem
);
$font-sizes-custom: () !default;

// ... other default and custom configuration maps
```

### How to Customize the Framework

With this enhanced structure, developers can easily customize the framework by creating their own config file before importing framework.scss:

```scss
// custom-config.scss
@use 'scss/config' with (
  $theme-colors-custom: (
    primary: #3E8ED0,
    secondary: #64748B
  ),
  $enable-utilities: false
);

// main.scss
@use 'custom-config';
@use 'scss/framework' as *;

// Your custom styles...
```

This provides a powerful, flexible system that maintains all the benefits of Flexssentials while allowing for extensive customization.









 

