# Flexssentials: A Minimalist and Flexible SCSS Framework

**Flexssentials** is a modern, minimalist, and flexible SCSS framework designed to streamline your web development process. It offers a simple and lightweight approach to styling essentials: colors, layout, typography, and spacing.

---

## **Key Features**

- **Single-File Structure**: Everything you need in one clean, well-organized SCSS file.
- **DRY (Don't Repeat Yourself)**: SCSS maps automatically generate CSS variables.
- **Framework Agnostic**: Works perfectly with Angular, React, Vue, or any web project.
- **Powerful Yet Simple**: Provides core functionality without bloat.
- **Runtime Flexibility**: Uses CSS variables for dynamic theming capabilities.

---

## **What's Included**

- **SCSS Maps**: Single source of truth for:
  - Colors
  - Grid columns
  - Spacing
  - Utility sizes
  - Font sizes
  - Breakpoints

- **CSS Variables**: Auto-generated from SCSS maps, allowing runtime customization.

- **Core Functions**: Simple utilities for:
  - Accessing CSS variables
  - Manipulating colors
  - Working with spacing
  - Responsive breakpoints

- **Essential Mixins**:
  - Flexbox layouts
  - Typography
  - Margin and padding
  - Responsive design

---

## **Getting Started**

### 1. **Include in Your Project**

Simply copy the `framework.scss` file into your project's SCSS directory.

### 2. **Import and Use**

Import the framework in your main SCSS file:
```scss
@use 'sass:color';
@use 'scss/framework.scss' as *;
```

### 3. **Start Using the Mixins and Functions**

```scss
.container {
  @include padding(md);
  background-color: color(primary);
  
  @include responsive(md) {
    width: container(w-80);
  }
}

.button {
  @include flexbox(row, center, center);
  background-color: color-value(primary, -10%);
  color: color-value(grayscale, 100%);
}
```

---

## **Documentation**

Refer to the `documentation.md` file for detailed usage instructions, examples, and a complete API reference.

---

## **Why Flexssentials?**

Flexssentials strikes the perfect balance between utility and simplicity:

- **No CSS Bloat**: Only generates what you actually use in your stylesheets.
- **No Learning Curve**: Intuitive functions and mixins with clear naming.
- **Flexible Architecture**: Adapt it to any design system or project requirements.
- **Modern Best Practices**: Built on current SCSS best practices and patterns.

---

## **License**

Flexssentials is licensed under the [MIT License](LICENSE). Feel free to use it in your projects.

---