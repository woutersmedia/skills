# skills

Wouters Media AI agent skills, organised by category. These skills apply to all AI agents (GitHub Copilot, Claude, Cursor, etc.), not just Copilot.

## 📁 Structure

```
coding/                            # React / TypeScript / Next.js standards
  coding-standards.md              # Core standards: strict typing, exports, imports, server-first
  coding-guidelines.md             # Index: links to all rule files
  rules/
    general.md                     # TypeScript & React style basics, naming, error handling
    dry.md                         # DRY principles and avoiding duplication
    testing.md                     # Testing practices with React Testing Library
    use-effect.md                  # When and how to avoid useEffect
    styling.md                     # Tailwind-only styling rules
    database-schema.md             # ORM conventions, typed queries (Prisma recommended)
    component-architecture.md      # components/ vs containers/ vs app/ separation
    middleware-prohibition.md      # Never use middleware.ts — use proxy.ts
    multilingual-labels.md         # i18n: no hardcoded language text in UI

optional/                          # Project-specific / nice-to-have rules
  typography.md                    # <Heading> and <Paragraph> component usage
  buttons-links.md                 # <Button> and <AppLink> component usage
  locale.md                        # Locale-prefixed routes and absolute API URLs

docs/
  docs-maintenance.md              # Docs app: adding pages, syncing SDK (Nextra recommended)

design/
  web-design-guidelines.md         # UI review against Vercel Web Interface Guidelines

tooling/
  package-manager.md               # pnpm preference, monorepo workspace commands
  github-actions.md                # GitHub Actions optimisation: caching, DRY workflows, KISS
  turbopack.md                     # Turbopack / Turborepo: caching, task pipelines, optimisation
```

## 🚀 Skills at a Glance

| Skill | Category | Description |
|-------|----------|-------------|
| `coding-standards` | Coding | Core TypeScript/React rules, exports, file naming |
| `coding-guidelines` | Coding | Index of all rule files |
| `docs-maintenance` | Docs | Adding/updating doc pages and syncing SDK types |
| `web-design-guidelines` | Design | Accessibility & UX audit against Vercel guidelines |
| `package-manager` | Tooling | pnpm preference and monorepo workspace commands |
| `github-actions` | Tooling | GitHub Actions optimisation: caching, DRY, KISS, fewer build minutes |
| `turbopack` | Tooling | Turbopack / Turborepo caching, task pipelines, and build optimisation |

