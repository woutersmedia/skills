---
name: coding-guidelines
description: Wouters Media coding standards index. Use when writing or reviewing code in any project. Points to individual rule files for React/TypeScript style, DRY, tests, styling, database, component architecture, and i18n. Triggers on tasks involving components, API routes, database, tests, or general project standards.
---

# Coding Guidelines

Index of Wouters Media coding rules. Each rule is defined once in its own file — read the relevant file for details and examples.

## Core Standards

| Rule file                         | Description                                                   |
| --------------------------------- | ------------------------------------------------------------- |
| `coding-standards.md`             | TypeScript strict typing, exports, React imports, file naming |
| `rules/general.md`                | Naming conventions, error handling, file size limits          |
| `rules/dry.md`                    | DRY principles, avoiding duplication                          |
| `rules/use-effect.md`             | When to avoid `useEffect` and what to use instead             |
| `rules/component-architecture.md` | `components/` vs `containers/` vs `app/` separation          |
| `rules/testing.md`                | Test conventions with React Testing Library                   |
| `rules/styling.md`                | Tailwind-only styling, design tokens                          |
| `rules/database-schema.md`        | ORM conventions, schema naming, typed queries                 |

## i18n & Middleware

| Rule file                        | Description                                       |
| -------------------------------- | ------------------------------------------------- |
| `rules/multilingual-labels.md`   | No hardcoded language text — all strings via i18n |
| `rules/middleware-prohibition.md`| Never create `middleware.ts` — use `proxy.ts`     |

## Optional / Project-Specific

These rules apply when the project uses the relevant technology or pattern:

| Rule file                   | Description                                             |
| --------------------------- | ------------------------------------------------------- |
| `optional/typography.md`    | `<Heading>` and `<Paragraph>` component usage           |
| `optional/buttons-links.md` | `<Button>` and `<AppLink>` component usage              |
| `optional/locale.md`        | Locale-prefixed routes and absolute API URLs            |

