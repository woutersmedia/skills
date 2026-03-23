# Styling

## Rule

**Tailwind CSS only. No inline `style` props, no CSS modules, no styled-components.**

## Tailwind

Use Tailwind utility classes exclusively for all styling.

```typescript
// ❌ FORBIDDEN — inline styles
<div style={{ marginTop: "16px", color: "#6b7280" }}>Content</div>

// ❌ FORBIDDEN — arbitrary CSS-in-JS
const styles = { container: { padding: "1rem" } };
<div style={styles.container}>Content</div>

// ✅ CORRECT — Tailwind utilities
<div className="mt-4 text-muted-foreground">Content</div>
```

## Exceptions

Inline `style` is allowed **only** in the following UI primitive files where dynamic values are unavoidable:
- `components/ui/progress.tsx` — dynamic width/height percentages
- `components/ui/slider.tsx` — dynamic thumb position
- Any component where the value is a runtime variable that cannot be expressed as a static Tailwind class

```typescript
// Acceptable exception — dynamic value only known at runtime
<div style={{ width: `${percentage}%` }} />
```

## Design Tokens

Always use design-token classes (`text-primary`, `bg-background`, `border-border`, etc.) instead of raw color classes. This ensures theme switching works correctly.

```typescript
// ❌ AVOID — hardcoded color
<p className="text-gray-500">Hint text</p>

// ✅ CORRECT — design token
<p className="text-muted-foreground">Hint text</p>
```

## Responsive Design

Use Tailwind's responsive prefixes consistently. Mobile-first:

```typescript
// ✅ CORRECT — mobile-first, then override for larger screens
<div className="flex flex-col gap-4 md:flex-row md:gap-6">
```

## Do Not

- Do **not** override Tailwind component classes in arbitrary ways — extend via `cn()` or variants.
- Do **not** use `!important` (`!` prefix in Tailwind) unless there is no other option.
- Do **not** add CSS files or modules for application-level components.
