# Coding standards

Read during review (Matt Pocock `code-review` lists this file as a Standards source) and when writing code. These come from the owner's brief; add a rule only when an automated check cannot enforce it, and prefer a lint rule or CI check over a sentence here.

## Types and structure

- Strict TypeScript. No `any`; model unknowns with `unknown` plus a Zod parse at the boundary. Keep type boundaries explicit; no silent casts or fallbacks that hide an unclear invariant.
- Validate all external input (HTTP, forms, webhooks, third-party responses, environment variables) with Zod at the system edge; inside the boundary, trust the parsed type.
- Money is an integer in the smallest unit chosen in `TECH_STACK.md`; Rial and Toman conversion happens in one module, never inline.
- Modules are cohesive, with a small public interface and no circular dependencies. Name modules, interfaces, seams and adapters with Matt Pocock's `codebase-design` vocabulary.

## Readability

- Descriptive names for everything; no `data`, `temp` or `result` without context.
- Comments explain **why** (a constraint, a trade-off, a gotcha), not what the code does.
- Prefer deleting an abstraction to polishing it; add one only for a need the spec states.
- Keep changes small (about 100 lines is comfortable; split above about 1000) and separate refactors from behavior changes.

## Errors and logging

- Never swallow an error. Handle it, or rethrow with context.
- Errors that cross a boundary are structured with a stable machine-readable code and a safe human message; internals and stack traces stay in server logs.
- Never log secrets, OTP codes, tokens, full phone numbers, or payment data.

## Dependencies

- Prefer the standard library and the existing stack. Before adding a package, check: does the stack already solve it, size, maintenance, known vulnerabilities, license. One dependency per change; review the lockfile diff; install lifecycle scripts stay blocked unless reviewed and approved.
- Record any new dependency category (queue, cache, search, state library) as a decision.

## Tests

- Test behavior through public interfaces at the seams agreed in `TEST_STRATEGY.md`; one red-to-green slice at a time. Expected values come from independent literals, not from recomputing the implementation.
- Names read like specifications. Mock only at system boundaries (payment, SMS, time, randomness); prefer real implementations, then fakes, then stubs.
- A skipped or deleted test to get green is a defect, not a fix.

## Secrets and data

- Secrets stay out of the repository, logs and chat. `.env.example` holds placeholders; real values live in ignored `.env.local`.
- Database changes ship as reviewed migrations; destructive operations need the owner's approval.
