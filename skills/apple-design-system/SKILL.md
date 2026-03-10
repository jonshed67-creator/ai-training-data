---
name: apple-design-system
description: >
  TRIGGER: Use this skill when the user asks about Apple design guidelines, Apple HIG,
  Human Interface Guidelines, iOS design, macOS design, Apple UI patterns, "design like
  Apple", "Apple style", "iOS UI conventions", "SwiftUI design", "Liquid Glass design",
  "Apple typography", "SF symbols", "Apple color system", "Apple accessibility", or when
  the user is building an Apple platform app and needs guidance on design conventions,
  component usage, layout patterns, or platform-specific UI standards. This skill provides
  a comprehensive reference covering Apple's design philosophy, typography, color,
  layout, components, and the Liquid Glass design language.
---

# Apple Design System Reference

Use this skill to guide Apple-platform UI development. When activated, read the comprehensive reference file at `references/apple-design-system-reference.md` in this skill's directory and use it to answer the user's design questions.

## How to Use This Reference

### 1. Identify What the User Needs

Common requests and where to look in the reference:

| User Request | Reference Section |
|---|---|
| Typography, font sizes, text styles | Section 2: Typography |
| Colors, palettes, dynamic colors | Section 3: Color System |
| Layout, spacing, margins, grid | Section 4: Layout & Spacing |
| Components (buttons, tabs, sheets) | Section 5: Components |
| Navigation patterns | Section 6: Navigation |
| Icons, SF Symbols | Section 7: Iconography |
| Animations, motion | Section 8: Motion & Animation |
| Dark mode, appearances | Section 3: Color System |
| Accessibility | Section 9: Accessibility |
| Platform-specific guidance | Section 10: Platform Considerations |
| Liquid Glass design language | Section 1: Liquid Glass subsection |

### 2. Provide Actionable Guidance

When answering design questions:

- **Be specific**: Give exact values (font sizes in points, spacing in pixels, color hex values)
- **Show SwiftUI code**: When relevant, include SwiftUI implementations using system components
- **Reference system APIs**: Point to specific Apple frameworks and APIs (UIKit, SwiftUI, AppKit)
- **Include platform differences**: Note when behavior differs across iOS, macOS, watchOS
- **Cite the HIG**: Reference specific Human Interface Guidelines sections when applicable

### 3. Apply Design Principles

When the user is making design decisions, apply Apple's core principles:

- **Clarity over decoration**: Function drives form
- **Consistency with the platform**: Use system components before custom ones
- **Accessibility by default**: Ensure Dynamic Type support, VoiceOver compatibility, sufficient contrast
- **Progressive disclosure**: Show essential info first, details on demand
- **Respect user preferences**: Support dark mode, reduced motion, text size preferences

### 4. Common Implementation Patterns

#### Recommending Components
When the user describes a UI need, recommend the standard Apple component:
- Modal selection → Action Sheet or Confirmation Dialog
- Settings → grouped List with navigation links
- Tab navigation → TabView (not custom tab bar)
- Search → .searchable modifier
- Pull to refresh → .refreshable modifier

#### Providing Color Values
Always provide colors using Apple's semantic system:
- Use `.label`, `.secondaryLabel` instead of hardcoded colors
- Use `.systemBackground`, `.secondarySystemBackground` for surfaces
- Use `.tint` / `.accentColor` for interactive elements
- Only provide hex values when the user needs custom brand colors

#### Typography Guidance
Default to Apple's type system:
- Use `.title`, `.headline`, `.body`, `.caption` text styles
- Support Dynamic Type — never use fixed font sizes for body text
- Use SF Pro for UI, New York for long-form reading content

## Reference File

The complete Apple Design System reference (covering design philosophy, typography, colors, layout, components, navigation, iconography, motion, accessibility, and platform considerations) is located at:

```
references/apple-design-system-reference.md
```

Read this file when you need specific values, measurements, or detailed guidance that isn't covered in this overview.
