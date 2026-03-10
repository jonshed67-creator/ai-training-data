# Apple Design System -- Comprehensive Reference

> Research compiled from Apple Human Interface Guidelines (HIG), WWDC sessions, and community implementation guides. Covers iOS/iPadOS/macOS conventions through 2025-2026, including the Liquid Glass design language introduced at WWDC 2025.

---

## 1. Design Philosophy

### Core Principles

- **Clarity**: Text is legible at every size, icons are precise and lucid, adornments are subtle and appropriate, and a sharpened focus on functionality motivates the design.
- **Deference**: Fluid motion and a crisp interface help people understand and interact with content while never competing with it. Content fills the screen; translucency and blurring hint at more.
- **Depth**: Visual layers and realistic motion convey hierarchy, impart vitality, and facilitate understanding. Distinct layers help establish hierarchy and position.
- **Direct Manipulation**: Direct manipulation of onscreen content engages people and facilitates understanding. Users see immediate, visible results of their actions.
- **Feedback**: Interactive elements highlight briefly when tapped. Progress indicators communicate the status of long-running operations. Animation and sound help clarify the results of actions.
- **Metaphors**: People learn more quickly when an app's virtual objects and actions are metaphors for familiar experiences -- whether rooted in the real or digital world.
- **Consistency**: Apps use familiar standards and conventions -- system-provided interface elements, well-known icons, standard text styles, and a uniform terminology.
- **User Control**: The best apps find the correct balance between enabling people and avoiding unwanted outcomes.

### Liquid Glass (2025-2026 Design Language)

Apple's most significant visual redesign since iOS 7. Applies universally across iOS 26, iPadOS 26, macOS 26, watchOS 26, and tvOS 26.

Key characteristics:
- Translucent, rounded UI components with optical qualities of real glass (including refraction)
- Elements react dynamically to motion, content, and user input
- Simulates real-world glass effects: light bending on curved edges, localized distortion, specular highlights
- Refined color palette with bolder left-aligned typography
- Concentricity creating a unified rhythm between hardware and software

---

## 2. Typography

### San Francisco Font Family

Apple's system typeface, designed for optimal legibility across all screen sizes.

| Variant | Usage |
|---------|-------|
| SF Pro | iOS, iPadOS, macOS system font |
| SF Pro Display | 20pt and above (automatic optical sizing) |
| SF Pro Text | Below 20pt (automatic optical sizing) |
| SF Pro Rounded | Rounded variant for softer contexts |
| SF Compact | watchOS, small UI contexts |
| SF Compact Rounded | watchOS rounded variant |
| SF Mono | Code, tabular data |
| New York | Serif companion (editorial, reading) |

### iOS Type Scale (Default Size Category)

| Text Style | Size (pt) | Weight | Leading (line-height) |
|------------|-----------|--------|----------------------|
| Large Title | 34 | Regular | 41 |
| Title 1 | 28 | Regular | 34 |
| Title 2 | 22 | Regular | 28 |
| Title 3 | 20 | Regular | 25 |
| Headline | 17 | Semi-Bold | 22 |
| Body | 17 | Regular | 22 |
| Callout | 16 | Regular | 21 |
| Subheadline | 15 | Regular | 20 |
| Footnote | 13 | Regular | 18 |
| Caption 1 | 12 | Regular | 16 |
| Caption 2 | 11 | Regular | 13 |

### Typography Rules

- Minimum legible size: 11pt
- Navigation bar title (scrolled/collapsed): 17pt Medium
- Navigation bar title (expanded/large): 34pt Bold
- Tab bar labels: 10pt Regular
- Tracking (letter-spacing) adjusts automatically by optical size in the native SF font
- Use text styles (not hard-coded sizes) to support Dynamic Type

### CSS Font Stack (Web Approximation)

```css
/* Apple's system font stack */
font-family: -apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text",
             "Helvetica Neue", Helvetica, Arial, sans-serif;

/* For monospace */
font-family: "SF Mono", SFMono-Regular, ui-monospace, Menlo, Monaco,
             "Cascadia Code", "Courier New", monospace;

/* Apple.com uses */
font-family: "SF Pro Display", "SF Pro Icons", "Helvetica Neue", Helvetica,
             Arial, sans-serif;
```

### CSS Type Scale Approximation

