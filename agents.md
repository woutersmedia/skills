# Agent Instructions

- Never hide exceptions with empty `try/catch` blocks.
- Never swallow errors silently in `catch` blocks.
- If you catch an error, either:
  - handle it meaningfully, or
  - rethrow it, or
  - log it with enough context for production debugging.
