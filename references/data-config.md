# Data and Configuration

Configuration comments should be sparse and operationally useful. Explain why a setting exists, why a value differs from the default, units, precedence, dangerous toggles, environment behavior, and unusual ordering. Do not comment every key/value pair.

## YAML

Extensions: `.yaml`, `.yml`.

Use `#` comments. Document non-obvious semantics, anchors/aliases, templating constraints, CI conditions, permissions, cache/artifact flow, deployment gates, and environment-specific behavior.

For Kubernetes manifests, prioritize probes, resource choices, security settings, rollout behavior, disruption constraints, storage/network assumptions, and unusual annotations. Verify whether Helm, Jinja, or another preprocessor changes comment behavior before editing.

Do not modify lockfiles merely because they use YAML syntax.

## TOML

Extension: `.toml`.

Use `#` comments. Explain feature flags, dependency choices, tool constraints, unusual sections, values whose meaning is not apparent from the key, and environment assumptions. Prefer native description fields when the target tool offers them.

## INI, Properties, and Dotenv Examples

Common files include `.ini`, `.properties`, `.env.example`, and tool-specific configuration.

Verify the parser's accepted syntax—commonly `#` or `;`, but not necessarily both. For `.env.example`, document purpose, required/optional status, accepted format, units, and safe placeholders. Never put real credentials, tokens, private keys, secret URLs, or production values in examples or comments.

Avoid editing real secret-bearing `.env` files unless explicitly requested and safe.

## Standard JSON

Extension: `.json`.

Standard JSON does not support comments. Never add `//` or `/* ... */`, and never silently convert a file to JSONC. Do not modify lockfiles or generated manifests for documentation.

When documentation is needed and within scope, prefer:

- JSON Schema `description`, `title`, examples, or other supported metadata;
- an adjacent README or format guide;
- a source schema or generator definition;
- descriptive keys only when changing them is behaviorally safe and explicitly in scope.

## JSONC and JSON5

Extensions: `.jsonc`, `.json5`, plus tool-specific formats proven to allow comments.

Use comments only after verifying parser support. Follow the sparse configuration rules above. A `.json` filename is not JSONC merely because one editor tolerates comments.

## GraphQL

Extensions: `.graphql`, `.gql`.

For schemas, prefer GraphQL descriptions (`"""..."""` or supported string descriptions) for public types, fields, arguments, enums, and directives so semantics appear in introspection. Use `#` for developer-only maintenance rationale.

Descriptions explain client-facing API behavior; comments explain implementation or maintenance context clients do not need. Do not use `#` as a substitute for public descriptions when schema tooling supports them.

For operations, explain non-obvious fragments, cache/client constraints, pagination assumptions, or intentionally unusual field selection without narrating the query.

## Protocol Buffers

Extension: `.proto`.

Use `//` comments for messages, fields, enums, services, and RPCs. Explain wire/API semantics, units, presence behavior, compatibility requirements, reserved values, ownership, and deprecation intent.

Never renumber fields, reuse tags, or otherwise alter wire compatibility as part of documentation work.

## Generated Schemas and Manifests

Skip generated schemas and manifests. Document the source definition, generator, or customization hook. Preserve generated markers and do not hand-edit output unless the user explicitly authorizes it.

## Review

Run the actual parser, schema validator, or tool-specific check. Confirm comments are legal at their exact location, templating is unaffected, standard JSON remains comment-free, and no secrets or environment-sensitive operational details were exposed.
