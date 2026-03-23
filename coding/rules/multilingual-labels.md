# Multilingual Labels

## Rule

**Never hardcode Dutch (or any language) text.** All user-visible strings must use the i18n translation system.

All **code** (variable names, comments, function names, error messages in logs) must be written in **English**.

## i18n System

Use `getTranslation(locale, key)` in server components and `t(key)` (from the translation context/hook) in client components.

Always add new keys to **both** `en.json` and `nl.json` simultaneously.

## ❌ Forbidden

```typescript
// Hardcoded Dutch
<h1>Welkom bij CIMI</h1>
<button>Opslaan</button>
<nav label="Navigatie" />

// Hardcoded English (still forbidden for UI text)
<h1>Welcome to CIMI</h1>
```

```typescript
// Dutch variable/comment (code must be English)
const gebruikersNaam = "Jan";
// Haal de gebruiker op
```

## ✅ Correct

```typescript
// Server component
import { getTranslation } from "@/i18n/getTranslation";

export default async function Page({ params: { locale } }: PageProps) {
  const t = await getTranslation(locale, "common");
  return <h1>{t("welcome")}</h1>;
}
```

```typescript
// Client component
import { useTranslation } from "@/i18n/useTranslation";

export const SaveButton = () => {
  const { t } = useTranslation("common");
  return <button>{t("save")}</button>;
};
```

```json
// en.json
{ "common": { "welcome": "Welcome to CIMI", "save": "Save" } }

// nl.json
{ "common": { "welcome": "Welkom bij CIMI", "save": "Opslaan" } }
```

## Navigation Labels

Navigation items use dot-notation translation keys:

```typescript
// ❌ FORBIDDEN
{ label: "Instellingen", href: "/settings" }

// ✅ CORRECT
{ labelKey: "nav.settings", href: "/settings" }
```
