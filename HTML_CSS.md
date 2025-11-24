# Technical Paper on HTML/CSS Fundamentals

## Abstract

This paper provides a concise overview of essential HTML and CSS concepts for modern web development, covering the box model, positioning, layout systems, responsive design, and best practices.

---

## 1. The CSS Box Model

Every HTML element is rendered as a rectangular box consisting of four areas:

1. **Content** - The actual content (text, images)
2. **Padding** - Space between content and border
3. **Border** - Line surrounding the padding
4. **Margin** - Space outside the border

```css
/* Default: width applies to content only */
.box {
  width: 300px;
  padding: 20px;
  border: 5px solid black;
  /* Total width = 300 + 40 + 10 = 350px */
}

/* border-box: width includes padding and border */
.box-alt {
  box-sizing: border-box;
  width: 300px;
  padding: 20px;
  border: 5px solid black;
  /* Total width = 300px */
}
```

**Best Practice:** Use `box-sizing: border-box` globally for intuitive sizing.

---

## 2. Inline vs Block Elements

### Block Elements
- Start on a new line
- Take full width available
- Can set width and height
- Examples: `<div>`, `<p>`, `<h1>`, `<section>`, `<article>`

### Inline Elements
- Flow within text
- Only take necessary width
- Cannot set width/height
- Examples: `<span>`, `<a>`, `<strong>`, `<img>`

### Inline-Block
- Flow horizontally like inline
- Can set width/height like block

```css
.element {
  display: block;        /* Block-level */
  display: inline;       /* Inline */
  display: inline-block; /* Inline-block */
  display: none;         /* Hidden */
}
```

---

## 3. CSS Positioning

### Static (Default)
Normal document flow. `top`, `left`, etc. have no effect.

### Relative
Positioned relative to its normal position. Original space preserved.

```css
.relative {
  position: relative;
  top: 20px;
  left: 30px;
}
```

### Absolute
Removed from normal flow. Positioned relative to nearest positioned ancestor.

```css
.container {
  position: relative; /* Creates positioning context */
}

.absolute {
  position: absolute;
  top: 50px;
  right: 20px;
}
```

### Fixed
Positioned relative to viewport. Stays in place when scrolling.

```css
.fixed-header {
  position: fixed;
  top: 0;
  width: 100%;
}
```

### Sticky
Toggles between relative and fixed based on scroll position.

```css
.sticky-nav {
  position: sticky;
  top: 0;
}
```

---

## 4. Common CSS Structural Classes

```css
/* Container */
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}

/* Flexbox row/columns */
.row {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}

.col {
  flex: 1;
}

.col-6 {
  flex: 0 0 50%;
}

/* Grid layout */
.grid {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  gap: 20px;
}
```

---

## 5. Common CSS Styling Classes

```css
/* Typography */
.text-center { text-align: center; }
.text-uppercase { text-transform: uppercase; }
.font-bold { font-weight: bold; }

/* Display */
.d-block { display: block; }
.d-none { display: none; }
.d-flex { display: flex; }

/* Spacing (utility-first approach) */
.m-0 { margin: 0; }
.m-2 { margin: 0.5rem; }
.mt-3 { margin-top: 1rem; }
.p-2 { padding: 0.5rem; }
.px-4 { padding-left: 1.5rem; padding-right: 1.5rem; }

/* Colors */
.text-primary { color: #007bff; }
.bg-light { background-color: #f8f9fa; }
```

---

## 6. CSS Specificity

Specificity determines which styles apply when multiple rules target the same element.

### Hierarchy (lowest to highest):
1. Universal selector (`*`): 0,0,0,0
2. Element selectors (`div`, `p`): 0,0,0,1
3. Class selectors (`.class`): 0,0,1,0
4. ID selectors (`#id`): 0,1,0,0
5. Inline styles: 1,0,0,0
6. `!important`: Overrides all

### Examples:
```css
p { color: red; }              /* 0,0,0,1 */
.text { color: blue; }         /* 0,0,1,0 - WINS */
#header { color: purple; }     /* 0,1,0,0 - WINS */
p.text { color: green; }       /* 0,0,1,1 */
```

**Best Practice:** Keep specificity low. Avoid `!important` and overly specific selectors.

---

## 7. CSS Responsive Queries

Media queries adapt styles to different screen sizes.

### Mobile-First Approach (Recommended):
```css
/* Mobile styles (default) */
.container { padding: 10px; }

/* Tablets (768px and up) */
@media (min-width: 768px) {
  .container { padding: 20px; }
  .col-md-6 { flex: 0 0 50%; }
}

/* Desktops (992px and up) */
@media (min-width: 992px) {
  .container { max-width: 960px; }
  .col-lg-4 { flex: 0 0 33.333%; }
}

/* Large desktops (1200px and up) */
@media (min-width: 1200px) {
  .container { max-width: 1140px; }
}
```

### Advanced Media Queries:
```css
/* Orientation */
@media (orientation: landscape) {
  .hero { height: 50vh; }
}

/* Dark mode preference */
@media (prefers-color-scheme: dark) {
  body {
    background-color: #1a1a1a;
    color: #fff;
  }
}

/* Print styles */
@media print {
  .no-print { display: none; }
}
```

---

## 8. Flexbox

