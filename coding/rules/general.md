# General React / TypeScript Style

## TypeScript

- **No `any`, `unknown`, or `never` types.** Use explicit interfaces and type unions.
- **No `React.` prefix.** Import types and hooks directly from `"react"`.
- **No `FC` or `FunctionComponent`.** Type props via an interface; let TypeScript infer the return type.
- Files should stay **under 100 lines**. Split large files into focused modules.

```typescript
// ❌ FORBIDDEN
import React from "react";
const MyComponent: React.FC<{ name: string }> = ({ name }) => <p>{name}</p>;

// ✅ CORRECT
import { type ReactNode } from "react";

interface MyComponentProps {
  name: string;
  children?: ReactNode;
}

export const MyComponent = ({ name, children }: MyComponentProps) => (
  <p>
    {name}
    {children}
  </p>
);
```

## Component Export Style

Always `export const`, never `export function` for components and utilities.

```typescript
// ❌ FORBIDDEN
export function Button({ children }: ButtonProps) { ... }

// ✅ CORRECT
export const Button = ({ children }: ButtonProps) => { ... };
```

## Naming Conventions

| Artifact   | Convention  | Example                  |
| ---------- | ----------- | ------------------------ |
| Component  | PascalCase  | `UserProfile.tsx`        |
| Container  | PascalCase  | `UserProfileContainer.tsx` |
| Utility    | camelCase   | `formatDate.ts`          |
| Type file  | PascalCase  | `User.ts`                |
| Test file  | `*.test.ts` | `UserProfile.test.tsx`   |

## Error Handling

```typescript
// ❌ FORBIDDEN
} catch (error: any) { console.error(error.message); }

// ✅ CORRECT
} catch (error) {
  if (error instanceof Error) {
    console.error(error.message);
  }
}
```

## Avoid `useEffect`

Prefer `useMemo` for derived values and server components for data fetching. See the Booker coding standards for detailed examples.

Only use `useEffect` for genuine external-system synchronisation (WebSockets, imperative DOM APIs, third-party SDK initialisation). Always include a cleanup function.
