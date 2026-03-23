# Middleware Prohibition

## Rule

**Never create, suggest, or mention `middleware.ts`.**

Routing and authentication in this project are handled exclusively by `apps/proxy.ts`. This file must not be renamed, moved, or refactored.

## Why

Next.js `middleware.ts` conflicts with the existing proxy-based routing architecture. Introducing it would break authentication and routing for the entire application.

## What to Do Instead

- Route protection → handled in `apps/proxy.ts`
- Auth checks → handled in `apps/proxy.ts`
- Locale detection → handled in `apps/proxy.ts`

If Next.js warns about missing middleware, **do not** create `middleware.ts`. Inform the user that routing is intentionally managed via the proxy, and they will handle any Next.js configuration if needed.

## ❌ Forbidden

```typescript
// NEVER create this file
// apps/middleware.ts
import { NextResponse } from "next/server";
export function middleware(request: Request) { ... }
```

## ✅ Correct

```typescript
// All routing/auth logic lives here — do not touch
// apps/proxy.ts
```
