# Formatting standards

Use this reference when selecting a formatting baseline or verifying compliance. Follow explicit user instructions and the repository's established standard/configuration. These defaults apply only when the project has no declared standard; they do not authorize migrating an entire codebase or adding dependencies.

## Discover and enforce the actual standard

Inspect repository instructions, contributing documentation, EditorConfig, formatter/linter configuration, package scripts, and CI. In a monorepo, use the configuration that applies to the affected package. Use the installed or pinned tool version and the project's wrapper command when available. The repository's documented formatter/check command, pinned version, and applicable configuration are authoritative; prefer the CLI or wrapper used by CI. Inspect IDE settings only when the repository delegates formatting authority to them. A successful canonical project check does not require proof of IDE equivalence or an IDE-verification disclaimer. If the authoritative tooling is unavailable, report that limitation and distinguish manual inspection from an automated pass. A nearby legacy formatting defect does not by itself establish an intentional project convention.

## Current standards and practical formatting equivalents

Verified 14 October 2026. Language specifications define valid syntax and behavior; a formatter or style guide supplies layout rules. Do not describe formatter defaults as an international standard. When a task requests the latest standard, verify the current official edition and tool support rather than treating this dated inventory as permanently current. Keep the project's supported runtime, browser, and parser targets; a new standard is not authorization to adopt unsupported features.

| Language | Official baseline | Formatting and checks |
|---|---|---|
| PHP | [PSR-1](https://www.php-fig.org/psr/psr-1/) plus current [PHP-FIG PER Coding Style 3.1](https://www.php-fig.org/per/coding-style/), which replaces and extends [PSR-12](https://www.php-fig.org/psr/psr-12/). PSR-4 concerns autoloading, not whitespace. | Target the latest published PER compatible with supported PHP syntax. Use an existing PHP-CS-Fixer/PHP_CodeSniffer ruleset that actually supports that PER version; verify preset support. Follow an explicitly pinned older guide; discuss migration only when requested or necessary for the task. Do not present an older PSR-12-only check as full current-PER compliance. |
| JavaScript | [ECMA-262](https://ecma-international.org/publications-and-standards/standards/ecma-262/): ECMAScript 2026, 17th edition. It specifies the language, not one whitespace style. | Existing Prettier/Biome formatting and ESLint or equivalent configured checks. If no style is defined, use Prettier defaults as a practical convention. Respect actual runtime/transpiler targets and existing module semantics. |
| HTML | [WHATWG HTML Living Standard](https://html.spec.whatwg.org/multipage/). | Existing formatter and HTML conformance checks where available; default to Prettier's HTML formatting when no style is declared. Preserve meaningful whitespace, preformatted content, and template syntax. [Google's HTML/CSS guide](https://google.github.io/styleguide/htmlcssguide.html) is an optional declared style guide, not the HTML specification. |
| CSS | [W3C CSS specifications](https://www.w3.org/Style/CSS/specs.html), currently organized by CSS Snapshot 2025 and individual modules; there is no single current CSS version covering all features. | Existing Prettier/Biome and Stylelint or equivalent checks; default to Prettier when no layout standard is declared. Preserve cascade, duplicate declarations used as fallbacks, and browser targets; do not alphabetize rules/declarations indiscriminately. |
| YAML | [YAML 1.2.2](https://yaml.org/spec/1.2.2/), with the consuming tool's supported version/schema. | Existing Prettier/yamllint or equivalent configured checks. Default to two-space indentation and readable block mappings/sequences where appropriate; two spaces is a style convention, not a spec mandate. No indentation tabs. Preserve scalar types, quoting needed by consumers, anchors, and block-scalar indentation/chomping. |
| JSON | [RFC 8259](https://www.rfc-editor.org/rfc/rfc8259) and [ECMA-404](https://ecma-international.org/publications-and-standards/standards/Ecma-404/). | Strict JSON parsing plus existing formatter/schema checks. Default to two-space pretty printing for human-maintained files. JSON strings and member names use double quotes; comments and trailing commas are not strict JSON. Treat explicitly declared JSONC/JSON5 as separate formats and do not silently convert them. Do not reorder data merely for cosmetics. |
| Python | [PEP 8](https://peps.python.org/pep-0008/). | Existing Black/Ruff or other configured checks; retain their deliberate choices rather than importing another tool's defaults. |
| Go | Canonical [gofmt](https://pkg.go.dev/cmd/gofmt) output. | Existing Go formatting check; retain adopted additional import rules. |
| Rust | [rustfmt](https://github.com/rust-lang/rustfmt) with the project's toolchain, edition, and configuration. | Existing formatter command or `cargo fmt --check` when its scope fits the task. |

[Prettier configuration](https://prettier.io/docs/configuration) and [options](https://prettier.io/docs/options) define its behavior; consult the installed version. Its defaults are conventions, not substitutes for parsers, semantic linters, schema validation, or HTML conformance checks. Google's JavaScript guide is no longer updated according to its [own notice](https://google.github.io/styleguide/jsguide.html); do not select it as the latest maintained universal JavaScript standard.

For other languages, discover the official or established ecosystem formatter/style guide and prefer the repository's chosen implementation. Do not invent one set of indentation, braces, quotes, import sorting, or line-length rules for all languages.

## What to check

Verify indentation; braces and continuation alignment; operator, argument, and declaration spacing; blank-line grouping; wrapping; applicable import organization; trailing whitespace; encoding and line endings; and final newlines. Enforce each according to the selected standard. Do not mechanically reorder imports whose execution order affects behavior.

For PHP with no declared override, PER provides a concrete baseline: four-space indentation, LF endings and a final newline, no trailing whitespace, one statement per line, appropriate declaration/namespace/import separation, and `declare(strict_types=1)` without spaces inside its parentheses when present. Its blank-line guidance allows related-block grouping where not otherwise forbidden. Formatting does not authorize adding strict typing or changing program semantics. Consult the current specification for supported modern syntax rather than extrapolating obsolete rules.

Follow the configured project formatter for logical expressions. Where layout is discretionary, prefer multiline complex conditions and explicit grouping for mixed operators; short, clear expressions may remain compact. Preserve precedence, operand order, short-circuit behavior, and returned values. Project formatting takes precedence over discretionary preferences in this skill.

Respect generated-file ownership and exact-content fixtures. Use the generator or the appropriate ignore rule already in place; do not add exclusions to hide defects in owned code. A formatter changing the whole file can be valid, but its mechanical changes must remain easy to distinguish from the requested behavior change.

## Imports and qualified names

Follow language and repository conventions for imports and qualified names. For PHP, prefer imported short names for external symbols in code and type declarations, with descriptive aliases for collisions. Use class imports, `use function`, and `use const` as appropriate, including global exception classes. Prefer individual symbol imports over namespace aliases unless the project establishes another convention. Do not add redundant imports for same-namespace symbols, built-in types, or language constructs.

Do not rewrite namespace/import declarations or literal names required by an external API, serialized format, configuration, reflection contract, or fixture. Preserve function/constant resolution, dynamic lookup semantics, and import execution order where it matters. Check affected usage sites after formatting and import optimization.

## Report evidence proportionately

Name the formatter/check and its result when material to review. Distinguish automated verification from manual inspection. Report pre-existing or out-of-scope violations without presenting them as introduced defects or silently fixing unrelated files. A clean syntax check or PHPUnit run is not a formatting check. This skill directs enforcement; it does not itself install a CI gate.
