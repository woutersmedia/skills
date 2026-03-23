# Database Schema (Prisma)

## Rule

**Use Prisma for all database access. After any schema change, regenerate the Prisma client.**

## After Editing `schema.prisma`

Always run the generate command after modifying the schema:

```bash
# From the project root
npx prisma generate

# Or from the db package directory
cd packages/db && npm run generate
```

Never commit code that imports from `@prisma/client` without first ensuring the client is up to date.

## Schema Conventions

```prisma
// ❌ AVOID — unclear naming, missing @@map
model user_data {
  i    Int    @id
  n    String
  eml  String
}

// ✅ CORRECT — PascalCase model, camelCase fields, explicit @@map for table names
model User {
  id        String   @id @default(cuid())
  name      String
  email     String   @unique
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt

  @@map("users")
}
```

- Model names: **PascalCase** (`User`, `Booking`, `ContentItem`)
- Field names: **camelCase** (`createdAt`, `userId`)
- Always add `createdAt` and `updatedAt` timestamps
- Use `@default(cuid())` or `@default(uuid())` for IDs
- Add `@@map("table_name")` to control the actual SQL table name

## Querying

Use Prisma's typed query API. Never use raw SQL unless there is no alternative, and document why.

```typescript
// ❌ AVOID — raw query bypasses type safety
const users = await prisma.$queryRaw`SELECT * FROM users`;

// ✅ CORRECT — typed Prisma query
const users = await prisma.user.findMany({
  where: { email: { contains: "@example.com" } },
  select: { id: true, name: true, email: true },
});
```

## Error Handling

```typescript
import { Prisma } from "@prisma/client";

try {
  await prisma.user.create({ data: { email } });
} catch (error) {
  if (error instanceof Prisma.PrismaClientKnownRequestError) {
    if (error.code === "P2002") {
      throw new Error("Email already in use");
    }
  }
  throw error;
}
```

## Do Not

- Do **not** expose Prisma models directly as API response types — map to a safe DTO first.
- Do **not** call `prisma.$connect()` / `prisma.$disconnect()` manually in application code; use the singleton client.
- Do **not** run migrations in production manually — use the CI/CD pipeline.
