---
name: typography
description: >
  Optional project-specific skill. Use when the project provides <Heading> and <Paragraph>
  UI components. Enforces use of these components over bare HTML heading and paragraph tags.
  Apply when asked to add or update headings, paragraphs, or other text in a project that
  uses this component system.
metadata:
  internal: true
---

# Typography

## Rule

**Never use bare HTML heading or paragraph tags. Always use the `<Heading>` and `<Paragraph>` UI components.**

## Heading Component

Import `Heading` and `HeadingTypes` from the shared UI library. Use:
- `as` prop → controls the rendered HTML element (semantic)
- `size` prop → controls the visual appearance

```typescript
// ❌ FORBIDDEN
<h1 className="text-xl font-semibold tracking-tight">Title</h1>
<h2 className="text-2xl font-semibold">Section</h2>
<h3>Subsection</h3>

// ✅ CORRECT
import { Heading, HeadingTypes } from "@/components/ui/heading";

<Heading as="h1" size={HeadingTypes.H3}>Title</Heading>
<Heading size={HeadingTypes.H2}>Section</Heading>
<Heading as="h3" size={HeadingTypes.H3}>Subsection</Heading>
```

Available `HeadingTypes`: `H1`, `H2`, `H3`, `H4`, `H5`, `H6`

## Paragraph Component

Import `Paragraph` and `ParagraphTypes`. Use:
- `as` prop → controls the rendered HTML element
- `variant` prop → controls the visual style
- `className` → only for spacing, layout, or minor color overrides

```typescript
// ❌ FORBIDDEN
<p className="text-muted-foreground text-sm">Subtitle</p>
<p className="text-base leading-relaxed">Body text</p>
<span className="text-sm">Small text</span>

// ✅ CORRECT
import { Paragraph, ParagraphTypes } from "@/components/ui/paragraph";

<Paragraph variant={ParagraphTypes.MUTED}>Subtitle</Paragraph>
<Paragraph variant={ParagraphTypes.DEFAULT}>Body text</Paragraph>
<Paragraph as="span" variant={ParagraphTypes.SMALL}>Small text</Paragraph>

// Spacing override — acceptable
<Paragraph variant={ParagraphTypes.MUTED} className="mb-2 font-medium">Label</Paragraph>
```

Available `ParagraphTypes`:

| Type      | Visual style                             |
| --------- | ---------------------------------------- |
| `DEFAULT` | `text-base leading-relaxed`              |
| `MUTED`   | `text-muted-foreground text-sm`          |
| `SMALL`   | `text-sm`                                |
| `LABEL`   | `text-muted-foreground text-xs font-medium` |

## Enforcement

When a project provides `<Heading>` and `<Paragraph>` components, prefer them over bare HTML tags and typography `className` combinations. The specific constraints below apply to projects that adopt this system:

- Bare `<h1>`–`<h6>` tags should be avoided in favour of the `<Heading>` component.
- Bare `<p>` tags with typography `className` should be avoided in favour of `<Paragraph>`.
- Do **not** use `className` on `<Heading>` / `<Paragraph>` to recreate a variant that already exists.

> **Note:** If the project does not use these components, use semantic HTML with Tailwind classes directly. Check the project's `components/ui/` directory to confirm what is available.
