# Scripting Languages

Use the repository's native documentation convention and parser-safe comments. Scripts often carry operational risk, so document prerequisites, side effects, idempotency, privilege needs, and destructive behavior in proportion to that risk.

## Shell

Applies to `.sh`, `.bash`, `.zsh`, `.fish`, and extensionless shell scripts after verifying the interpreter.

Preserve the shebang as the first line. Use `#` comments with syntax valid for the actual shell. For non-trivial scripts, add a concise header after the shebang covering purpose, required environment/tools, important inputs, side effects, destructive behavior, and whether rerunning is safe.

Explain quoting, globbing, pipelines, subshells, traps, cleanup, error-mode choices, platform assumptions, and order-sensitive phases when non-obvious. Do not comment basic shell syntax line by line.

```sh
#!/usr/bin/env bash
# Synchronizes generated artifacts to the staging directory. Rerunning is safe,
# but removed source files are also removed from the destination.

set -euo pipefail
```

Number phase comments only for real orchestration. Preserve `shellcheck` and other functional directives.

## PowerShell

Extensions: `.ps1`, `.psm1`, `.psd1`.

Use comment-based help for reusable or public commands when appropriate: `.SYNOPSIS`, `.DESCRIPTION`, `.PARAMETER`, `.EXAMPLE`, `.OUTPUTS`, and `.NOTES`. Use `#` for implementation rationale.

For deployment and administration scripts, document prerequisites, privileges, remote effects, destructive operations, idempotency assumptions, error preferences, and cleanup. Do not copy credentials or production values into examples.

## PHP

Extensions: `.php`.

Use PHPDoc for public and important APIs when the project follows that convention. Common useful tags include `@param`, `@return`, `@throws`, `@template`, `@var`, and `@deprecated`.

Modern type declarations already carry structure. Use PHPDoc for semantics, generics/static-analysis contracts, lifecycle, side effects, constraints, and behavior rather than duplicating signatures. Use ordinary comments for internal rationale.

## Ruby

Extensions: `.rb`, `.rake`.

Use `#` comments and the repository's existing documentation convention. If YARD is already used, follow tags such as `@param`, `@return`, `@raise`, and `@example`. Do not introduce YARD solely because this skill is running.

For Rake tasks, service objects, callbacks, and metaprogramming, explain prerequisites, side effects, ordering, dynamic behavior, and rollback limitations where non-obvious.

## Elixir

Extensions: `.ex`, `.exs`.

Use native `@moduledoc`, `@doc`, and `@typedoc` for public contracts and `#` for implementation rationale. Do not add `@doc` to every private helper. Explain process ownership, supervision, message ordering, retries, state transitions, and failure semantics when relevant.

## Lua

Extensions: `.lua`.

Use `--` or parser-supported block comments and follow existing LuaDoc/LDoc/EmmyLua conventions if present. Do not introduce an annotation dialect the repository does not use. Explain table shapes, metatable behavior, mutation, callback contracts, resource ownership, and host-runtime assumptions where needed.

## R

Extensions: `.R`, `.r`, `.Rmd` code regions after verifying the format.

Use roxygen2 comments such as `#'` for exported package APIs when the project uses roxygen2; use `#` for implementation rationale. Document vectorization, recycling, missing-value handling, factor/column assumptions, side effects, random seeds, units, and statistical constraints rather than repeating function signatures.

For R Markdown, improve visible prose when appropriate and apply R conventions only inside R code chunks.

## Review

Verify the actual interpreter and version, comment syntax, help/doc generator conventions, safe rerun claims, and platform assumptions. Run the project's parser, linter, help generation, and tests where available.
