---
name: color-palette-generator
description: >
  TRIGGER: Use this skill when the user asks to generate a color palette, create colors
  for a project, pick a color scheme, "generate colors", "create a palette", "color scheme",
  "brand colors", "design system colors", "CSS color variables", "Tailwind colors",
  "I need colors for my project", "make me a palette", or mentions wanting accessible
  colors, contrast ratios, or color accessibility. Takes a base color, mood, or theme
  and outputs CSS variables, Tailwind config, and a visual HTML preview with accessibility
  contrast checks.
---

# Color Palette Generator

Generate cohesive, accessible color palettes for web projects. Accept a base color or mood/theme as input and produce ready-to-use CSS variables, Tailwind config, and a visual HTML preview with WCAG contrast checks.

## Step-by-Step Process

### 1. Determine the Input

Accept any of these as starting points:

- **Hex color**: `#3B82F6`
- **RGB**: `rgb(59, 130, 246)`
- **HSL**: `hsl(217, 91%, 60%)`
- **Named color**: `blue`, `coral`, `forest green`
- **Mood/theme**: `professional`, `playful`, `dark and moody`, `warm`, `cyberpunk`
- **Brand reference**: `similar to Stripe`, `like GitHub's palette`

If the user provides a mood instead of a color, select an appropriate base color that embodies that mood.

### 2. Generate the Palette

Build a complete palette with these categories:

**Primary Scale (10 shades):**
Generate shades from 50 (lightest) to 950 (darkest) — matching Tailwind's scale convention:
- 50, 100, 200, 300, 400, 500 (base), 600, 700, 800, 900, 950

**Semantic Colors:**
- `success`: A green that harmonizes with the primary
- `warning`: An amber/yellow that harmonizes with the primary
- `error`: A red that harmonizes with the primary
- `info`: A blue (or the primary if primary is blue)

**Neutral Scale:**
Generate a neutral gray scale (50–950) that has a subtle tint of the primary color — this creates visual cohesion compared to pure grays.

**Surface Colors:**
- `background`: Page background
- `surface`: Card/component background
- `surface-elevated`: Elevated elements (modals, dropdowns)
- `border`: Default border color
- `text-primary`: Main text color
- `text-secondary`: Muted text color
- `text-inverted`: Text on primary-colored backgrounds

### 3. Check Accessibility

For every text/background combination, calculate the WCAG contrast ratio:

```
Contrast Ratio = (L1 + 0.05) / (L2 + 0.05)
where L1 = lighter relative luminance, L2 = darker relative luminance
```

Requirements:
- **AA Normal text (14px)**: ratio >= 4.5:1
- **AA Large text (18px+ or 14px bold)**: ratio >= 3:1
- **AAA Normal text**: ratio >= 7:1

Flag any combinations that fail AA. Suggest alternatives for failing combinations.

Key combinations to check:
- `text-primary` on `background`
- `text-secondary` on `background`
- `text-inverted` on `primary-500`
- `text-inverted` on `primary-600`
- `text-inverted` on `primary-700`

### 4. Output CSS Variables

```css
:root {
  /* Primary */
  --color-primary-50: #eff6ff;
  --color-primary-100: #dbeafe;
  --color-primary-200: #bfdbfe;
  --color-primary-300: #93c5fd;
  --color-primary-400: #60a5fa;
  --color-primary-500: #3b82f6;
  --color-primary-600: #2563eb;
  --color-primary-700: #1d4ed8;
  --color-primary-800: #1e40af;
  --color-primary-900: #1e3a8a;
  --color-primary-950: #172554;

  /* Semantic */
  --color-success: #22c55e;
  --color-warning: #f59e0b;
  --color-error: #ef4444;
  --color-info: #3b82f6;

  /* Surfaces */
  --color-bg: #ffffff;
  --color-surface: #f8fafc;
  --color-surface-elevated: #ffffff;
  --color-border: #e2e8f0;

  /* Text */
  --color-text: #0f172a;
  --color-text-muted: #64748b;
  --color-text-inverted: #ffffff;
}
```

### 5. Output Tailwind Config

```js
// tailwind.config.js — extend the colors section
module.exports = {
  theme: {
    extend: {
      colors: {
        primary: {
          50: '#eff6ff',
          100: '#dbeafe',
          200: '#bfdbfe',
          300: '#93c5fd',
          400: '#60a5fa',
          500: '#3b82f6',
          600: '#2563eb',
          700: '#1d4ed8',
          800: '#1e40af',
          900: '#1e3a8a',
          950: '#172554',
        },
        // ... other scales
      },
    },
  },
}
```

### 6. Generate Visual HTML Preview

Create a self-contained HTML file that displays:
- All color swatches with hex values
- Text samples on different backgrounds
- Contrast ratio badges (pass/fail)
- Light and dark mode previews
- Sample UI components (button, card, input) using the palette

Write this to a file like `color-palette-preview.html` so the user can open it in a browser.

## Dark Mode

Always generate a dark mode variant alongside the light palette:

```css
@media (prefers-color-scheme: dark) {
  :root {
    --color-bg: #0f172a;
    --color-surface: #1e293b;
    --color-surface-elevated: #334155;
    --color-border: #334155;
    --color-text: #f1f5f9;
    --color-text-muted: #94a3b8;
  }
}
```

The primary color scale can often stay the same in dark mode — it's the surfaces, text, and borders that need to invert.

## Edge Cases

- **Very light base color**: Shift the "500" anchor to a more saturated version so the scale has enough range.
- **Very dark base color**: Same — ensure the light end of the scale is actually light.
- **Neon/vivid colors**: Warn that highly saturated colors can cause eye strain for large surfaces. Suggest using them as accents only.
- **Grayscale palette**: If the user wants a monochrome palette, generate a single gray scale with carefully chosen contrast levels.
- **Existing palette**: If the project already has colors defined, read them and suggest complementary additions rather than replacing everything.