```css
:root {
  --font-large-title: 600 2.125rem/1.2 var(--font-stack);   /* 34px */
  --font-title1:      600 1.75rem/1.21 var(--font-stack);    /* 28px */
  --font-title2:      600 1.375rem/1.27 var(--font-stack);   /* 22px */
  --font-title3:      600 1.25rem/1.25 var(--font-stack);    /* 20px */
  --font-headline:    600 1.0625rem/1.29 var(--font-stack);  /* 17px */
  --font-body:        400 1.0625rem/1.47 var(--font-stack);  /* 17px */
  --font-callout:     400 1rem/1.31 var(--font-stack);       /* 16px */
  --font-subheadline: 400 0.9375rem/1.33 var(--font-stack);  /* 15px */
  --font-footnote:    400 0.8125rem/1.38 var(--font-stack);  /* 13px */
  --font-caption1:    400 0.75rem/1.33 var(--font-stack);    /* 12px */
  --font-caption2:    400 0.6875rem/1.18 var(--font-stack);  /* 11px */
}
```

---

## 3. Spacing & Layout

### 8-Point Grid System

Apple uses an 8pt base grid. All spacing, sizing, and layout should use multiples of 8.

```
Spacing scale: 4, 8, 12, 16, 20, 24, 32, 40, 48, 56, 64, 72, 80
```

Note: 4pt is used as a half-step for fine-grained adjustments (icon padding, small gaps).

### Standard Spacing Values

| Token | Value | Usage |
|-------|-------|-------|
| `--space-xxs` | 4px | Tight internal padding, icon gaps |
| `--space-xs` | 8px | Minimum padding, compact spacing |
| `--space-sm` | 12px | Small component padding |
| `--space-md` | 16px | Standard content margins, card padding |
| `--space-lg` | 20px | Screen edge margins (iPhone) |
| `--space-xl` | 24px | Section spacing |
| `--space-2xl` | 32px | Large section gaps |
| `--space-3xl` | 40px | Major section dividers |
| `--space-4xl` | 48px | Page-level spacing |

### Component Dimensions

| Component | Height (pt) |
|-----------|-------------|
| Status bar | 59 (with Dynamic Island: 59) |
| Navigation bar (single row) | 44 |
| Navigation bar (large title row) | 58 |
| Navigation bar (search row) | 48 |
| Tab bar | 49 (83 with home indicator) |
| Home indicator region | 34 |
| Toolbar | 44 |
| Minimum tap target | 44 x 44 |

### Screen Margins

| Context | Margin |
|---------|--------|
| iPhone content margin | 16px (compact) / 20px (regular) |
| iPad content margin | 20px |
| Grouped table view inset | 16-20px |
| Card internal padding | 16px |
| List item horizontal padding | 16-20px |

### Safe Areas

Always respect safe areas for:
- Status bar at top
- Home indicator at bottom (34pt)
- Sensor housing (Dynamic Island / notch)
- Rounded screen corners

### CSS Spacing System

```css
:root {
  --spacing-unit: 8px;
  --space-0: 0;
  --space-1: 4px;    /* 0.5x */
  --space-2: 8px;    /* 1x */
  --space-3: 12px;   /* 1.5x */
  --space-4: 16px;   /* 2x */
  --space-5: 20px;   /* 2.5x */
  --space-6: 24px;   /* 3x */
  --space-8: 32px;   /* 4x */
  --space-10: 40px;  /* 5x */
  --space-12: 48px;  /* 6x */
  --space-16: 64px;  /* 8x */
  --space-20: 80px;  /* 10x */
}
```

---

## 4. Color System

### Semantic System Colors

#### Light Mode

| Color Name | Hex | RGBA |
|------------|-----|------|
| label (primary text) | #000000 | rgba(0, 0, 0, 1.0) |
| secondaryLabel | #3C3C43 | rgba(60, 60, 67, 0.6) |
| tertiaryLabel | #3C3C43 | rgba(60, 60, 67, 0.3) |
| quaternaryLabel | #3C3C43 | rgba(60, 60, 67, 0.18) |
| systemBackground | #FFFFFF | rgba(255, 255, 255, 1.0) |
| secondarySystemBackground | #F2F2F7 | rgba(242, 242, 247, 1.0) |
| tertiarySystemBackground | #FFFFFF | rgba(255, 255, 255, 1.0) |
| systemGroupedBackground | #F2F2F7 | rgba(242, 242, 247, 1.0) |
| secondarySystemGroupedBackground | #FFFFFF | rgba(255, 255, 255, 1.0) |
| tertiarySystemGroupedBackground | #F2F2F7 | rgba(242, 242, 247, 1.0) |
| separator | #3C3C43 | rgba(60, 60, 67, 0.29) |
| opaqueSeparator | #C6C6C8 | rgba(198, 198, 200, 1.0) |

#### Dark Mode

