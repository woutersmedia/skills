# DRY — Don't Repeat Yourself

## Rule

**Keep It Simple and Don't Repeat Yourself.** Every piece of logic should have a single, authoritative representation in the codebase.

## Principles

- Remove unused code, props, variables, and imports immediately.
- Extract repeated logic into a shared utility or hook — but only when it is used in **two or more** places.
- Prefer composing small, focused functions over large, multi-purpose ones.
- Minimize abstraction layers; a direct solution is almost always clearer than a generic one.

## Common Violations

### Duplicated fetch logic

```typescript
// ❌ AVOID — same fetch pattern copy-pasted across containers
// UserContainer.tsx
const res = await fetch(new URL("/api/users", window.location.origin).toString());
const users = await res.json();

// OrderContainer.tsx
const res = await fetch(new URL("/api/orders", window.location.origin).toString());
const orders = await res.json();

// ✅ CORRECT — shared utility
// utils/apiFetch.ts
export const apiFetch = <T>(path: string): Promise<T> =>
  fetch(new URL(path, window.location.origin).toString()).then((r) => r.json());

// UserContainer.tsx
const users = await apiFetch<User[]>("/api/users");
```

### Duplicated type definitions

```typescript
// ❌ AVOID — same shape defined in two files
// components/UserCard.tsx
interface User { id: string; name: string; email: string; }

// containers/UserContainer.tsx
interface User { id: string; name: string; email: string; }

// ✅ CORRECT — single source of truth
// types/User.ts
export interface User { id: string; name: string; email: string; }
```

### Unused code

Remove dead code rather than commenting it out. Version control preserves history.

```typescript
// ❌ AVOID
// const oldHandler = () => { ... };
export const handler = () => { ... };

// ✅ CORRECT
export const handler = () => { ... };
```

## When NOT to Deduplicate

Do not create an abstraction for code that appears similar but serves different domains or may diverge in the future. Premature abstraction creates tight coupling and makes future changes harder.
