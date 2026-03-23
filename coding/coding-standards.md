---
name: coding-standards
description: Wouters Media coding standards - TypeScript strict typing, export const components, no FC, no React namespace, component/container separation, server-side first, localization requirements, Heading/Paragraph components. Avoid useEffect — prefer useMemo for derived values and server components for data fetching.
---

# Wouters Media Coding Standards

Apply these rules whenever writing or reviewing code in Wouters Media projects.

## TypeScript Strict Typing

**CRITICAL: No `any`, `unknown`, or `never` types.**

```typescript
// ❌ FORBIDDEN
function processData(data: any) { return data.value; }

// ✅ CORRECT
interface Data { value: string; }
function processData(data: Data): string { return data.value; }
```

Error handling:

```typescript
// ❌ FORBIDDEN
} catch (error: any) { console.log(error.message); }

// ✅ CORRECT
} catch (error) {
  if (error instanceof Error) {
    console.log(error.message);
  }
}
```

## Component Export Style

**Always use `export const`, never `export function` for components and utilities.**

```typescript
// ❌ FORBIDDEN
export function Button({ children }: ButtonProps) { ... }

// ✅ CORRECT
export const Button = ({ children }: ButtonProps) => { ... };
```

## React Imports — No `React.` Namespace

**Never use `React.FC`, `React.ReactNode`, `React.useState`, etc. Import directly.**

```typescript
// ❌ FORBIDDEN
import React from 'react';
const Component: React.FC<Props> = ({ children }) => { ... };

// ❌ FORBIDDEN
const Component: FC<Props> = ({ children }) => { ... };

// ✅ CORRECT
import { type ReactNode } from 'react';
export const Component = ({ children }: { children: ReactNode }) => { ... };
```

Common React type imports (never import `FC` or `FunctionComponent`):

```typescript
import {
  type ReactNode,
  type ComponentProps,
  type ChangeEvent,
  type FormEvent,
  useState,
  useMemo,
  useRef,
} from "react";
```

