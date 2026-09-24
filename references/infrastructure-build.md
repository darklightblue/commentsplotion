# Infrastructure and Build Files

Infrastructure and build documentation should explain operational intent and risk: why resources or stages exist, execution order, deployment dependencies, environment assumptions, security implications, destructive replacement, and compatibility workarounds.

## Terraform and HCL

Extensions: `.tf`, `.tfvars`, `.hcl`.

Use `#` or `//` according to repository style. Document:

- why a resource exists and who owns it;
- cross-resource dependencies not obvious from references;
- lifecycle and ignore rules;
- security, network, identity, and data-retention implications;
- provider bugs or version workarounds;
- unusual timeouts, replacement behavior, and rollout ordering;
- module assumptions and environment-specific constraints.

For variables and outputs, prefer native `description` attributes over external comments. For validations, explain the business or operational constraint when the expression alone is insufficient. Never expose secrets in comments, defaults, examples, outputs, or state-related instructions.

Treat `.tfvars` that contain real values as potentially sensitive. Do not open or modify secret-bearing files unless explicitly required and safe.

## Dockerfiles

Files: `Dockerfile`, `Dockerfile.*`.

Use `#` comments. Explain stage purpose in multi-stage builds, why non-obvious packages are installed, cache-sensitive ordering, platform/architecture constraints, security hardening, unusual environment variables, and why files are copied at a particular stage.

Do not narrate obvious instructions such as a routine `WORKDIR`. Preserve syntax directives such as `# syntax=...` and linter directives exactly where tooling expects them.

## Makefiles

Files: `Makefile`, `.mk`.

Use `#` comments. Document important targets, prerequisites not encoded in dependency edges, side effects, environment assumptions, phony orchestration, recursive invocation, generated outputs, and unusual shell behavior.

Preserve tab-sensitive recipe syntax. Do not move comments into recipes or continuations without verifying semantics.

## Build-System Files

For Gradle, Maven, CMake, Bazel, Meson, package scripts, and other build definitions, verify the actual language and load its host reference when applicable. Explain generated-source boundaries, plugin/task ordering, dependency overrides, reproducibility choices, platform constraints, caching, artifact flow, and compatibility workarounds.

Prefer native metadata such as task descriptions, target documentation, or variable descriptions when the ecosystem exposes it.

## CI/CD and Deployment

For pipeline files, also load the relevant data/config or scripting reference. Explain non-obvious conditions, permissions, identity assumptions, caching, artifact handoff, deployment gates, environment promotion, rollback, concurrency controls, and destructive steps.

Do not repeat what a clearly named job or step already says. Never place credentials or production values in comments.

## Operational Scripts

Load the scripting-language reference as well. Document privilege requirements, external tools, side effects, idempotency, cleanup, retry policy, and execution order. Dangerous commands deserve the reason they are safe and the scope guards that constrain them.

## Review

Run the native formatter, validation, plan/dry-run, build-graph check, or parser as appropriate. Confirm comments do not alter tab/continuation semantics, templating, image cache behavior, deployment ordering, or resource replacement. Never apply infrastructure merely to validate documentation unless the user explicitly requests it.
