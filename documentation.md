# Flexssentials SCSS 

This documentation details an overview of the **Flexssentials SCSS** system. It offers a simple and lightweight approach to styling essentials: colors, layout, typography, and spacing.

## Getting Started

### Installation

To use Flexssentials in your project:

1. First, ensure the `scss` directory containing the framework file is included in your project structure:
   ```
   your-project/
   ├── scss/
   │   ├── framework.scss      (the main and only framework file)
   │   └── _reset.scss         (optional reset styles)
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

* Colors and theming
* Flexbox layouts
* Spacing (margins and padding)
* Typography
* Responsive design

### SCSS Maps

Flexssentials uses SCSS maps as the single source of truth for all variables, which are then automatically converted to CSS custom properties (variables).

1. **Colors** (`$colors`):
   ```scss
   $colors: (
     primary: #55B785,
     secondary: #808181,
     info: #0099FF,
     success: #30DF3E,
     warning: #FB8609,
     danger: #DF3030,
     text: #121212,
     white: #FFFFFF,
     black: #000000
   );
   ```

2. **Grid columns** (`$columns`):
   ```scss
   $columns: (
     col-12: 100%,
     col-11: 91.6666666667%,
     col-10: 83.3333333333%,
     col-9: 75%,
     col-8: 66.6666666667%,
     col-7: 58.3333333333%,
     col-6: 50%,
     col-5: 41.6666666667%,
     col-4: 33.3333333333%,
     col-3: 25%,
     col-2: 16.6666666667%,
     col-1: 8.3333333333%
   );
   ```

3. **Spacing** (`$spacing`):
   ```scss
   $spacing: (
     none: 0,
     xs: 0.25rem,
     sm: 0.5rem,
     md: 1rem,
     lg: 1.5rem,
     xl: 2rem,
     xxl: 3rem
   );
   ```

4. **Utility sizes** (`$sizes`):
   ```scss
   $sizes: (
     w-100: 100%,
     w-95: 95%,
     w-90: 90%,
     /* ...more size entries... */
     w-15: 15%,
     w-10: 10%
   );
   ```

5. **Font sizes** (`$font-sizes`):
   ```scss
   $font-sizes: (
     footnote: 0.75rem,
     table: 0.85rem,
     label: 0.9rem,
     body: 1rem,
     h1: 3rem,
     h2: 2.5rem,
     h3: 2rem,
     h4: 1.75rem,
     h5: 1.5rem,
     h6: 1.25rem
   );
   ```

6. **Breakpoints** (`$breakpoints`):
   ```scss
   $breakpoints: (
     xs: 576px,
     sm: 768px,
     md: 992px,
     lg: 1200px,
     xl: 1400px
   );
   ```

### CSS Variables

All SCSS maps are automatically converted to CSS variables using `@each` loops:

```scss
:root {
  // Theme Colors 
  @each $name, $value in $colors {
    --#{$name}: #{$value};
  }

  // Grid Columns
  @each $name, $value in $columns {
    --#{$name}: #{$value};
  }

  // And so on for all other variable categories...
}
```

### Functions

1. `column($col)`:
   * Returns a CSS variable for column width
   * Example: `column(col-6)` → `var(--col-6)` (50%)

2. `container($size)`:
   * Returns a CSS variable for container width
   * Example: `container(w-75)` → `var(--w-75)` (75%)

3. `spacing($size)`:
   * Returns a CSS variable for spacing
   * Example: `spacing(md)` → `var(--md)` (1rem)

4. `color($color-name)`:
   * Returns a CSS variable for color
   * Example: `color(primary)` → `var(--primary)` (#55B785)

5. `color-value($color-name, $percentage)`:
   * Returns an actual color value with optional lightness adjustment
   * Examples:
     * `color-value(primary)` → The primary color as is
     * `color-value(primary, 20%)` → Primary color lightened by 20%
     * `color-value(primary, -30%)` → Primary color darkened by 30%

6. `font-size($size)`:
   * Returns a CSS variable for font size
   * Example: `font-size(h1)` → `var(--h1)` (3rem)

7. `breakpoint($size)`:
   * Returns a CSS variable for breakpoint
   * Example: `breakpoint(md)` → `var(--break-md)` (992px)

### Mixins

1. `flexbox($direction, $justify, $items, $content, $wrap, $gap)`:
   * Sets up a flexbox layout with the specified properties
   * Example: 
     ```scss
     @include flexbox(column, center, flex-start);
     ```

2. `font-styles($size, $weight, $style, $transform)`:
   * Applies typography styles
   * Example:
     ```scss
     @include font-styles(h2, bold, normal, uppercase);
     ```

3. `responsive($breakpoint)`:
   * Creates a media query for responsive design
   * Example:
     ```scss
     @include responsive(md) {
       // Styles for medium-sized screens and up
     }
     ```

4. `spacing($type, $top, $right, $bottom, $left)`:
   * Applies spacing (margin or padding) to elements
   * Example:
     ```scss
     @include spacing(margin, md, lg, md, lg);
     ```

5. `margin($top, $right, $bottom, $left)`:
   * Shorthand for applying margin spacing
   * Example:
     ```scss
     @include margin(md, sm, md, sm);
     ```

6. `padding($top, $right, $bottom, $left)`:
   * Shorthand for applying padding spacing
   * Example:
     ```scss
     @include padding(lg, md, lg, md);
     ```

## Usage Examples

### Creating a Card Component

```scss
.card {
  @include padding(md);
  background-color: color(white);
  border-radius: spacing(xs);
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  
  &__header {
    @include margin(none, none, sm, none);
    color: color(primary);
    font-size: font-size(h5);
  }
  
  &__body {
    @include flexbox(column);
    @include margin(md, none, none, none);
  }
  
  &__footer {
    @include flexbox(row, space-between);
    @include margin(md, none, none, none);
    @include padding(md, none, none, none);
    border-top: 1px solid color-value(text, 80%);
  }
  
  @include responsive(md) {
    @include flexbox(row);
    
    &__body {
      flex: 1;
    }
  }
}
```

### Creating a Button Component

```scss
.button {
  @include padding(sm, md);
  background-color: color(primary);
  color: color(white);
  border: none;
  border-radius: spacing(xs);
  cursor: pointer;
  transition: background-color 0.3s ease;
  
  &:hover {
    background-color: color-value(primary, -10%);
  }
  
  &--secondary {
    background-color: color(secondary);
    
    &:hover {
      background-color: color-value(secondary, -10%);
    }
  }
  
  &--small {
    @include padding(xs, sm);
    font-size: font-size(footnote);
  }
  
  &--large {
    @include padding(md, lg);
    font-size: font-size(h6);
  }
}
```

### Responsive Layout

```scss
.container {
  width: container(w-100);
  @include margin(none, auto);
  @include padding(md);
  
  @include responsive(sm) {
    width: container(w-90);
  }
  
  @include responsive(md) {
    width: container(w-80);
  }
  
  @include responsive(lg) {
    width: container(w-70);
  }
}

.grid {
  @include flexbox(row, flex-start, stretch, flex-start, wrap, md);
  
  &__item {
    width: 100%;
    
    @include responsive(sm) {
      width: column(col-6);
    }
    
    @include responsive(md) {
      width: column(col-4);
    }
    
    @include responsive(lg) {
      width: column(col-3);
    }
  }
}
```









 

