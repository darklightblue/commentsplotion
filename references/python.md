# Python

Use Python docstrings for public and important APIs and `#` comments for implementation rationale. Follow the repository's established Google, NumPy, Sphinx/reStructuredText, or other docstring style. If none is evident, prefer readable Google-style docstrings.

## Modules and Packages

Add a module docstring when a file defines a meaningful subsystem, protocol, algorithm, command, integration, or package boundary. Explain shared invariants or lifecycle assumptions that individual symbols cannot carry. Skip generic module prose for trivial leaf modules.

For `__init__.py` files containing only imports and for pure re-export modules, usually add nothing.

## Functions and Methods

Document:

- purpose and verified behavior;
- semantic meaning and constraints of arguments;
- return meaning and edge behavior;
- raised exceptions only when verified;
- side effects, retries, cleanup, and ordering when important;
- examples for reusable, complex, or easy-to-misuse APIs.

Do not copy type hints into prose. An `Args` section should explain why an input matters, its units, sentinel values, relationships, or allowed lifecycle—not merely say “the user ID.”

## Classes

Explain responsibility, lifecycle, ownership, and important invariants. Document constructor inputs whose meaning is not obvious. Public methods should describe state changes and failure behavior; routine private helpers need documentation only when their contract or rationale is independently non-obvious.

## Properties, Dataclasses, and Protocols

Document domain semantics, mutability, ownership, validation boundaries, and non-obvious defaults. Do not repeat field names and annotations mechanically. For protocols and abstract base classes, focus on implementer obligations and invariants.

## Async, Context Managers, and Generators

For `async` functions, document cancellation, retries, timeout behavior, external effects, and verified exception paths. For context managers, explain acquired resources, cleanup guarantees, and failure behavior. For generators and iterators, explain yielding order, laziness, termination, and side effects when non-obvious.

## Stub Files

For `.pyi`, keep documentation concise and API-focused. There is no implementation to narrate. Preserve docstrings or comments that type checkers and generated documentation consume.

## Inline Comments

Use `#` comments for rationale that does not belong in public docs: fallback priority, branch policy, state transitions, transformation intent, compatibility workarounds, and phase ordering. Avoid comments such as `# Check if user is None`.

Number phases only for genuinely ordered flows:

```py
# --- Step 1: Read the current durable state ---
# --- Step 2: Validate the transition before mutation ---
# --- Step 3: Persist before notifying subscribers ---
```

## Example

```py
def resolve_display_name(settings: DisplaySettings) -> str:
    """Resolve the highest-priority display name.

    An explicit user choice takes precedence over the organization default and
    the product fallback.

    Args:
        settings: Candidate names from each configuration scope.

    Returns:
        The first non-empty display name in priority order.
    """
    # Broader defaults must not replace a value the user selected explicitly.
    if settings.user_name:
        return settings.user_name

    return settings.organization_name or DEFAULT_DISPLAY_NAME
```

## Review

Run the project's formatter, linter, docstring checks, type checker, and tests as applicable. Verify docstrings match current signatures and do not claim exceptions or guarantees absent from the implementation.
