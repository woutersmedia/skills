# skills

Wouters Media AI agent skills, organised by category. These skills apply to all AI agents (GitHub Copilot, Claude, Cursor, etc.), not just Copilot.

## Install

```bash
npx skills add woutersmedia/skills
```

## 📁 Structure

```
skills/                            # Installable skills (npx skills add woutersmedia/skills)
  coding-standards/SKILL.md        # Core standards: strict typing, exports, imports, server-first
  coding-guidelines/SKILL.md       # Comprehensive guide: all rules inline (React, DRY, tests, i18n, etc.)
  web-design-guidelines/SKILL.md   # UI review against Vercel Web Interface Guidelines
  docs-maintenance/SKILL.md        # Docs app: adding pages, syncing SDK (Nextra recommended)
  package-manager/SKILL.md         # pnpm preference, monorepo workspace commands
  github-actions/SKILL.md          # GitHub Actions optimisation: caching, DRY workflows, KISS
  turbopack/SKILL.md               # Turbopack / Turborepo: caching, task pipelines, optimisation
  typography/SKILL.md              # Optional: <Heading> and <Paragraph> component usage
  buttons-links/SKILL.md           # Optional: <Button> and <AppLink> component usage
  locale/SKILL.md                  # Optional: locale-prefixed routes and absolute API URLs

coding/                            # Source documentation
  coding-standards.md
  coding-guidelines.md
  rules/                           # Individual rule files referenced by coding-guidelines

optional/                          # Source documentation for optional/project-specific rules

docs/
  docs-maintenance.md

design/
  web-design-guidelines.md

tooling/
  package-manager.md
  github-actions.md
  turbopack.md
```

## 🚀 Skills at a Glance

| Skill | Category | Description |
|-------|----------|-------------|
| `coding-standards` | Coding | Core TypeScript/React rules, exports, file naming |
| `coding-guidelines` | Coding | Comprehensive guide: all rules inline (DRY, tests, styling, DB, i18n, middleware) |
| `web-design-guidelines` | Design | Accessibility & UX audit against Vercel guidelines |
| `docs-maintenance` | Docs | Adding/updating doc pages and syncing SDK types |
| `package-manager` | Tooling | pnpm preference and monorepo workspace commands |
| `github-actions` | Tooling | GitHub Actions optimisation: caching, DRY, KISS, fewer build minutes |
| `turbopack` | Tooling | Turbopack / Turborepo caching, task pipelines, and build optimisation |
| `typography` | Optional | `<Heading>` and `<Paragraph>` component usage (project-specific) |
| `buttons-links` | Optional | `<Button>` and `<AppLink>` component usage (project-specific) |
| `locale` | Optional | Locale-prefixed routes and absolute API URLs (project-specific) |

Optional skills are hidden by default. Install them explicitly when the project uses that component system:

```bash
INSTALL_INTERNAL_SKILLS=1 npx skills add woutersmedia/skills --skill typography
INSTALL_INTERNAL_SKILLS=1 npx skills add woutersmedia/skills --skill buttons-links
INSTALL_INTERNAL_SKILLS=1 npx skills add woutersmedia/skills --skill locale
```