Flexbox is a one-dimensional layout system for arranging items in rows or columns.

### Container Properties:
```css
.flex-container {
  display: flex;
  flex-direction: row;              /* row | column */
  flex-wrap: wrap;                  /* wrap items */
  justify-content: space-between;   /* main axis alignment */
  align-items: center;              /* cross axis alignment */
  gap: 20px;                        /* spacing between items */
}
```

### Item Properties:
```css
.flex-item {
  flex: 1;              /* grow to fill space */
  flex: 0 0 200px;      /* fixed size, no grow/shrink */
  align-self: center;   /* individual alignment */
  order: 1;             /* change visual order */
}
```

### Common Patterns:
```css
/* Center an element */
.center {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

/* Equal-width columns */
.columns > * {
  flex: 1;
}

/* Navigation bar */
.navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
```

---

## 9. CSS Grid

CSS Grid is a two-dimensional layout system for creating complex grid-based layouts.

### Container Properties:
```css
.grid-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);  /* 3 equal columns */
  grid-template-rows: auto 1fr auto;      /* header, content, footer */
  gap: 20px;
  
  /* Alignment */
  justify-items: center;
  align-items: center;
}
```

### Item Properties:
```css
.grid-item {
  grid-column: 1 / 3;       /* span columns 1-3 */
  grid-row: span 2;         /* span 2 rows */
  grid-area: 1 / 1 / 3 / 3; /* row-start / col-start / row-end / col-end */
}
```

### Named Grid Areas:
```css
.layout {
  display: grid;
  grid-template-areas:
    "header header header"
    "sidebar main main"
    "footer footer footer";
  grid-template-columns: 200px 1fr 1fr;
  gap: 20px;
}

.header { grid-area: header; }
.sidebar { grid-area: sidebar; }
.main { grid-area: main; }
.footer { grid-area: footer; }
```

### Common Patterns:
```css
/* Responsive card grid */
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 30px;
}

/* Center with grid */
.center {
  display: grid;
  place-items: center;
  height: 100vh;
}
```

**Flexbox vs Grid:**
- Use **Flexbox** for one-dimensional layouts (rows OR columns)
- Use **Grid** for two-dimensional layouts (rows AND columns)

---

## 10. Common HTML Header Meta Tags

### Essential Meta Tags:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <!-- Character encoding -->
  <meta charset="UTF-8">
  
  <!-- Responsive viewport -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
  <!-- IE compatibility -->
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  
  <!-- SEO -->
  <title>Page Title - Site Name</title>
  <meta name="description" content="Page description (150-160 chars)">
  <meta name="keywords" content="html, css, web development">
  
  <!-- Favicon -->
  <link rel="icon" type="image/x-icon" href="/favicon.ico">
</head>
</html>
```

### Open Graph (Social Media):
```html
<meta property="og:title" content="Page Title">
<meta property="og:description" content="Page description">
<meta property="og:image" content="https://example.com/image.jpg">
<meta property="og:url" content="https://example.com/page">
<meta property="og:type" content="website">
```

### Twitter Cards:
```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="Page Title">
<meta name="twitter:description" content="Description">
<meta name="twitter:image" content="https://example.com/image.jpg">
```

### Performance:
```html
<!-- Preconnect to external domains -->
<link rel="preconnect" href="https://fonts.googleapis.com">

<!-- Preload critical resources -->
<link rel="preload" href="styles.css" as="style">

<!-- Canonical URL (prevent duplicate content) -->
<link rel="canonical" href="https://example.com/page">
```

---

## 11. Additional Important Concepts

### CSS Variables:
```css
:root {
  --primary-color: #007bff;
  --spacing-unit: 8px;
}

.button {
  background-color: var(--primary-color);
  padding: calc(var(--spacing-unit) * 2);
}
```

### CSS Transitions:
```css
.button {
  transition: background-color 0.3s ease;
}

.button:hover {
  background-color: darkblue;
}
```

### Pseudo-classes and Pseudo-elements:
```css
/* Pseudo-classes */
a:hover { color: blue; }
li:first-child { font-weight: bold; }
input:focus { border-color: blue; }

/* Pseudo-elements */
p::before { content: "→ "; }
p::after { content: " ←"; }
::selection { background: yellow; }
```

### CSS Reset/Normalize:
```css
/* Simple reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

/* or use normalize.css for better cross-browser consistency */
```

---

## 12. Best Practices

1. **Use semantic HTML** - `<header>`, `<nav>`, `<main>`, `<article>`, `<footer>`
2. **Mobile-first responsive design** - Start with mobile styles, scale up
3. **Keep specificity low** - Use classes over IDs, avoid overly specific selectors
4. **Use modern layout systems** - Flexbox and Grid over floats
5. **Optimize performance** - Minify CSS, use critical CSS, preload resources
6. **Maintain consistency** - Use CSS variables, establish naming conventions
7. **Ensure accessibility** - Proper contrast, semantic markup, keyboard navigation
8. **Use CSS preprocessors** - Sass/SCSS for variables, nesting, mixins (when needed)
9. **Follow BEM or similar methodology** - Block Element Modifier for maintainable CSS
10. **Test across browsers** - Use autoprefixer for vendor prefixes

---

## References

https://github.com/mountblue/python-django-path/blob/master/html-css/html-css.md
