---
name: docs-maintenance
description: >
  Automates maintenance of the Nextra documentation app (apps/docs).
  Use when asked to update docs, add a new API endpoint to the docs, sync SDK
  types with the documentation, regenerate _meta.json, or add a new doc page.
  Triggers on tasks mentioning "update docs", "add endpoint to docs", "sync
  docs with SDK", "new doc page", or "regenerate _meta".
---

# Docs Maintenance

## Overview

The documentation lives in **`apps/docs`** — a standalone Next.js + Nextra 4.x app that is
served at `/docs` on the main domain (e.g. `example.co.nl/docs`) via a reverse-proxy rewrite in
`apps/web/next.config.ts`.

The documentation is **automatically kept in sync** with the SDK surface through two mechanisms:

1. **`generateDocsMeta` utility** (`packages/docs/src/index.ts`) — regenerates `_meta.json`
   from a canonical list of pages without manual editing.
2. **This skill** — provides step-by-step instructions for keeping MDX content in sync with
   SDK and API changes.

---

## File Locations

| Purpose                            | Path                                                     |
| ---------------------------------- | -------------------------------------------------------- |
| All documentation MDX pages        | `apps/docs/content/*.mdx`                                |
| Sidebar navigation metadata        | `apps/docs/content/_meta.json`                           |
| Nextra config (Next.js wrapper)    | `apps/docs/next.config.ts`                               |
| Root layout (fonts, Nextra Layout) | `apps/docs/app/layout.tsx`                               |
| MDX catch-all page renderer        | `apps/docs/app/[[...mdxPath]]/page.tsx`                  |
| Global CSS + color theme           | `apps/docs/app/globals.css`                              |
| Docs config helper                 | `packages/docs/src/index.ts`                             |
| Canonical page list constant       | `packages/docs/src/index.ts` → `DOCS_DEFAULT_PAGES`      |

---

## Running the Docs App Locally

```bash
# Start the docs app alone (port 3001)
pnpm --filter docs dev

# Start everything together — web at :3000, docs at :3001
pnpm dev
```

---

## Adding a New Documentation Page

### 1. Create the MDX file

```bash
# Create apps/docs/content/<page-key>.mdx
```

Use `apps/docs/content/authentication.mdx` as a reference template for API endpoint pages.

### 2. Register the page in `_meta.json`

Either edit `apps/docs/content/_meta.json` directly:

```json
{
  "index": "Introduction",
  "my-new-page": "My New Page"
}
```

Or regenerate it programmatically using `generateDocsMeta` from the docs package:

```ts
import { generateDocsMeta, DOCS_DEFAULT_PAGES } from '@your-org/docs';

const meta = generateDocsMeta([
  ...DOCS_DEFAULT_PAGES,
  { key: 'my-new-page', label: 'My New Page' },
]);
// write JSON.stringify(meta, null, 2) to apps/docs/content/_meta.json
```

### 3. Update `DOCS_DEFAULT_PAGES` in `packages/docs/src/index.ts`

Add the new entry to keep the canonical page list in sync:

```ts
export const DOCS_DEFAULT_PAGES: DocsMetaEntry[] = [
  // ... existing entries
  { key: 'my-new-page', label: 'My New Page' },
];
```

---

## Syncing Docs When the SDK Changes

When new types, endpoints, or methods are added to `packages/sdk/src/`:

1. **Identify the affected doc page** — each SDK method maps to a content file:
   - `getApps` / `getApp` → `apps/docs/content/apps.mdx`
   - `getContent` / `getContentItem` → `apps/docs/content/content.mdx`
   - `getProducts` / `getProduct` → `apps/docs/content/products.mdx`
   - `getPages` / `getPage` → `apps/docs/content/pages.mdx`
   - `getContentTypes` / `getContentTypeFields` → `apps/docs/content/content.mdx`

2. **Update the MDX page** with new endpoint docs, request/response examples, and
   any changed query parameters.

3. **If a new top-level resource is added**, create a new MDX page and register it
   (follow steps in "Adding a New Documentation Page").

---

## Styling Guidelines

The docs app uses brand colors applied to Nextra's CSS variables:

```css
/* In apps/docs/app/globals.css */
:root {
  --nextra-primary-hue: 26;
  --nextra-primary-saturation: 90%;
  --nextra-primary-lightness: 48%;
}
```

To change the accent color, update those three `--nextra-primary-*` variables only.
Do **not** add inline styles or override Nextra's utility classes directly.

---

## Deployment

The docs app is a separate Next.js deployment. In production, set the `DOCS_URL` environment
variable in the main web app to point to the deployed docs URL:

```
DOCS_URL=https://docs.example.co.nl
```

The `apps/web/next.config.ts` rewrite rule will forward all `/docs` traffic to `DOCS_URL`.