> ⚠️ **`useEffect` is deprecated in this project.** See the [Avoid `useEffect`](#avoid-useeffect--prefer-alternatives) section below.

## Component Structure

Props interface defined above the component, typed inline:

```typescript
interface ButtonProps {
  children: ReactNode;
  onClick?: () => void;
  disabled?: boolean;
  variant?: 'primary' | 'secondary';
}

export const Button = ({
  children,
  onClick,
  disabled = false,
  variant = 'primary',
}: ButtonProps) => {
  return (
    <button onClick={onClick} disabled={disabled}>
      {children}
    </button>
  );
};
```

## Component / Container Separation

**Components (`components/`)** — Dumb, presentational only:
- Accept data through props only
- No data fetching, no API calls
- No global state access
- No side effects
- Deterministic: same props → same output
- Do NOT import from `containers/`, data-fetching `hooks/`, or `stores/`

**Containers (`containers/`)** — Smart, orchestration:
- Handle data fetching and mutations
- Contain business logic
- Access global state and context
- Coordinate multiple components
- No complex styling logic (delegate to components)

## Server-Side First

Prefer server components and server-side logic wherever possible:

```typescript
// ✅ CORRECT — server component (default, no directive)
export default async function Page() {
  const data = await fetchFromDatabase();
  return <Component data={data} />;
}

// Only use client components when interactivity is required
"use client";
```

## Localization

All user-facing text must be localized:

- All routes under `app/[lang]/`
- Use `useDictionary()` — never hardcode translatable strings
- Add new strings to **both** `en.json` and `nl.json` simultaneously

The `useDictionary()` hook reads from the `DictionaryProvider` context, which is set up in the `[lang]` layout and automatically receives the correct language from the route segment. No language parameter is needed in the hook itself.

```typescript
// ❌ FORBIDDEN
<h1>Welcome</h1>

// ✅ CORRECT — useDictionary() reads language from DictionaryProvider context
import { useDictionary } from "@/providers/Dictionary";

const dict = useDictionary();
<h1>{dict.welcome}</h1>
```

## File Naming

- Components: `PascalCase.tsx` → `Button.tsx`, `UserProfile.tsx`
- Utilities: `camelCase.ts` → `formatDate.ts`, `apiClient.ts`
- Types: `PascalCase.ts` → `User.ts`, `Booking.ts`
- Tests: `*.test.ts` or `*.test.tsx`

## Import Paths

- Components: `@/components/Example/Example` (PascalCase folder + file)
- ShadCN UI: `@/components/ui/button` (lowercase, kebab-case)
- All paths use `@/` alias for absolute imports

## 🛠️ Implementation Standards

- **Project Artifacts**: Do **NOT** create `output.txt`, `tsconfig.tsbuildinfo`, or other temporary files. **NO `output.txt` is allowed at any time.**
- **KISS & DRY**: Keep it simple. Remove unused code, props, and imports. Minimize abstractions.
- **Strict Typing**: No `any`, `unknown`, or `never`. We want robust types.

## Avoid `useEffect` — Prefer Alternatives

**`useEffect` is deprecated in this project. Do not reach for it as a default choice.**

Most common `useEffect` usages fall into one of two fixable patterns:

### Derived / computed values → use `useMemo`

Computing something from props or state inside a `useEffect` + `setState` pair introduces an unnecessary extra render cycle and hidden data-flow. Use `useMemo` instead.

```typescript
// ❌ AVOID — derived value computed in useEffect
"use client";
import { useState, useEffect } from "react";

export const BookingFilter = ({ bookings }: { bookings: Booking[] }) => {
  const [searchTerm, setSearchTerm] = useState("");
  const [filtered, setFiltered] = useState(bookings);

  useEffect(() => {
    setFiltered(bookings.filter((b) => b.name.includes(searchTerm)));
  }, [bookings, searchTerm]);

  return <BookingList bookings={filtered} />;
};

// ✅ CORRECT — useMemo for derived values (no extra render, no stale-state risk)
"use client";
import { useState, useMemo } from "react";

export const BookingFilter = ({ bookings }: { bookings: Booking[] }) => {
  const [searchTerm, setSearchTerm] = useState("");

  const filtered = useMemo(
    () => bookings.filter((b) => b.name.includes(searchTerm)),
    [bookings, searchTerm],
  );

  return <BookingList bookings={filtered} />;
};
```

### Data fetching → use server components

Fetching data on mount in a `useEffect` is a common anti-pattern in Next.js App Router projects. Move the fetch to a server component instead.

```typescript
// ❌ AVOID — client-side data fetching in useEffect
"use client";
import { useState, useEffect } from "react";

export const BookingList = () => {
  const [bookings, setBookings] = useState<Booking[]>([]);

  useEffect(() => {
    fetch("/api/bookings")
      .then((r) => r.json())
      .then(setBookings);
  }, []);

  return <ul>{bookings.map((b) => <li key={b.id}>{b.name}</li>)}</ul>;
};

// ✅ CORRECT — server component fetches data directly (no useEffect, no extra request)
export default async function BookingListPage() {
  const bookings = await getBookings();
  return <ul>{bookings.map((b) => <li key={b.id}>{b.name}</li>)}</ul>;
}
```

### When `useEffect` is still acceptable

Only use `useEffect` when you genuinely need to **synchronise with an external system** that cannot be handled any other way — for example:

- Subscribing to a third-party event emitter or WebSocket
- Calling an imperative browser/DOM API (e.g. initialising a map SDK, managing focus)
- Integrating with a non-React animation library

In those cases, always include a cleanup function and document why no alternative is possible.

```typescript
// ✅ Acceptable — synchronising with an external imperative API
import { useEffect, useRef } from "react";

export const MapContainer = ({ apiKey }: { apiKey: string }) => {
  const ref = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (!ref.current) return;
    const map = ThirdPartyMapSDK.init(ref.current, { apiKey });
    return () => map.destroy(); // always clean up
  }, [apiKey]);

  return <div ref={ref} />;
};
```

## References

- `CODING_GUIDELINES.md` — Full project coding standards
- `.github/STYLING_GUIDELINES.md` — UI styling rules

## Typography — `<Heading>` and `<Paragraph>` Components

**STRICT RULE: Never use bare HTML heading or paragraph tags in components. Always use the typed `<Heading>` and `<Paragraph>` UI components.**

### Heading

Import and use `HeadingTypes` for the `size` prop. The `as` prop controls the HTML tag (semantic), `size` controls the visual appearance.

```typescript
// ❌ FORBIDDEN
<h1 className="text-xl font-semibold tracking-tight">Title</h1>
<h2 className="text-2xl font-semibold">Section</h2>

// ✅ CORRECT
import { Heading, HeadingTypes } from "@/components/ui/heading";

// Renders an h1 element but styled like an h3
<Heading as="h1" size={HeadingTypes.H3}>Title</Heading>

// Renders an h2 (default) styled like an h2
<Heading size={HeadingTypes.H2}>Section</Heading>
```

Available `HeadingTypes`: `H1`, `H2`, `H3`, `H4`, `H5`, `H6`

### Paragraph

Import and use `ParagraphTypes` for the `variant` prop. The `as` prop controls the HTML tag, `variant` controls the visual style. Use `className` only for spacing or layout overrides.

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

// Extra spacing/layout via className — acceptable
<Paragraph variant={ParagraphTypes.MUTED} className="mb-2 font-medium">Label</Paragraph>
```

Available `ParagraphTypes`:
| Type | Visual style |
|------|-------------|
| `DEFAULT` | `text-base leading-relaxed` |
| `MUTED` | `text-muted-foreground text-sm` |
| `SMALL` | `text-sm` |
| `LABEL` | `text-muted-foreground text-xs font-medium` |

### ⚠️ Enforcement

- Bare `<h1>`–`<h6>` tags in components are **forbidden** (except inside `components/ui/heading.tsx` itself)
- Bare `<p>` tags with typography `className` are **forbidden** (except inside `components/ui/paragraph.tsx`)
- `className` on `<Heading>` / `<Paragraph>` is allowed only for spacing, layout, or minor color overrides — not for recreating a variant that already exists