| Color Name | Hex | RGBA |
|------------|-----|------|
| label (primary text) | #FFFFFF | rgba(255, 255, 255, 1.0) |
| secondaryLabel | #EBEBF5 | rgba(235, 235, 245, 0.6) |
| tertiaryLabel | #EBEBF5 | rgba(235, 235, 245, 0.3) |
| quaternaryLabel | #EBEBF5 | rgba(235, 235, 245, 0.18) |
| systemBackground | #000000 | rgba(0, 0, 0, 1.0) |
| secondarySystemBackground | #1C1C1E | rgba(28, 28, 30, 1.0) |
| tertiarySystemBackground | #2C2C2E | rgba(44, 44, 46, 1.0) |
| systemGroupedBackground | #000000 | rgba(0, 0, 0, 1.0) |
| secondarySystemGroupedBackground | #1C1C1E | rgba(28, 28, 30, 1.0) |
| tertiarySystemGroupedBackground | #2C2C2E | rgba(44, 44, 46, 1.0) |
| separator | #545458 | rgba(84, 84, 88, 0.6) |
| opaqueSeparator | #38383A | rgba(56, 56, 58, 1.0) |

#### System Accent Colors

| Color | Light | Dark |
|-------|-------|------|
| systemRed | #FF3B30 | #FF453A |
| systemOrange | #FF9500 | #FF9F0A |
| systemYellow | #FFCC00 | #FFD60A |
| systemGreen | #34C759 | #30D158 |
| systemTeal | #5AC8FA | #64D2FF |
| systemBlue | #007AFF | #0A84FF |
| systemIndigo | #5856D6 | #5E5CE6 |
| systemPurple | #AF52DE | #BF5AF2 |
| systemPink | #FF2D55 | #FF375F |

#### System Gray Scale

| Color | Light | Dark |
|-------|-------|------|
| systemGray | #8E8E93 | #8E8E93 |
| systemGray2 | #AEAEB2 | #636366 |
| systemGray3 | #C7C7CC | #48484A |
| systemGray4 | #D1D1D6 | #3A3A3C |
| systemGray5 | #E5E5EA | #2C2C2E |
| systemGray6 | #F2F2F7 | #1C1C1E |

### CSS Color System Implementation

```css
:root {
  /* Light mode (default) */
  --color-label: #000000;
  --color-secondary-label: rgba(60, 60, 67, 0.6);
  --color-tertiary-label: rgba(60, 60, 67, 0.3);
  --color-quaternary-label: rgba(60, 60, 67, 0.18);
  --color-bg-primary: #FFFFFF;
  --color-bg-secondary: #F2F2F7;
  --color-bg-tertiary: #FFFFFF;
  --color-bg-grouped: #F2F2F7;
  --color-separator: rgba(60, 60, 67, 0.29);
  --color-separator-opaque: #C6C6C8;

  /* Accent colors */
  --color-blue: #007AFF;
  --color-green: #34C759;
  --color-red: #FF3B30;
  --color-orange: #FF9500;
  --color-yellow: #FFCC00;
  --color-purple: #AF52DE;
  --color-pink: #FF2D55;
  --color-teal: #5AC8FA;
  --color-indigo: #5856D6;

  /* Gray scale */
  --color-gray: #8E8E93;
  --color-gray2: #AEAEB2;
  --color-gray3: #C7C7CC;
  --color-gray4: #D1D1D6;
  --color-gray5: #E5E5EA;
  --color-gray6: #F2F2F7;
}

@media (prefers-color-scheme: dark) {
  :root {
    --color-label: #FFFFFF;
    --color-secondary-label: rgba(235, 235, 245, 0.6);
    --color-tertiary-label: rgba(235, 235, 245, 0.3);
    --color-quaternary-label: rgba(235, 235, 245, 0.18);
    --color-bg-primary: #000000;
    --color-bg-secondary: #1C1C1E;
    --color-bg-tertiary: #2C2C2E;
    --color-bg-grouped: #000000;
    --color-separator: rgba(84, 84, 88, 0.6);
    --color-separator-opaque: #38383A;

    /* Accent colors shift slightly brighter in dark mode */
    --color-blue: #0A84FF;
    --color-green: #30D158;
    --color-red: #FF453A;
    --color-orange: #FF9F0A;
    --color-yellow: #FFD60A;
    --color-purple: #BF5AF2;
    --color-pink: #FF375F;
    --color-teal: #64D2FF;
    --color-indigo: #5E5CE6;

    /* Gray scale inverts direction */
    --color-gray: #8E8E93;
    --color-gray2: #636366;
    --color-gray3: #48484A;
    --color-gray4: #3A3A3C;
    --color-gray5: #2C2C2E;
    --color-gray6: #1C1C1E;
  }
}
```

---

## 5. Dark Mode

### Key Principles

