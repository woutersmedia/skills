# Testing

## Rule

Every non-trivial feature must have tests. Aim for meaningful coverage, not 100% line coverage.

## Test File Naming

- `ComponentName.test.tsx` — component tests
- `utilityName.test.ts` — utility/hook tests
- Co-locate test files with the source they test, or place them in a `__tests__/` sibling directory.

## What to Test

| Layer      | Focus                                                  |
| ---------- | ------------------------------------------------------ |
| Components | Renders correctly, responds to user interactions       |
| Containers | Data transformations, conditional rendering, API calls |
| Utilities  | Pure functions: happy path + edge cases                |
| API routes | Request/response shape, error handling                 |

## React Component Tests

Use **React Testing Library** — test behaviour, not implementation details.

```typescript
import { render, screen, fireEvent } from "@testing-library/react";
import { SaveButton } from "@/components/SaveButton/SaveButton";

describe("SaveButton", () => {
  it("calls onSave when clicked", () => {
    const onSave = jest.fn();
    render(<SaveButton onSave={onSave} />);
    fireEvent.click(screen.getByRole("button", { name: /save/i }));
    expect(onSave).toHaveBeenCalledTimes(1);
  });

  it("is disabled while loading", () => {
    render(<SaveButton onSave={jest.fn()} isLoading />);
    expect(screen.getByRole("button")).toBeDisabled();
  });
});
```

## API Route Tests

```typescript
import { GET } from "@/app/api/users/route";
import { NextRequest } from "next/server";

it("returns users list", async () => {
  const req = new NextRequest("http://localhost/api/users");
  const res = await GET(req);
  expect(res.status).toBe(200);
  const body = await res.json();
  expect(Array.isArray(body)).toBe(true);
});
```

## Mocking

- Mock external dependencies (Prisma, fetch, third-party SDKs) — never call live services in tests.
- Use `jest.mock(...)` or `msw` for fetch mocking.

```typescript
jest.mock("@/lib/prisma", () => ({
  user: { findMany: jest.fn().mockResolvedValue([{ id: "1", name: "Alice" }]) },
}));
```

## Do Not

- Do **not** test implementation internals (internal state, private helpers).
- Do **not** snapshot-test large component trees — they become brittle and meaningless.
- Do **not** remove or skip existing tests without a documented reason.
