# Commentsplotion

Commentsplotion is a Hermes skill for adding thorough, high-signal documentation to mixed-language codebases. It explains intent, rationale, contracts, invariants, side effects, failure behavior, and operational constraints using each file type's native documentation system.

It is not a request to maximize comment count. Straightforward code stays straightforward; documentation is concentrated where future maintainers would otherwise have to reverse-engineer decisions.

Repository: https://github.com/darklightblue/commentsplotion

## What `/commentsplotion` Does

The skill first inspects the files in scope, determines their languages and parser constraints, excludes unsafe targets, and loads only the language references it needs. It then documents public contracts and non-obvious implementation reasoning in proportion to complexity and risk without changing runtime behavior.

The same skill also serves as a standing coding convention: when installed for that purpose, documentation near meaningfully changed code is kept current during normal feature, fix, and refactor work.

## Why Progressive Disclosure

The source material covers many ecosystems. Loading every language rule for every task would waste context and make the relevant convention harder to see. The lean `SKILL.md` contains universal workflow and quality rules; `references/` contains syntax, tags, examples, and ecosystem-specific guidance.

A TypeScript-only change loads the JavaScript/TypeScript reference. A Python and SQL change loads those two references. Unrelated language profiles stay out of context.

## Supported Families

- JavaScript, TypeScript, JSX, and TSX
- Python
- Go, Rust, Java, Kotlin, C#, C, C++, Swift, Dart, and Scala
- Shell, PowerShell, PHP, Ruby, Elixir, Lua, and R
- SQL and database migrations
- YAML, TOML, INI/properties, dotenv examples, JSON-family formats, GraphQL, and Protocol Buffers
- HTML, XML, CSS/SCSS/Sass/Less, Markdown/MDX, Vue, Svelte, and Astro
- Terraform/HCL, Dockerfiles, Makefiles, CI/CD, and build-system files

Each profile uses the ecosystem's native mechanism: JSDoc/TSDoc, Python docstrings, Go doc comments, rustdoc, Javadoc/KDoc, C# XML docs, Doxygen when already established, GraphQL descriptions, Terraform metadata, or parser-safe ordinary comments.

## Mixed-Language Repositories

Commentsplotion inventories the actual files first, groups them by convention, and respects subproject-local style. A repository is not forced into one comment dialect. Mixed single-file components are handled by region: script, template, and style sections each use their native rules.

For example, a directory containing TypeScript, Python, SQL, YAML, and JSON loads four references. JSON is recognized but receives no comments because standard JSON forbids them.

## Formats Intentionally Left Uncommented

Standard JSON does not support comments. Commentsplotion never adds `//` or `/* ... */` to `.json` files and never silently converts them to JSONC. When documentation is necessary and in scope, use JSON Schema metadata, a source schema, or adjacent documentation.

The same restraint applies to any format whose parser or toolchain does not safely accept comments.

## Exclusions

The skill normally skips:

- generated code and generated schemas;
- dependency lockfiles;
- vendored dependencies;
- minified and bundled files;
- build output and compiled artifacts;
- binary files;
- ordinary snapshots;
- already-applied or checksum-sensitive migrations unless edits are explicitly permitted;
- trivial tests, re-export barrels, and obvious declarations that need no added context.

Compiler, linter, formatter, coverage, type-suppression, code-generator, and runtime directives are preserved because they are functional, not disposable prose.

## Usage

Invoke the skill explicitly:

```text
/commentsplotion Document the authentication service.
/commentsplotion Review src/payments and migrations for missing rationale.
/commentsplotion Apply a documentation pass to the files changed on this branch.
```

During ordinary coding, a persistent user preference can instruct Hermes to load Commentsplotion automatically for files it meaningfully touches.

## Repository Structure

```text
commentsplotion/
├── SKILL.md
├── README.md
├── references/
│   ├── javascript-typescript.md
│   ├── python.md
│   ├── compiled-languages.md
│   ├── scripting-languages.md
│   ├── markup-styling.md
│   ├── data-config.md
│   ├── database-query.md
│   └── infrastructure-build.md
├── .gitignore
└── LICENSE
```

## Installing into Hermes

This repository is intended to remain the canonical source. Hermes supports external skill directories, so point the active profile at this repository rather than maintaining a copied installation:

```text
hermes config set skills.external_dirs '["/absolute/path/to/commentsplotion"]'
```

If `skills.external_dirs` already contains entries, preserve them and append this repository path. Start a new Hermes session after installation so the skill index includes `/commentsplotion`.

This approach is preferable to a duplicate copy: `SKILL.md` and all references are loaded directly from the Git working tree.

## Updating

Edit files in this repository. Because Hermes reads the repository through `skills.external_dirs`, saved changes are the installed skill; there is no synchronization step. Start a new session when frontmatter or discovery metadata changes. For an already running session, reload the skill before relying on edited instructions.

Useful checks:

```text
hermes skills list
hermes skills check commentsplotion
```

The Git repository is the canonical source for the skill. A local Hermes installation can load it directly through `skills.external_dirs`, so repository edits do not require a separate copy or synchronization step.
