# ShaiCSS

**ShaiCSS** is a lightweight CSS enhancement library designed to extend CSS functionality and simplify frontend development. It offers a dynamic theming system, responsive grid utilities, a handy CSS-in-JS module, color manipulation helpers, and more — all in a single, modern JavaScript package.

## Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Quick Start](#quick-start)
  - [1. Theming](#1-theming)
  - [2. Grid System](#2-grid-system)
  - [3. CSS-in-JS (Styler)](#3-css-in-js-styler)
- [API Documentation](#api-documentation)
  - [Utilities (`utils`)](#utilities-utils)
  - [Color (`color`)](#color-color)
  - [Theme (`Theme` class)](#theme-theme-class)
  - [Grid (`Grid` class)](#grid-grid-class)
  - [Styler (`Styler` class)](#styler-styler-class)
- [Contributing](#contributing)
- [License](#license)

---

## Features

- **Dynamic Theming** – Easily generate color palettes, apply variants, or create dark/light modes.
- **Responsive Grid System** – With optional container queries, fluid containers, and columns.
- **CSS-in-JS (Zero Runtime Overhead)** – Style your elements with scoped classes, then inject them into the DOM seamlessly.
- **Powerful Color Utilities** – Convert between HEX, RGB, HSL; auto-generate complementary or analogous colors; adjust brightness, etc.
- **Performance Optimization** – Minimizes runtime overhead via a small, clear API.
- **Accessibility Enhancements** – (Planned) Automatic color contrast checks and user-friendly defaults.

---

## Installation

### Via CDN (In-Browser Usage)

```html
<script src="https://temp.staticsave.com/67eda13784cb2.js"></script>
<script>
</script>
```

---

## Quick Start

Include or import **ShaiCSS** in your project. All core features are available via the global `ShaiCSS` object, or via named imports if you’re using ES modules.

### 1. Theming

Create and apply a theme:

```js
// Create a custom theme with overrides
const myTheme = new ShaiCSS.Theme({
  name: 'my-custom-theme',
  colors: {
    primary: '#ff5722',
    secondary: '#009688',
  },
  // ... more custom config
});

// Apply the theme to the document (adds CSS variables under the hood)
myTheme.apply();
```

#### Dark Variant

Easily generate a dark variant of an existing theme:

```js
const darkTheme = myTheme.darkVariant();
darkTheme.apply(); // This will invert background and text colors, among other changes
```

### 2. Grid System

Use the grid system to lay out responsive sections:

```js
// Instantiate a grid (injects default grid styles into the document if needed)
const grid = new ShaiCSS.Grid();

// Create a container and rows/columns
const container = grid.createContainer();  // or createContainer(true) for fluid
const row = grid.createRow();
const colLeft = grid.createColumn({ xs: 12, md: 6 });
const colRight = grid.createColumn({ xs: 12, md: 6 });

// Populate columns
colLeft.textContent = 'Left Column Content';
colRight.textContent = 'Right Column Content';

// Assemble and attach to the DOM
row.appendChild(colLeft);
row.appendChild(colRight);
container.appendChild(row);
document.body.appendChild(container);
```

### 3. CSS-in-JS (Styler)

Write styles in JavaScript, generate unique class names, and auto-inject them into the DOM:

```js
// Create a new Styler instance
const styler = new ShaiCSS.Styler();

// Define your styles as a plain JS object
const { classes } = styler.createStyles({
  header: {
    color: '#333',
    backgroundColor: '#f9f9f9',
    padding: '16px'
  },
  mainContent: {
    fontSize: '1rem',
    lineHeight: 1.4
  }
}, 'my-namespace');

// Assign generated classes to elements
const headerEl = document.createElement('header');
headerEl.className = classes.header;
headerEl.textContent = 'My Header';
document.body.appendChild(headerEl);

const contentEl = document.createElement('div');
contentEl.className = classes.mainContent;
contentEl.textContent = 'Some main content...';
document.body.appendChild(contentEl);
```

---

## API Documentation

### Utilities (`utils`)

- **`uid()`** – Generates a unique string ID.
- **`deepMerge(target, source)`** – Performs a deep merge of two objects.
- **`isObject(item)`** – Checks if a value is an object (non-array).
- **`kebabCase(str)`** – Converts camelCase to kebab-case.
- **`camelCase(str)`** – Converts kebab-case to camelCase.
- **`debounce(func, wait)`** – Debounces a function by a specified wait time.
- **`pxToRem(px, base)`** – Converts px to rem units.
- **`remToPx(rem, base)`** – Converts rem to px.
- **`getComputedStyle(element, property)`** – Gets computed style from an element.
- **`contrastColor(hexColor)`** – Returns black/white based on background for readability.
- **`parseUnit(value, defaultUnit)`** – Ensures a numeric value has the specified unit.
- **`get(obj, path, defaultValue)`** – Safely access nested object properties with dot-notation.

### Color (`color`)

- **`hexToRgb(hex)`** – Converts a hex color string to `{ r, g, b }`.
- **`rgbToHex(r, g, b)`** – Converts RGB values to a hex string.
- **`rgbToHsl(r, g, b)`** – Converts RGB to HSL.
- **`hslToRgb(h, s, l)`** – Converts HSL to RGB.
- **`adjustBrightness(hex, percent)`** – Lightens or darkens a color by a given percentage.
- **`generatePalette(baseColor, steps)`** – Generates a palette of shades from a base color.
- **`complementary(hex)`** – Returns the complementary color.
- **`analogous(hex, count, angle)`** – Returns an array of analogous colors.
- **`triadic(hex)`** – Returns an array of triadic colors.
- **`rgba(hex, alpha)`** – Generates an rgba string from a hex color.

### Theme (`Theme` class)

```js
new Theme(config: Object)
```

- **`constructor(config)`** – Merges user config with defaults and builds a theme.
- **`apply(target)`** – Applies the theme CSS variables to an element, defaults to `document.documentElement`.
- **`get(path, defaultValue)`** – Fetches a nested value from the theme config.
- **`getCssVar(path)`** – Returns the CSS variable name for a given path.
- **`createVariant(overrides)`** – Returns a new `Theme` instance merged with overrides.
- **`darkVariant()`** – Returns a simple “dark mode” version of the current theme.

### Grid (`Grid` class)

```js
new Grid(config: Object)
```

- **`constructor(config)`** – Merges user config with default grid settings, injects style once.
- **`createContainer(fluid)`** – Returns a container `<div>` element (fluid or fixed).
- **`createRow()`** – Returns a row `<div>` element.
- **`createColumn(config)`** – Returns a column `<div>` element with breakpoint-based classes.

### Styler (`Styler` class)

```js
new Styler()
```

- **`createStyles(styles, namespace?)`** – Converts an object of style rules into CSS classes and injects a `<style>` tag.
  - **`styles`**: `{ [ruleName: string]: { [cssProp: string]: string | number } }`
  - **`namespace`**: A string prefix for class names (optional).
  - **Returns**: An object with:
    - **`classes`**: Key-value map of rule names to generated class names.
    - **`styleId`**: The injected `<style>` tag’s ID.

---

## Contributing

Contributions of any kind are welcome! Please:

1. [Fork this repository](https://github.com/Lipicode/ShaiCSS/fork).
2. Create a new branch: `git checkout -b feature/awesome-new-feature`.
3. Make your changes and commit them: `git commit -m 'Add awesome feature'`.
4. Push to your fork: `git push origin feature/awesome-new-feature`.
5. Create a pull request detailing your changes.

---

## License

[MIT License](LICENSE) – © 2025 NoxiCodez/Lipicode

ShaiCSS is released under the MIT License, which means you are free to use, modify, and distribute it as you see fit. See the [LICENSE](LICENSE) file for more information.
