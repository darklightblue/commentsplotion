# JavaScript and TypeScript

Use this reference for JavaScript, TypeScript, JSX, and TSX files. The core skill controls what deserves documentation; this file controls native syntax and ecosystem-specific placement.

## Conventions

Use JSDoc/TSDoc-style `/** ... */` blocks for exported or otherwise important contracts. Use `//` for implementation rationale. Follow existing repository tooling when it requires a stricter dialect.

When applicable, prefer this tag order:

1. `@description`
2. `@param`
3. `@example`
4. `@implements`
5. `@returns`
6. `@throws`

Use `@deprecated`, `@template`, `@fires`, `@emits`, `@readonly`, or `@default` only when truthful and useful. Preserve existing metadata rather than normalizing it away.

## Functions and Methods

Document semantic contracts, not TypeScript syntax:

- why the function exists and where it fits;
- parameter meaning, units, constraints, sentinel values, and relationships;
- the meaning of the returned value, including empty, `null`, cached, or fallback results;
- verified exceptions and rejected-promise conditions;
- side effects, ordering, retries, cancellation, and partial completion;
- realistic examples for reusable or easy-to-misuse APIs.

Preferred parameter form:

```ts
@param input - {@link DomainType} Candidate values from each configuration scope.
```

For primitives, links are unnecessary. Do not enumerate values already clear from a linked union or enum unless one value has non-obvious semantics.

`@see` should normally reference same-file symbols unless the project's documentation tooling reliably resolves cross-file targets. Mention ordinary cross-file symbols in backticks. Never fabricate links.

## Types, Interfaces, Enums, and Constants

Place a doc block above a meaningful public type or interface. Explain the role of the overall shape and only those fields whose semantics, lifecycle, ownership, units, or constraints are not obvious.

For enums, explain domain meaning and non-obvious member behavior rather than restating names. For public constants, explain why the value exists, its units, source, mutability expectations, or compatibility implications. Do not repeat literal values in prose.

## Classes

Explain responsibility, lifecycle, ownership, and important invariants. Document constructor parameters and public methods in proportion to complexity. Avoid doc blocks on trivial private helpers unless their contract is independently important.

## React Components

Document exported components as public functions. Cover meaningful props behavior, state ownership, important effects, non-obvious render branches, accessibility constraints, and integration assumptions. Do not narrate JSX or duplicate the props type field by field.

Place implementation comments around surprising conditional rendering, effect ordering, memoization constraints, portal boundaries, or accessibility workarounds.

## Hooks

Explain the state or lifecycle behavior a hook owns, caller obligations, side effects, cleanup, and result semantics. Comment a dependency array only when the chosen dependencies are intentionally constrained or non-obvious; do not annotate every `useEffect` mechanically.

## Async and Concurrent Behavior

For async functions, document verified rejection conditions, cancellation or abort behavior, retries, timeout handling, ordering, race prevention, and external state left behind after failure. Do not claim a promise “throws” when the local convention distinguishes rejection language.

For queues, locks, deduplication, caches, and optimistic updates, state the invariant and why ordering matters.

## Inline Flow Documentation

Use comments for branch rationale, fallback priority, transformed data, side effects, and multi-step orchestration. Number phases only when order matters:

```ts
// --- Step 1: Load the persisted state ---
// --- Step 2: Validate the requested transition ---
// --- Step 3: Persist before publishing downstream events ---
```

For fallback chains, explain the policy behind the order rather than labeling obvious `if` statements.

## Example

```ts
/**
 * @description Resolves the display name using the explicit user choice before
 * broader defaults.
 *
 * @param settings - {@link DisplaySettings} Candidate names from each scope.
 *
 * @returns {string} The highest-priority non-empty display name.
 */
export function resolveDisplayName(settings: DisplaySettings): string {
  // A user override represents an explicit choice, so broader defaults cannot replace it.
  if (settings.userName) return settings.userName;

  return settings.organizationName || DEFAULT_DISPLAY_NAME;
}
```

## Review

Confirm tags reflect real behavior, examples compile against the current API, cross-links resolve under existing tooling, and comments do not duplicate static types or obvious code.
