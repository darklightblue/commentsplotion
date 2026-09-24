---
name: commentsplotion
description: Use when documenting code with useful native comments.
version: 0.1.0
author: light, Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [documentation, comments, mixed-language, code-quality]
    related_skills: []
---

# Commentsplotion

Apply a documentation pass to code and configuration without changing behavior. Commentsplotion means an explosion of useful context—not maximum comment count. Write for a capable teammate in their first week: explain what implementation alone cannot make obvious.

## When to Use

Use this skill:

- whenever creating, editing, refactoring, or meaningfully touching code;
- for focused documentation reviews;
- across mixed-language folders or repositories;
- when the user invokes `/commentsplotion`.

For ordinary coding work, update documentation near changed logic rather than turning the task into an unrelated repository-wide rewrite. Do not use this skill to add decorative comments, normalize unrelated prose, or alter behavior merely to make documentation easier.

## Core Standard

Document verified information that helps future maintainers recover intent:

- purpose, business rules, and branch rationale;
- input semantics, units, constraints, and precedence;
- result meaning and important edge behavior;
- preconditions, postconditions, and invariants;
- transformations, state transitions, and data ownership;
- database, network, filesystem, cache, event, process, and UI side effects;
- failure paths, partial success, retries, rollback, and recovery;
- ordering, concurrency, synchronization, and lifecycle assumptions;
- security-sensitive decisions, compatibility workarounds, and operational constraints.

Truth beats templates. Never speculate about guarantees the code does not enforce. Do not claim validation, atomicity, idempotency, thread safety, retries, caching, persistence, security, or rollback without evidence from implementation, tests, or authoritative project documentation. Resolve contradictions instead of documenting both as fact.

Types and readable code already communicate structure. Do not restate names, signatures, obvious assignments, or straightforward control flow. Prefer one high-value explanation per logical step over micro-comments on each line.

## Complexity and Risk

Choose depth per symbol or file, not per extension.

- **Low complexity:** Simple mappings, formatting helpers, obvious rendering, and basic values need a short public description only when it adds meaning. Usually omit inline commentary.
- **Medium complexity:** State transformations, reusable utilities, guarded branches, hooks, queries with business rules, and multi-phase scripts need native API documentation plus key branch, side-effect, and result semantics. Add an example when usage is not obvious.
- **High complexity:** Authentication, authorization, billing, entitlements, webhooks, retries, reconciliation, concurrency, migrations, destructive operations, cryptography, and cross-service workflows need explicit contracts, invariants, ordering, failure behavior, side effects, and recovery notes. Use phase comments where sequence matters.

Risk can raise the documentation tier even when the code is short.

## Progressive Disclosure

Inspect target files before loading references. Load only the files needed for the extensions and embedded language regions in scope:

| Files | Load |
|---|---|
| `.js`, `.jsx`, `.mjs`, `.cjs`, `.ts`, `.tsx`, `.mts`, `.cts` | `references/javascript-typescript.md` |
| `.py`, `.pyi` | `references/python.md` |
| `.go`, `.rs`, `.java`, `.kt`, `.kts`, `.cs`, C/C++ headers and sources, `.swift`, `.dart`, `.scala`, `.sc` | `references/compiled-languages.md` |
| `.php`, `.rb`, `.rake`, `.ex`, `.exs`, PowerShell, shell, Lua, R | `references/scripting-languages.md` |
| HTML, XML, CSS-family files, Markdown/MDX, Vue, Svelte, Astro | `references/markup-styling.md` |
| YAML, TOML, INI/properties, dotenv examples, JSON-family files, GraphQL, Protocol Buffers | `references/data-config.md` |
| `.sql` and database migration/query files | `references/database-query.md` |
| Terraform/HCL, Dockerfiles, Makefiles, and build/infrastructure files | `references/infrastructure-build.md` |

For mixed-language files, load each relevant reference and apply conventions by region. Do not load unrelated references. If a format is not listed, verify its parser, native documentation mechanism, and repository convention before editing; never guess comment syntax.

## Procedure

