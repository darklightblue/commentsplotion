# Markup and Styling

Markup and stylesheets are already highly descriptive. Add comments only for constraints a maintainer cannot recover from the visible structure: rendering boundaries, accessibility decisions, compatibility workarounds, integration mount points, layout invariants, and deliberate specificity.

## HTML

Extensions: `.html`, `.htm`.

Use `<!-- ... -->` for structural or integration rationale. Useful targets include server-rendering or hydration boundaries, third-party mount points, unusual accessibility structure, intentionally duplicated markup, and template markers.

Do not narrate ordinary DOM structure. Preserve framework, CMS, test, and build markers whose exact placement is functional.

## XML and XML-Based Formats

Extensions include `.xml`, `.xsd`, `.csproj`, `.props`, `.targets`, and other verified XML formats.

Use `<!-- ... -->` only where legal for the actual consumer. Do not place comments inside attributes, before an XML declaration where prohibited, inside generated files, or where tooling strips or rejects them.

For schemas and project/build files, explain unusual conditions, dependency overrides, generated/manual boundaries, target ordering, compatibility constraints, and build workarounds. Prefer native description/annotation elements when the ecosystem expects them.

## CSS, SCSS, Sass, and Less

Use syntax supported by the actual language and build pipeline. Prefer `/* ... */` for portable CSS comments. `//` may be available in Sass/SCSS/Less but can be stripped or interpreted differently, so follow repository convention.

Document:

- browser and platform workarounds;
- layout invariants and containing-block assumptions;
- stacking contexts and z-index policy;
- non-obvious magic values and design-token exceptions;
- selector specificity or cascade workarounds;
- animation, rendering, and performance constraints;
- accessibility decisions such as reduced motion or focus treatment.

Do not comment obvious declarations. If a magic value can safely become a named custom property within scope, prefer the clearer name and document the reason once.

## Vue, Svelte, and Astro

Extensions: `.vue`, `.svelte`, `.astro`.

Treat each region by its language:

- script blocks: load and apply the JavaScript/TypeScript reference;
- template/markup regions: use HTML-style comments sparingly;
- style blocks: use the relevant CSS-family convention.

Document the component's public behavior in the idiomatic location for that framework and repository. Avoid repeating the same explanation in script, template, and style sections. Preserve compiler directives and framework-specific comment markers.

Focus on state ownership, props/events/slots semantics, lifecycle side effects, server/client boundaries, accessibility constraints, and surprising render behavior.

## Markdown and MDX

Extensions: `.md`, `.mdx`.

Markdown is already documentation. Improve visible prose rather than injecting hidden comments to satisfy Commentsplotion. Use HTML comments only for genuine authoring or tooling notes compatible with the renderer.

For MDX, distinguish prose from embedded JSX/JavaScript. Apply JavaScript/TypeScript conventions inside code regions only when documentation there is useful; do not clutter narrative content.

## Templates and Embedded Languages

For template engines, verify delimiter and comment rules before editing. Apply the host format's convention and separately load references for embedded languages. Never assume an HTML comment is invisible to a server-side template, email client, or preprocessor.

## Review

Render or build the affected surface when possible. Verify comments do not alter whitespace-sensitive output, hydration, selector generation, minification, templating, accessibility trees, or parser validity.
