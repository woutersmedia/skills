# Component Architecture

## Rule

**Presentation in `/components`. Logic and data in `/containers`. Nothing else in `app/` except framework files.**

## Components (`/components`)

Components are **dumb and presentational**. They:

- Accept all data via props
- Return JSX
- Have no side effects, no data fetching, no API calls
- Do not access global state, context (except UI context like themes), or stores
- Produce the same output for the same props (deterministic)
- Do **not** import from `/containers`, data-fetching hooks, or stores

```typescript
// ✅ CORRECT — pure presentational component
interface UserCardProps {
  name: string;
  email: string;
  avatarUrl?: string;
}

export const UserCard = ({ name, email, avatarUrl }: UserCardProps) => (
  <div className="flex items-center gap-3 rounded-lg border p-4">
    {avatarUrl && <img src={avatarUrl} alt={name} className="h-10 w-10 rounded-full" />}
    <div>
      <Heading size={HeadingTypes.H5}>{name}</Heading>
      <Paragraph variant={ParagraphTypes.MUTED}>{email}</Paragraph>
    </div>
  </div>
);
```

## Containers (`/containers`)

Containers are **smart orchestrators**. They:

- Fetch data (server-side preferred) or call mutations
- Contain business logic
- Access global state, context, and stores
- Pass data down to components via props
- Delegate all complex rendering to components
- Do **not** contain complex styling logic

```typescript
// ✅ CORRECT — server container fetches data and passes to component
export const UserProfileContainer = async ({ userId }: { userId: string }) => {
  const user = await getUser(userId);
  return <UserCard name={user.name} email={user.email} avatarUrl={user.avatarUrl} />;
};
```

## App Directory (`/app`)

Only the following file types are allowed directly in `app/`:
- `page.tsx`
- `layout.tsx`
- `error.tsx`
- `loading.tsx`
- `not-found.tsx`
- `route.ts` (API routes)

**No custom components in `app/`.** Move them to `/components` or `/containers`.

```
// ❌ FORBIDDEN
app/
  dashboard/
    page.tsx
    DashboardStats.tsx   ← not allowed here

// ✅ CORRECT
app/
  dashboard/
    page.tsx             ← imports from containers/
components/
  DashboardStats/
    DashboardStats.tsx
containers/
  DashboardStatsContainer.tsx
```

## File Naming & Location

- `components/ComponentName/ComponentName.tsx` — PascalCase folder + matching filename
- `containers/ContainerName.tsx` — PascalCase, no subfolder needed unless complex
- Always `export const`, never `export default` for components (pages are the exception)
