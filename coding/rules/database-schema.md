# Database Schema

## Rule

**Use a typed ORM or query builder for all database access.** Never write raw SQL unless there is no alternative — document the reason when you do.

Preferred tool: [Prisma](https://www.prisma.io/). Other typed alternatives (Drizzle, Kysely) are acceptable if the project already uses them. Check the project's `package.json` to confirm which ORM is in use.

## After Editing the Schema

When using Prisma, always regenerate the client after modifying `schema.prisma`:

```bash
# From the project root
npx prisma generate

# Or from the db package directory
cd packages/db && npm run generate
```

Never commit code that relies on generated client types without ensuring the client is up to date.

## Schema Conventions

Apply these conventions regardless of ORM:

- Model/table names: **PascalCase** in the ORM model, snake_case in the database (`@@map("users")` in Prisma)
- Field/column names: **camelCase** in code, snake_case in the database
- Always include `createdAt` and `updatedAt` timestamp fields
- Use a generated unique ID (`cuid`, `uuid`) as the primary key

```prisma
// ❌ AVOID — unclear naming, no table mapping
model user_data {
  i    Int    @id
  n    String
  eml  String
}

// ✅ CORRECT — clear names, camelCase fields, explicit table name
model User {
  id        String   @id @default(cuid())
  name      String
  email     String   @unique
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@map("users")
}
```

## Querying

Prefer the ORM's typed API over raw queries:

```typescript
// ❌ AVOID — raw query bypasses type safety
const users = await prisma.$queryRaw`SELECT * FROM users`;

// ✅ CORRECT — typed query with explicit field selection
const users = await prisma.user.findMany({
  where: { email: { contains: "@example.com" } },
  select: { id: true, name: true, email: true },
});
```

## Error Handling

Catch and translate database-level errors (e.g. unique constraint violations) into meaningful application errors:

```typescript
try {
  await prisma.user.create({ data: { email } });
} catch (error) {
  if (error instanceof Error && error.message.includes("Unique constraint")) {
    throw new Error("Email already in use");
  }
  throw error;
}
```

## Do Not

- Do **not** expose raw ORM/database model objects directly as API response types — map to a DTO first.
- Do **not** manage database connections manually in application code — use the singleton client provided by the ORM.
- Do **not** run migrations in production manually — use the CI/CD pipeline.

