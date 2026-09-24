# Compiled Languages

Use the native documentation system for each language. The core skill determines documentation depth; this reference supplies syntax and ecosystem rules.

## Go

Extensions: `.go`.

Use Go doc comments for exported symbols. Start the comment with the exported symbol's exact name and write natural prose; do not introduce JSDoc tags.

```go
// ResolveDisplayName returns the highest-priority non-empty display name.
// An explicit user choice wins over broader defaults.
func ResolveDisplayName(settings DisplaySettings) string {
    // Preserve explicit user intent before applying organization policy.
    if settings.UserName != "" {
        return settings.UserName
    }
    return firstNonEmpty(settings.OrganizationName, defaultDisplayName)
}
```

Add package comments for meaningful exported libraries and commands. Explain concurrency ownership, goroutine lifecycle, channel closure, context cancellation, resource cleanup, and error semantics when relevant. Do not claim thread safety unless verified.

## Rust

Extensions: `.rs`.

Use rustdoc `///` for items and `//!` for module or crate documentation. Use normal `//` comments for implementation rationale. Add Markdown sections such as `# Examples`, `# Errors`, `# Panics`, and `# Safety` only when relevant and verified.

For `unsafe` code, document the safety invariant, caller obligations, and why the operation is sound. Explain ownership, lifetimes, synchronization, allocation, and error propagation only where they are not apparent from types and idioms.

## Java

Extensions: `.java`.

Use Javadoc `/** ... */` for public/protected APIs and important internal contracts. Use `//` or ordinary block comments for implementation rationale. Common tags include `@param`, `@return`, `@throws`, `@see`, and `@deprecated`; use `@since` only when the project maintains versioned API history.

Do not add `@return` to `void` methods. Use `{@link Type}` only when resolvable under project tooling. Describe checked and unchecked exceptions only when they are part of verified behavior.

## Kotlin

Extensions: `.kt`, `.kts`.

Use KDoc `/** ... */` for public and important declarations and `//` for internal rationale. Prefer Kotlin references such as `[TypeName]` over Java-style links. Use `@param`, `@return`, `@throws`, `@property`, `@receiver`, `@constructor`, and `@sample` when they add semantic value.

For Gradle Kotlin scripts, explain unusual plugin ordering, generated sources, compatibility constraints, and non-obvious task wiring. Do not document routine DSL syntax.

## C#

Extensions: `.cs`.

Use XML documentation comments `///` for public APIs where project conventions support them. Prefer `<summary>`, `<param name="...">`, `<returns>`, `<exception cref="...">`, `<remarks>`, `<example>`, and `<see cref="..."/>` as needed. Use `//` for implementation rationale.

Do not duplicate nullable annotations or signatures. Document exception contracts only when the visible implementation or interface supports the claim.

## C and C++

Extensions: `.c`, `.h`, `.cc`, `.cpp`, `.cxx`, `.hpp`, `.hh`.

Follow the project's existing documentation system. If Doxygen is in use, write compatible `/** ... */` blocks with tags such as `@brief`, `@param`, and `@return` where useful. Otherwise prefer clear documentation without forcing Doxygen everywhere.

Headers carry public contracts. Implementation files should focus comments on ownership, lifetime, pointer validity, allocation, synchronization, ABI or platform constraints, error handling, undefined-behavior boundaries, and performance-sensitive choices. Never infer memory ownership or thread safety.

## Swift

Extensions: `.swift`.

Use `///` for concise documentation and `/** ... */` for longer contracts. Use Swift Markdown fields such as `- Parameter name:`, `- Returns:`, `- Throws:`, `- Note:`, and `- Important:` when applicable. Use `//` for implementation rationale.

Explain actor/isolation assumptions, lifecycle, ownership, platform availability, and UI-thread requirements only when verified and not obvious from declarations.

## Dart

Extensions: `.dart`.

Use `///` for public APIs and Dart-supported Markdown references. Use `//` for implementation rationale. Avoid repeating nullability and type information. For Flutter code, focus on widget responsibility, state ownership, rebuild constraints, side effects, and accessibility or platform behavior.

## Scala

Extensions: `.scala`, `.sc`.

Use Scaladoc `/** ... */` for public APIs when that is the repository convention, and `//` for internal rationale. Explain effect behavior, laziness, implicit/type-class constraints, concurrency, and domain invariants rather than restating rich types.

## Cross-Language Review

For every language, keep public docs attached where its tooling expects them. Preserve annotations and attributes. Verify generated documentation or compiler/linter output when available, and ensure examples use current APIs.
