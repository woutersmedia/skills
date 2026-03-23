# Buttons & Links

## Rule

**Never use raw `<button>`, `<a>`, or Next.js `<Link>` tags directly.** Always use the shared `<Button>` and `<AppLink>` UI components.

## Button Component

```typescript
// ❌ FORBIDDEN
<button onClick={handleSave} className="px-4 py-2 bg-primary text-white rounded">
  Save
</button>

// ✅ CORRECT
import { Button } from "@/components/ui/button";

<Button onClick={handleSave}>Save</Button>
<Button variant="outline" onClick={handleCancel}>Cancel</Button>
<Button variant="destructive" disabled={isLoading}>Delete</Button>
```

Available variants (check `components/ui/button.tsx` for the full list):
- `default` — primary action
- `outline` — secondary action
- `ghost` — subtle/tertiary action
- `destructive` — irreversible/dangerous action
- `link` — inline text link style

## Links

```typescript
// ❌ FORBIDDEN
<a href="/dashboard">Go to dashboard</a>
import Link from "next/link";
<Link href="/dashboard">Go to dashboard</Link>

// ✅ CORRECT
import { AppLink } from "@/components/ui/app-link";

<AppLink href="/dashboard">Go to dashboard</AppLink>
```

Use `Button` with `asChild` and `AppLink` for button-styled navigation:

```typescript
import { Button } from "@/components/ui/button";
import { AppLink } from "@/components/ui/app-link";

<Button asChild>
  <AppLink href="/dashboard">Go to dashboard</AppLink>
</Button>
```

## External Links

External links must open in a new tab with proper `rel` attributes. Use `AppLink` with the `external` prop:

```typescript
<AppLink href="https://example.com" external>
  Visit example.com
</AppLink>
```

## Do Not

- Do **not** add `onClick` to `<div>` or `<span>` for interactivity — use `<Button>`.
- Do **not** use `cursor-pointer` classes to make non-interactive elements look clickable.
- Do **not** style buttons with arbitrary Tailwind classes — use variants instead.
