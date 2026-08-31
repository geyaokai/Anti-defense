# Anti-Defense Coding

## Core principle

Write the simplest correct implementation for the current requirement. Do not add guards, fallbacks, or error handling for hypothetical scenarios. Handle only cases required by the current contract or demonstrated by existing behavior.

## Scope and change discipline

- Apply these rules to all new code.
- When modifying existing code, remove nearby unnecessary defensive logic only when it is directly related to the requested change.
- Do not perform repo-wide cleanup unless explicitly requested.
- Keep diffs focused; avoid unrelated refactors, renames, formatting, or new layers.
- Prefer direct, readable control flow and YAGNI.

## Trusted internal code

- Trust documented contracts and established invariants after boundary validation.
- Do not repeat `None` checks, runtime type checks, or required-field validation inside trusted internal code.
- Access required data as required (`payload["id"]`, not `payload.get("id")`).
- Do not silently replace missing or invalid state with `None`, `false`, empty collections, or arbitrary defaults.
- Prefer fixing the producer of invalid state over guarding every consumer.

## Validate at boundaries

Validate genuinely untrusted data at user, network, API, CLI, environment, configuration, external-file, third-party-service, and untrusted-database boundaries. Convert it into a clear internal representation, then let downstream code rely on that representation.

## Errors and fallbacks

- Catch only errors that this layer can meaningfully handle.
- Avoid broad `except Exception` / `catch (Exception)`, swallowed errors, and catch-and-return-default patterns.
- Add retries, compatibility paths, and fallback chains only for a concrete supported requirement.
- Let unexpected programming errors reach the appropriate error boundary.

## Before adding complexity

Before adding a guard, fallback, retry, wrapper, abstraction, compatibility layer, optional parameter, exception handler, type check, or null check, answer:

> What concrete, currently supported scenario requires this?

If there is no concrete answer, do not add it.

## Tests and workflow

- Read relevant code and identify the smallest file set before editing.
- Test real behavior and demonstrated edge cases; keep assertions precise.
- Do not weaken tests or add hypothetical cases to justify defensive code.
- Run the narrowest relevant checks, then review the diff for accidental complexity.

