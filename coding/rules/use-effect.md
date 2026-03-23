# Avoid `useEffect` — Prefer Alternatives

**`useEffect` is discouraged as a default choice.** Most common usages fall into one of two fixable patterns.

## Derived / computed values → use `useMemo`

Computing something from props or state inside a `useEffect` + `setState` pair introduces an unnecessary extra render cycle and hidden data-flow. Use `useMemo` instead.

```typescript
// ❌ AVOID — derived value computed in useEffect
"use client";
import { useState, useEffect } from "react";

export const ItemFilter = ({ items }: { items: Item[] }) => {
  const [searchTerm, setSearchTerm] = useState("");
  const [filtered, setFiltered] = useState(items);

  useEffect(() => {
    setFiltered(items.filter((i) => i.name.includes(searchTerm)));
  }, [items, searchTerm]);

  return <ItemList items={filtered} />;
};

// ✅ CORRECT — useMemo for derived values (no extra render, no stale-state risk)
"use client";
import { useState, useMemo } from "react";

export const ItemFilter = ({ items }: { items: Item[] }) => {
  const [searchTerm, setSearchTerm] = useState("");

  const filtered = useMemo(
    () => items.filter((i) => i.name.includes(searchTerm)),
    [items, searchTerm],
  );

  return <ItemList items={filtered} />;
};
```

## Data fetching → use server components

Fetching data on mount in a `useEffect` is a common anti-pattern in Next.js App Router projects. Move the fetch to a server component instead.

```typescript
// ❌ AVOID — client-side data fetching in useEffect
"use client";
import { useState, useEffect } from "react";

export const ItemList = () => {
  const [items, setItems] = useState<Item[]>([]);

  useEffect(() => {
    fetch("/api/items")
      .then((r) => r.json())
      .then(setItems);
  }, []);

  return <ul>{items.map((i) => <li key={i.id}>{i.name}</li>)}</ul>;
};

// ✅ CORRECT — server component fetches data directly (no useEffect, no extra request)
export const ItemListPage = async () => {
  const items = await getItems();
  return <ul>{items.map((i) => <li key={i.id}>{i.name}</li>)}</ul>;
};
```

## When `useEffect` is still acceptable

Only use `useEffect` when you genuinely need to **synchronise with an external system** that cannot be handled any other way — for example:

- Subscribing to a third-party event emitter or WebSocket
- Calling an imperative browser/DOM API (e.g. initialising a map SDK, managing focus)
- Integrating with a non-React animation library

Always include a cleanup function and document why no alternative is possible.

```typescript
// ✅ Acceptable — synchronising with an external imperative API
import { useEffect, useRef } from "react";

export const MapContainer = ({ apiKey }: { apiKey: string }) => {
  const ref = useRef<HTMLDivElement>(null);

  useEffect(() => {
    if (!ref.current) return;
    const map = ThirdPartyMapSDK.init(ref.current, { apiKey });
    return () => map.destroy(); // always clean up
  }, [apiKey]);

  return <div ref={ref} />;
};
```
