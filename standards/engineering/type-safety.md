# Type Safety

We aim to keep TypeScript code type-safe and avoid bypassing the type system.

## Schema Validation

Use **Zod schema validation** instead of relying on TypeScript type casts (`as`) or manual type guards when validating data at application boundaries.

Zod should be used to validate and safely parse incoming or untrusted data before using it in the application.

For CouchDB, where different document types can exist in the same database, use Zod's `discriminatedUnion` when appropriate. This provides a clean way to validate different document types while keeping the code type-safe.

The goal is to validate the actual data rather than simply telling TypeScript to trust it.

## Avoid `any`

Avoid using `any` in TypeScript code.

Prefer proper types, type inference, generics, or schema validation such as Zod.

When a value's type is genuinely unknown, prefer `unknown` over `any` and narrow it safely before use.

If `any` is genuinely necessary, use it only with a clear technical reason.

The goal is to keep TypeScript useful by catching type-related issues during development rather than at runtime.
