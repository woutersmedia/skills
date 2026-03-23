# API & Locale Prefixes

## API Calls — Always Absolute URLs

When calling the API from client components, always construct absolute URLs using `window.location.origin`. Relative URLs break in certain server-side and proxy contexts.

```typescript
// ❌ FORBIDDEN — relative URL
const res = await fetch("/api/users");

// ✅ CORRECT — absolute URL
const res = await fetch(new URL("/api/users", window.location.origin).toString());
```

Prefer extracting this into a shared utility to avoid repetition:

```typescript
// utils/apiFetch.ts
export const apiFetch = <T>(path: string, init?: RequestInit): Promise<T> =>
  fetch(new URL(path, window.location.origin).toString(), init).then((r) => {
    if (!r.ok) throw new Error(`API error ${r.status}`);
    return r.json() as Promise<T>;
  });

// Usage
const users = await apiFetch<User[]>("/api/users");
```

## Locale-Prefixed Routes

All user-facing routes live under `app/[lang]/`. The `lang` segment drives the active locale for the entire route tree.

```
app/
  [lang]/
    layout.tsx      ← sets up DictionaryProvider with the correct locale
    page.tsx        ← home page
    dashboard/
      page.tsx
    settings/
      page.tsx
```

**Never** place user-facing pages outside `[lang]/`. API routes (`app/api/`) are exempt.

## Reading the Locale in Pages

```typescript
// ✅ CORRECT — page receives locale via params
interface PageProps {
  params: { lang: string };
}

export default async function Page({ params: { lang } }: PageProps) {
  const t = await getTranslation(lang, "common");
  return <Heading size={HeadingTypes.H1}>{t("title")}</Heading>;
}
```

## Generating Static Locale Paths

When using `generateStaticParams`, export all supported locales:

```typescript
import { locales } from "@/i18n/config";

export const generateStaticParams = () =>
  locales.map((lang) => ({ lang }));
```

## Do Not

- Do **not** hardcode locale strings (`"en"`, `"nl"`) anywhere except the i18n config.
- Do **not** read `navigator.language` or `Accept-Language` headers in components — use the route segment instead.
- Do **not** call the API without error handling — always check `res.ok` or use the shared `apiFetch` utility.
