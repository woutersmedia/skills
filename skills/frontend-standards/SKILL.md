---
name: frontend-standards
description: Coding standards for frontend projects covering TypeScript strictness, React best practices, general code quality, English-only source text, and multilingual support for user-facing content.
---

## TypeScript

- Never use `any`. Prefer `unknown` and narrow with type guards.
- Avoid unsafe type assertions (`as SomeType`) unless you can justify them with a comment.
- Enable and respect `strict` mode in `tsconfig.json` (`strictNullChecks`, `noImplicitAny`, etc.).
- Prefer explicit return types on exported functions and React components.

## React

- Avoid unnecessary `useEffect`. Derive state from existing state/props where possible; reach for `useEffect` only for genuine side-effects (data fetching, subscriptions, DOM mutations).
- Keep components small and focused on a single responsibility. Extract sub-components when JSX exceeds ~50 lines or when logic can be reused.
- Prefer controlled components over uncontrolled components for form elements.
- Memoize with `useMemo` / `useCallback` only when a measurable performance problem exists — do not memoize by default.

## Code Quality

- Write code for the next reader: choose clear, self-explaining names over brevity.
- Eliminate duplication by extracting shared logic into utility functions or custom hooks.
- Keep functions short; a function that does one thing is easier to test and understand.
- Delete commented-out code; rely on version control history instead.

## Language

- All identifiers, comments, commit messages, and documentation must be written in English.
- Never hard-code user-facing strings directly in components. Use a translation/i18n library (e.g. `react-i18next`, `next-intl`) so every string can be localised.
- Store translation keys in dedicated locale files (e.g. `locales/en.json`) and reference them by key in components.
- Provide at least an English (`en`) locale file as the baseline; add additional locales as needed.