- Dark mode is NOT a simple color inversion. Colors are carefully chosen for each context.
- Use **semantic colors** (not hard-coded values) so the system handles mode switching.
- Dark mode backgrounds use pure black (#000000) as the base, with elevated surfaces using progressively lighter grays (#1C1C1E, #2C2C2E, #3A3A3C).
- Accent colors become slightly brighter/more saturated in dark mode for contrast.

### Elevated Surfaces

In dark mode, elevation is communicated through lighter background colors:

| Elevation | Light Mode | Dark Mode (Base) | Dark Mode (Elevated) |
|-----------|-----------|------------------|---------------------|
| Base | #FFFFFF | #000000 | #1C1C1E |
| Level 1 | #F2F2F7 | #1C1C1E | #2C2C2E |
| Level 2 | #FFFFFF | #2C2C2E | #3A3A3C |

- "Elevated" variants appear in multitasking, multi-window, and popover contexts.
- Shadows are largely invisible on dark backgrounds; instead, lighter surface colors indicate hierarchy.

### Design Rules

- Prefer system background colors over custom backgrounds.
- Use vibrancy (translucent materials) to let background content show through.
- Avoid large areas of bright color in dark mode; use color sparingly as accents.
- Test all custom colors in both modes; aim for 7:1 contrast ratio for text.
- Use `prefers-color-scheme` media query for web implementations.

---

## 6. Component Patterns

### Navigation Bar

- Height: 44pt (single row), up to 44+58pt with large title
- Large title: 34pt Bold, left-aligned
- Collapsed title: 17pt Semi-Bold, centered
- Background: Translucent material with blur (in Liquid Glass: glass-like refraction)
- Tint color for interactive elements (default: systemBlue)
- Back button: chevron icon + previous screen title (truncated if needed)
- In iOS 26 (Liquid Glass): Navigation bars transition to toolbar patterns; labeled actions replaced by icon-only buttons

### Tab Bar

- Height: 49pt (83pt including home indicator area)
- 3-5 tabs recommended for iPhone
- Icon above label layout
- Label: 10pt Regular
- Active state: Filled icon + tint color
- Inactive state: Outline icon + systemGray
- Background: Translucent material with blur
- Remains visible across all screens except when covered by modal

### Action Sheets

- Appear from bottom of screen (iPhone) or as popover (iPad)
- Rounded corners: ~14px border-radius
- Grouped actions with separator lines
- Destructive actions: systemRed text color
- Cancel button separated by 8pt gap
- In iOS 26: Action sheets can appear contextually near the tap location

### Modals / Sheets

- Presented as cards sliding up from bottom
- Corner radius: ~10-12px at top
- Slight inset from screen edges (appears as card on top)
- Background dims to ~50% black overlay
- Supports detents: .medium (half screen) and .large (full screen)
- Swipe down to dismiss

### Cards

- Corner radius: 12-16px
- Internal padding: 16px
- Background: systemBackground or secondarySystemBackground
- Shadow: subtle, offset downward
- Grouped content with consistent internal spacing

### Lists / Table Views

- Row height: Minimum 44pt (tap target); typical 44-60pt
- Left inset for separators: 16-20px (aligned to text, not edge)
- Section headers: 13pt text, uppercase, secondaryLabel color
- Disclosure indicator (chevron) for drill-down rows
- Swipe actions: destructive (red), standard (gray/blue/green)

### Buttons

- Minimum tap target: 44x44pt
- Text buttons: 17pt, systemBlue, no background
- Filled buttons: Rounded rect, tint color background, white text
- Corner radius: 8-12px for standard buttons
- Gray/tinted buttons: secondarySystemBackground with tint text
- Destructive: systemRed

### Toggles (Switches)

- Width: 51pt, Height: 31pt
- On color: systemGreen (default) or custom tint
- Off color: systemGray5 (light) / systemGray4 (dark)
- Thumb: white circle with subtle shadow

### Segmented Controls

- Height: 32pt
- Corner radius: ~8px (rounded rect)
- Selected segment: White background with shadow
- Unselected: Clear background, secondaryLabel text
- Equal width segments

---

## 7. Corner Radius Conventions

### Superellipse (Squircle) Formula

Apple uses continuous corner curves (superellipse/squircle), NOT standard circular border-radius. The formula for replicating this:

```
corner-radius = side-length * 0.222
corner-smoothing = 61%  (in Figma)
```

### Standard Radius Values

| Element | Border Radius |
|---------|--------------|
| App icons (home screen) | ~22% of icon size (continuous curve) |
| Cards / Grouped lists | 12-16px |
| Buttons (standard) | 8-12px |
| Action sheet | 14px |
| Modal sheet | 10-12px |
| Small chips/tags | 6-8px |
| Toggle/switch | Fully round (50%) |
| Search fields | 10px |
| Text input fields | 8-10px |
| Full-width panels | 0px (edge to edge) |

### CSS Note

Standard CSS `border-radius` produces circular arcs, not Apple's squircle. For web, use higher radius values or the emerging `border-radius` with `corner-shape: squircle` (not yet widely supported). Alternatively, use SVG masks for precise squircle shapes.

---

## 8. Shadows, Blur & Materials

### Shadow Values (CSS Approximations)

```css
/* Subtle card shadow (Apple style) */
.shadow-sm {
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08),
              0 1px 2px rgba(0, 0, 0, 0.06);
}

/* Medium elevation */
.shadow-md {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08),
              0 2px 4px rgba(0, 0, 0, 0.06);
}

/* High elevation (modals, popovers) */
.shadow-lg {
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.12),
              0 4px 8px rgba(0, 0, 0, 0.08);
}

/* Apple.com style deep shadow */
.shadow-apple {
  box-shadow: 0 37px 70px -12px rgba(0, 0, 0, 0.3);
}

/* Card shadow on colored background */
.shadow-card {
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.2);
}
```

### Backdrop Blur / Vibrancy Materials

Apple uses "materials" -- translucent layers with background blur and tinting:

| Material | Blur | Background | Usage |
|----------|------|------------|-------|
| Ultra Thin | ~8px | rgba(255,255,255,0.1) | Subtle overlays |
| Thin | ~12px | rgba(255,255,255,0.2) | Navigation bars |
| Regular | ~16px | rgba(255,255,255,0.3) | Standard material |
| Thick | ~24px | rgba(255,255,255,0.5) | Prominent surfaces |
| Chrome (Liquid Glass) | ~2-4px + refraction | rgba(255,255,255,0.15) | iOS 26 glass elements |

```css
/* Standard Apple-like blur material (light) */
.material-regular {
  background: rgba(255, 255, 255, 0.72);
  backdrop-filter: saturate(180%) blur(20px);
  -webkit-backdrop-filter: saturate(180%) blur(20px);
}

/* Dark mode material */
.material-regular-dark {
  background: rgba(28, 28, 30, 0.72);
  backdrop-filter: saturate(180%) blur(20px);
  -webkit-backdrop-filter: saturate(180%) blur(20px);
}

/* Liquid Glass (2025+) */
.liquid-glass {
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: blur(4px) saturate(180%);
  -webkit-backdrop-filter: blur(4px) saturate(180%);
  border: 1px solid rgba(255, 255, 255, 0.8);
  border-radius: 20px;
  box-shadow: 0 8px 32px rgba(31, 38, 135, 0.2),
              inset 0 4px 20px rgba(255, 255, 255, 0.3);
}
```

### Liquid Glass Advanced Implementation

For refraction effects, SVG filters are required:

```css
/* Pair with SVG feDisplacementMap for refraction */
.liquid-glass-advanced {
  background: rgba(255, 255, 255, 0.15);
  backdrop-filter: url(#glass-filter) blur(2px) saturate(180%);
  border: 1px solid rgba(255, 255, 255, 0.8);
  border-radius: 2rem;
  box-shadow:
    0 8px 32px rgba(31, 38, 135, 0.2),
    inset 0 4px 20px rgba(255, 255, 255, 0.3);
  filter: brightness(115%);
}
```

Performance note: Limit Liquid Glass to a small number of floating UI elements (toolbars, modals, nav bars, primary CTAs). Heavy use causes frame drops on low-power devices.

Browser support: `backdrop-filter` with SVG filters works in Chromium-based browsers. Safari requires `-webkit-backdrop-filter`. SVG filter refraction is not yet supported in Safari or Firefox.

---

## 9. Animation & Motion

### Spring Animation (Primary Model)

Apple has standardized on spring-based animations rather than bezier curves. Springs produce natural, organic motion with no abrupt stopping point.

#### Modern Parameters (iOS 17+)

Two parameters only:
- **duration**: How long the animation takes (seconds). Default: ~0.5s
- **bounce**: How bouncy the animation is (0.0 = no bounce, 1.0 = very bouncy). Default: 0.0

#### Legacy Parameters (UIKit)

- **dampingRatio**: 0.0-1.0 (1.0 = critically damped/no bounce, 0.5 = bouncy). Default: 0.825
- **response**: Speed of the spring. Default: 0.55s
- **initialVelocity**: Starting momentum. 1.0 = traverse full distance in 1 second

#### SwiftUI Defaults

```
.spring(response: 0.55, dampingFraction: 0.825, blendDuration: 0)
```

### Standard iOS Transitions

| Transition | Duration | Curve/Spring |
|-----------|----------|-------------|
| Push/pop navigation | ~0.35s | Spring (no bounce) |
| Modal presentation | ~0.35s | ease-in-out |
| Sheet presentation | ~0.5s | Spring (slight bounce) |
| Tab switch | ~0.25s | ease-in-out |
| Toggle/switch | ~0.2s | Spring (no bounce) |
| Button press | ~0.1s | ease-out (scale 0.97) |
| Dismiss keyboard | ~0.25s | ease-in-out |
| Alert appearance | ~0.2s | Spring (subtle bounce) |
| List reorder | ~0.3s | Spring |

### CSS Animation Equivalents

```css
/* Apple-like spring (approximation with cubic-bezier) */
--ease-spring: cubic-bezier(0.175, 0.885, 0.32, 1.275);

/* Standard iOS transitions */
--ease-in-out: cubic-bezier(0.42, 0, 0.58, 1);
--ease-out: cubic-bezier(0, 0, 0.58, 1);
--ease-in: cubic-bezier(0.42, 0, 1, 1);

/* iOS-like spring with CSS (using linear() for precision) */
--spring-smooth: linear(
  0, 0.006, 0.025 2.8%, 0.101 6.1%, 0.539 18.9%,
  0.721 25.3%, 0.849 31.5%, 0.937 38.1%,
  0.968 41.8%, 0.991 45.7%, 1.006 50.1%,
  1.015 55%, 1.017 63.9%, 1.001 85.7%, 1
);

/* Durations */
--duration-fast: 0.15s;
--duration-normal: 0.3s;
--duration-slow: 0.5s;

/* Usage */
.element {
  transition: transform 0.35s var(--spring-smooth),
              opacity 0.25s var(--ease-out);
}
```

### Gesture-Driven Animation Principles

- Animations should be interruptible: a user can grab a moving element mid-animation.
- Velocity should transfer from gesture to animation (momentum).
- Rubber-banding: overscroll/drag beyond bounds snaps back with a spring.
- Swipe-to-dismiss: element tracks finger, then completes or reverses based on velocity/threshold.

### Micro-Interactions

- Button tap: Scale to ~0.97 on press, spring back to 1.0 on release
- Long press: Subtle scale + haptic feedback
- Pull to refresh: Rubber-band stretch, spinner appears at threshold
- Switch toggle: Thumb slides with spring, track color cross-fades
- Contextual menu: Scale-up spring from press point

---

## 10. SF Symbols

### Overview

SF Symbols is Apple's icon library, containing 6,900+ symbols (as of SF Symbols 7) that integrate seamlessly with the San Francisco font.

### Symbol Properties

- **Nine weights**: Ultralight through Black (matching SF font weights)
- **Three scales**: Small, Medium, Large (for different contexts alongside text)
- **Four rendering modes**:
  - Monochrome: Single color, fully opaque
  - Hierarchical: Single color with varying opacity for depth
  - Palette: Two or more custom colors
  - Multicolor: Fixed, inherent colors (like the Apple logo colors)

### Symbol Categories

Communication, Weather, Objects, Devices, Connectivity, Transportation, Human, Nature, Editing, Text Formatting, Media, Keyboard, Commerce, Time, Health, Shapes, Arrows, Indices, Math, Gaming, and more.

### Usage Guidelines

- Use symbols alongside text; they automatically align to the font baseline.
- Match symbol weight to the text weight for visual harmony.
- Do NOT use SF Symbols in app icons, logos, or trademarks.
- Symbols depicting Apple products (AirPods, Apple Watch, etc.) cannot be customized.
- Prefer standard symbols over custom icons when a suitable match exists.
- Use rendering modes deliberately: monochrome for minimalist UI, hierarchical for depth, multicolor for recognition.

### Web Alternatives

SF Symbols are not available for web. Alternatives:
- **Phosphor Icons**: Similar weight/style flexibility
- **Heroicons**: Similar aesthetic, by the Tailwind team
- **Lucide Icons**: Clean, consistent stroke icons
- **Material Symbols (Outlined)**: Variable weight/fill support

---

## 11. Accessibility

### Dynamic Type

- Support all accessibility sizes (xSmall through AX5)
- Use text styles, not fixed font sizes
- Test at the largest accessibility size
- Ensure layouts reflow gracefully (no truncation of critical content)
- Line heights should maintain readability at all sizes

### VoiceOver

- Every interactive element needs an accessibility label
- Group related elements to reduce navigation verbosity
- Announce state changes (e.g., toggle on/off)
- Use proper semantic roles (button, heading, link)
- Provide hints for non-obvious interactions
- Test by navigating with VoiceOver without looking at the screen

### Contrast Requirements

| Content Type | Minimum Ratio (WCAG AA) | Enhanced Ratio (WCAG AAA) |
|-------------|------------------------|--------------------------|
| Normal text (< 18pt) | 4.5:1 | 7:1 |
| Large text (>= 18pt bold or >= 24pt) | 3:1 | 4.5:1 |
| UI components & graphics | 3:1 | -- |

- Apple recommends a 7:1 contrast ratio for custom colors, exceeding WCAG AA.
- System semantic colors automatically meet contrast requirements in both modes.

### Additional Accessibility

- **Reduce Motion**: Respect `prefers-reduced-motion`; replace springs/bounces with dissolves.
- **Bold Text**: Support the bold text accessibility setting.
- **Increase Contrast**: Support increased contrast mode with more defined borders and stronger color differences.
- **Button Shapes**: Underline or outline buttons when the user enables button shapes.
- **Color Blindness**: Never rely solely on color to convey information.
- **Minimum Tap Target**: 44x44pt for all interactive elements.

### CSS Accessibility

```css
/* Respect reduced motion */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}

/* Respect increased contrast */
@media (prefers-contrast: more) {
  :root {
    --color-separator: rgba(0, 0, 0, 0.5);
    --shadow-card: 0 0 0 1px rgba(0, 0, 0, 0.2);
  }
}

/* Support dynamic type / user font size preference */
html {
  font-size: 100%; /* Respect browser default */
}
body {
  font-size: 1rem; /* Scale with user preference */
}
```

---

## 12. Modern Trends (2025-2026)

### Liquid Glass (Primary Trend)

- Universal across all Apple platforms
- Replaces flat/frosted glass with optically-realistic translucent surfaces
- Elements feature refraction, specular highlights, and dynamic light response
- Not purely decorative: serves as a depth and hierarchy signal

### Dynamic Island Integration

- Pill-shaped cutout at top of screen adapts to show live activities
- Smooth spring animations expand/contract the island
- Contextual information surfaces without interrupting the user
- Design around it: treat it as a feature, not an obstacle

### Widget Design

- Use SF Pro + SF Symbols consistently
- Minimal information density: 1-3 data points per widget
- Rounded corners matching the platform squircle
- Support multiple sizes: small (2x2), medium (4x2), large (4x4), extra large (iPad)
- Live Activities: real-time updates in a compact format

### Concentricity

- UI elements echo the rounded forms of hardware (screen corners, buttons, camera cutouts)
- Nested rounded rectangles share a common center
- Creates visual harmony between software and device

### Bolder Typography

- iOS 26 shifts toward bolder, more prominent titles
- Left-aligned large titles are the standard
- Reduced use of uppercase section headers in favor of bold mixed-case

---

## 13. Complete CSS Starter Template

```css
/* ============================================
   Apple-Inspired Design System (CSS Variables)
   ============================================ */

:root {
  /* --- Typography --- */
  --font-system: -apple-system, BlinkMacSystemFont, "SF Pro Display",
                 "SF Pro Text", "Helvetica Neue", Helvetica, Arial, sans-serif;
  --font-mono: "SF Mono", SFMono-Regular, ui-monospace, Menlo, Monaco,
               "Cascadia Code", "Courier New", monospace;
  --font-serif: "New York", "Iowan Old Style", "Apple Garamond",
                Baskerville, "Times New Roman", serif;

  /* --- Type Scale --- */
  --text-large-title: 34px;
  --text-title1: 28px;
  --text-title2: 22px;
  --text-title3: 20px;
  --text-headline: 17px;
  --text-body: 17px;
  --text-callout: 16px;
  --text-subheadline: 15px;
  --text-footnote: 13px;
  --text-caption1: 12px;
  --text-caption2: 11px;

  /* --- Spacing (8pt grid) --- */
  --space-1: 4px;
  --space-2: 8px;
  --space-3: 12px;
  --space-4: 16px;
  --space-5: 20px;
  --space-6: 24px;
  --space-8: 32px;
  --space-10: 40px;
  --space-12: 48px;

  /* --- Border Radius --- */
  --radius-sm: 6px;
  --radius-md: 10px;
  --radius-lg: 14px;
  --radius-xl: 20px;
  --radius-full: 9999px;

  /* --- Shadows --- */
  --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.08), 0 1px 2px rgba(0, 0, 0, 0.06);
  --shadow-md: 0 4px 12px rgba(0, 0, 0, 0.08), 0 2px 4px rgba(0, 0, 0, 0.06);
  --shadow-lg: 0 8px 32px rgba(0, 0, 0, 0.12), 0 4px 8px rgba(0, 0, 0, 0.08);

  /* --- Transitions --- */
  --ease-spring: cubic-bezier(0.175, 0.885, 0.32, 1.275);
  --ease-smooth: cubic-bezier(0.42, 0, 0.58, 1);
  --duration-fast: 0.15s;
  --duration-normal: 0.3s;
  --duration-slow: 0.5s;

  /* --- Colors (Light) --- */
  --color-label: #000000;
  --color-secondary-label: rgba(60, 60, 67, 0.6);
  --color-tertiary-label: rgba(60, 60, 67, 0.3);
  --color-bg-primary: #FFFFFF;
  --color-bg-secondary: #F2F2F7;
  --color-bg-tertiary: #FFFFFF;
  --color-separator: rgba(60, 60, 67, 0.29);
  --color-separator-opaque: #C6C6C8;
  --color-blue: #007AFF;
  --color-green: #34C759;
  --color-red: #FF3B30;
  --color-orange: #FF9500;
  --color-yellow: #FFCC00;
  --color-purple: #AF52DE;
  --color-pink: #FF2D55;
  --color-teal: #5AC8FA;
  --color-indigo: #5856D6;
  --color-gray: #8E8E93;
}

@media (prefers-color-scheme: dark) {
  :root {
    --color-label: #FFFFFF;
    --color-secondary-label: rgba(235, 235, 245, 0.6);
    --color-tertiary-label: rgba(235, 235, 245, 0.3);
    --color-bg-primary: #000000;
    --color-bg-secondary: #1C1C1E;
    --color-bg-tertiary: #2C2C2E;
    --color-separator: rgba(84, 84, 88, 0.6);
    --color-separator-opaque: #38383A;
    --color-blue: #0A84FF;
    --color-green: #30D158;
    --color-red: #FF453A;
    --color-orange: #FF9F0A;
    --color-yellow: #FFD60A;
    --color-purple: #BF5AF2;
    --color-pink: #FF375F;
    --color-teal: #64D2FF;
    --color-indigo: #5E5CE6;
    --color-gray: #8E8E93;

    --shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.3);
    --shadow-md: 0 4px 12px rgba(0, 0, 0, 0.3);
    --shadow-lg: 0 8px 32px rgba(0, 0, 0, 0.4);
  }
}

/* --- Material / Blur --- */
.material {
  background: rgba(255, 255, 255, 0.72);
  backdrop-filter: saturate(180%) blur(20px);
  -webkit-backdrop-filter: saturate(180%) blur(20px);
}

@media (prefers-color-scheme: dark) {
  .material {
    background: rgba(28, 28, 30, 0.72);
  }
}

/* --- Reduced Motion --- */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

---

## Sources

- [Apple Human Interface Guidelines](https://developer.apple.com/design/human-interface-guidelines)
- [Apple Typography Guidelines](https://developer.apple.com/design/human-interface-guidelines/typography)
- [Apple Dark Mode Guidelines](https://developer.apple.com/design/human-interface-guidelines/dark-mode)
- [Apple Color Guidelines](https://developer.apple.com/design/human-interface-guidelines/color)
- [Apple Motion Guidelines](https://developer.apple.com/design/human-interface-guidelines/motion)
- [Apple SF Symbols](https://developer.apple.com/design/human-interface-guidelines/sf-symbols)
- [Apple Accessibility Guidelines](https://developer.apple.com/design/human-interface-guidelines/accessibility)
- [Designing for iOS](https://developer.apple.com/design/human-interface-guidelines/designing-for-ios)
- [WWDC 2025: Get to know the new design system](https://developer.apple.com/videos/play/wwdc2025/356/)
- [WWDC 2023: Animate with springs](https://developer.apple.com/videos/play/wwdc2023/10158/)
- [iOS System Colors Reference (Noah Gilmore)](https://noahgilmore.com/blog/dark-mode-uicolor-compatibility)
- [iOS Font Size Guidelines (Learn UI Design)](https://www.learnui.design/blog/ios-font-size-guidelines.html)
- [iOS Design Guidelines: Illustrated Patterns (Learn UI Design)](https://www.learnui.design/blog/ios-design-guidelines-templates.html)
- [Recreating Liquid Glass with CSS (DEV Community)](https://dev.to/kevinbism/recreating-apples-liquid-glass-effect-with-pure-css-3gpl)
- [Apple's Liquid Glass UI design + CSS guide (DEV Community)](https://dev.to/gruszdev/apples-liquid-glass-revolution-how-glassmorphism-is-shaping-ui-design-in-2025-with-css-code-1221)
- [Getting Clarity on Apple's Liquid Glass (CSS-Tricks)](https://css-tricks.com/getting-clarity-on-apples-liquid-glass/)
- [iOS Accessibility Best Practices 2025 (Medium)](https://medium.com/@david-auerbach/ios-accessibility-guidelines-best-practices-for-2025-6ed0d256200e)