1. **Establish scope.** Identify files created or meaningfully changed. For a repository-wide request, inventory the tree and group candidates by language and subproject.
2. **Exclude unsafe targets.** Skip generated, vendored, minified, bundled, compiled, binary, lock, build-output, generated-schema, and ordinary snapshot files. Document the generator or source schema instead when useful.
3. **Detect conventions.** Inspect neighboring files, project instructions, doc tooling, and parser behavior. Repository conventions override this skill unless the user asks to standardize them.
4. **Load references.** Use `skill_view(name='commentsplotion', file_path='references/<file>.md')` only for the file families in scope.
5. **Review existing context.** Preserve correct rationale and human knowledge. Merge duplicate explanations, remove comments that merely narrate code, and update stale claims only when the correction is verifiable.
6. **Classify depth.** Apply the complexity and risk tier to each meaningful symbol or flow.
7. **Document contracts and rationale.** Prefer native API documentation for public contracts and ordinary comments for internal reasoning. Put comments immediately beside what they govern.
8. **Verify behavior is unchanged.** Review the diff, then run the repository's relevant formatter, parser, linter, type checker, tests, or build.
9. **Audit quality.** Account for every modified file and remove noise, duplication, unsupported claims, invalid syntax, and accidental secret exposure.

A repository-wide pass is complete only when every in-scope file is documented, intentionally skipped, or excluded with a reason.

## Special Policies

### Formats without comments

Never insert comments into standard JSON or another format whose parser forbids them. Do not silently convert JSON to JSONC. When documentation is needed and within scope, use an adjacent README, a schema, JSON Schema `description`, or another native metadata mechanism.

### Tests

Do not blanket-document ordinary test cases. Names, fixtures, `describe`/`context`/`it` blocks, and table cases should carry most intent. Comment strange fixtures, timing or ordering dependencies, intentionally malformed inputs, non-obvious regressions, compatibility workarounds, and complex reused helpers.

### Migrations

Do not rewrite or reorder migrations merely for documentation. For checksum-sensitive or already-applied migrations, avoid edits unless the project explicitly permits them. When safe, document destructive-operation rationale, backfill assumptions, locking/performance impact, repaired invariants, and rollback limits.

### Re-exports and trivial modules

Skip pure barrels and re-export-only modules unless they define a meaningful public boundary. Add file/module documentation only for real subsystem roles, shared invariants, protocols, initialization ordering, security models, or external contracts—not generic headers such as “helpers live here.”

### Functional and historical comments

Preserve compiler, linter, formatter, coverage, type-suppression, code-generator, and runtime directives; generated markers; deprecation metadata; legal headers; issue/RFC references; and safety notes. Their placement may be functional. Preserve historical rationale that cannot be reconstructed from code unless it is clearly obsolete.

### Markers and external references

Use `TODO`, `FIXME`, `HACK`, `NOTE`, and warnings only when actionable or materially informative. Explain why a workaround exists and what would remove it. Preserve authoritative links and add new links only when verified and useful; state the requirement they support so context survives link rot.

### Security

Never copy credentials, tokens, private keys, secret URLs, personal data, or production values into comments or examples. Explain the mechanism and invariant without exposing sensitive material.

## Style and Placement

- Use the file's native public-documentation construct where one exists.
- Use ordinary comments for implementation rationale.
- Keep public docs directly attached to their declaration and internal comments directly before the logical block they explain.
- Do not insert comments between decorators, annotations, attributes, or directives and their target when tooling could break.
- Use numbered phase comments only when order genuinely matters.
- Use canonical domain terminology consistently; do not blur distinct concepts with near-synonyms.
- Keep examples realistic, current, and copy-pasteable when possible. Skip examples whose normal usage is self-evident.
- If safe naming makes a comment unnecessary and renaming is already within scope, prefer the clearer name.

## Verification Checklist

Before finishing, verify:

- every edited file uses legal, idiomatic syntax and the relevant reference was loaded;
- native API documentation is used for public contracts where appropriate;
- standard JSON, generated files, lockfiles, binaries, vendor code, and build output were not polluted;
- comments describe current, verifiable behavior and not merely syntax;
- important constraints, edge results, side effects, failures, ordering, and recovery are covered in proportion to risk;
- examples match current APIs and no secrets or production data were added;
- existing directives, headers, metadata, and irreplaceable rationale remain intact;
- terminology matches the surrounding domain;
- the diff contains no unrelated formatting churn or runtime behavior changes;
- relevant validation passes.

Commentsplotion is semantic, not syntactic. Explain only the context future maintainers would otherwise have to rediscover.
