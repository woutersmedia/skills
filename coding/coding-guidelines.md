---
name: coding-guidelines
description: Wouters Media coding standards. Use when writing or reviewing code in any project. CRITICAL never create, suggest, or mention middleware.ts—it is forbidden; routing uses apps/proxy.ts only. CRITICAL all user-visible labels must use the i18n system (never hardcode Dutch or any language); all code must be written in English. Covers React/TypeScript style, DRY, tests, typography/buttons/styling, Prisma, component architecture (components vs containers), and API calls with absolute URLs. Triggers on tasks involving components, API routes, Prisma, tests, or CODING_GUIDELINES.
---

# Coding Guidelines

Project best practices from CODING_GUIDELINES. Apply when writing or reviewing code in this repository.

## Critical: middleware.ts is forbidden

**Never create, suggest, or mention `middleware.ts`.** Routing and auth use `apps/proxy.ts` only. Do not rename, move, or refactor that file. If Next.js complains about missing middleware, do not add middleware.ts—the user will handle it. See `rules/middleware-prohibition.md`.

## Critical: all labels must be multilingual — no hardcoded language text

**Never hardcode Dutch (or any language) text in components, containers, navigation labels, or module definitions.** All user-visible strings must use the i18n translation system via `getTranslation(locale, key)`. All code (variable names, comments, error messages) must be written in **English**. See `rules/multilingual-labels.md`.

## When to Apply

- Writing or refactoring React/TypeScript components
- Adding or changing API routes or client fetch calls
- Modifying Prisma schema or database code
- Adding tests or reviewing coverage
- Working with typography, buttons, links, or styling
- Organizing components vs containers or app folder structure
- Ensuring copy and tone match brand (community-first, inclusive)

## Rule Categories

| Category               | Impact   | Rule file                |
| ---------------------- | -------- | ------------------------ |
| Middleware prohibition | CRITICAL | `middleware-prohibition` |
| General style          | HIGH     | `general`                |
| DRY                    | HIGH     | `dry`                    |
| Testing                | HIGH     | `testing`                |
| Typography             | HIGH     | `typography`             |
| Buttons & links        | HIGH     | `buttons-links`          |
| Styling                | HIGH     | `styling`                |
| Database (Prisma)      | HIGH     | `database-schema`        |
| Component architecture | HIGH     | `component-architecture` |
| API & locale           | CRITICAL | `api-locale-prefixes`    |

## Quick Reference

- **React**: No `React.` prefix; import types explicitly. No `any`. Functional components, clear names. Files < 100 lines.
- **UI**: Use `<Heading>` / `<Paragraph>` from Typography; `<Button>` from Button. No raw `<h1>`–`<h6>`, `<p>`, `<button>`, `<a>`, or next/link `<Link>`.
- **Styling**: Tailwind only; no inline `style` prop (exceptions in listed UI files only).
- **Structure**: Presentation in `/components`, logic/API in `/containers`. No components in `app` (except page/layout/error/loading/not-found).
- **API**: Always absolute URLs for fetch: `new URL('/api/...', window.location.origin).toString()`.
- **Prisma**: After editing schema, run `npx prisma generate` (or `cd packages/db && npm run generate`).
- **i18n**: All user-visible text via `t(key)`. Add keys to both `en.json` and `nl.json`. Navigation labels use dot-notation keys. Code always in English.

## How to Use

Read the relevant rule file for details and examples:

```
rules/middleware-prohibition.md
rules/multilingual-labels.md
rules/general.md
rules/dry.md
rules/testing.md
rules/typography.md
rules/buttons-links.md
rules/styling.md
rules/database-schema.md
rules/component-architecture.md
rules/api-locale-prefixes.md
```

For the full project document, see repository root: `CODING_GUIDELINES.md`.
