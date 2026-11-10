# AI implementation and human review balance

Apply these criteria when an AI agent writes or changes code that a human will maintain or review. They supplement the main skill's scope, contracts, design, formatting, and verification rules. Do not turn preferences into mandatory rewrites or apply them to unrelated code.

## Optimize the complete change

Optimize for **correct behavior, understandable implementation, efficient verification, and inexpensive human review** together. Faster generation, fewer tokens, shorter files, or fewer tool calls are not independent quality goals. Never trade away a meaningful invariant or test solely to reduce context or diff size.

Before editing, identify the requested behavior, affected entry point, existing owner, callers, side effects, and verification route. Use targeted symbol navigation and repository conventions rather than inventing a new architecture. Keep the relevant implementation close enough that a reviewer can follow a representative execution path without excessive jumping between files. Do not duplicate important policy merely to achieve locality.

## Prefer explicit, economical contracts

- Use the project's supported native types, return types, enums, and static-analysis annotations where they clarify material inputs, outputs, shapes, nullability, units, or failures. Respect existing public APIs and supported language versions.
- Make external effects, transaction boundaries, authorization, validation, and resource lifetimes discoverable at their owning boundary. Avoid adding wrappers or comments for behavior already clear from established framework conventions.
- Prefer descriptive domain operations and named values over opaque flags or dynamic dispatch when ambiguity affects correctness. Do not create a DTO for every argument or an interface for every concrete class.
- Preserve intentional framework idioms. AI unfamiliarity with a convention is a tooling/discovery problem, not sufficient grounds to replace the convention.

## Choose structures by navigation and coupling cost

- Start with a direct function or cohesive concrete class. Extract a collaborator when it owns a real invariant, has an independent reason to change, or materially improves testability or comprehension.
- Prefer composition when independently varying behavior or dependencies make it useful. An interface needs a meaningful consumer boundary, not merely a hypothetical second implementation.
- Use a trait for small, cohesive reusable behavior whose dependencies are explicit. Avoid traits relying on undocumented consumer properties/methods, conflicting aliases, or scattered state. Declare required methods where the language supports it.
- Use an abstract base class when a genuine subtype contract or stable shared lifecycle justifies inheritance. Avoid deep inheritance, fragile hooks, and overriding behavior that obscures the executed path.
- Favor locality without forcing everything into one file. Extract details that obscure the main operation, but avoid trivial one-line forwarding methods and chains of layers with no distinct responsibility.
- Avoid speculative patterns, broad generic helpers, and mechanical class splitting. Evaluate the concrete cost to a future change and a present review.

## Write code that can be checked, not just explained

- Make business invariants observable in behavior-focused tests. Include meaningful success, boundary, and failure cases proportionate to risk; do not write tests that merely restate implementation.
- Use existing formatter, static analysis, and test tooling. Prefer machine-enforced rules over verbose comments or additional agent-only documentation.
- Explain non-obvious *why*, including ordering, compatibility, performance, and safety constraints. Do not narrate obvious syntax or duplicate types in prose.
- When choosing between equally correct implementations, favor the one with fewer hidden assumptions and fewer conceptual steps for a reviewer, not automatically the fewest lines.

## Review the generated change as a human would

Before handoff, inspect the final diff and ask:

1. Can a reviewer identify the behavior change and its owner quickly?
2. Can they follow a representative success path and relevant failure path without reconstructing hidden state?
3. Are the important contracts, side effects, and ordering visible or discoverable through established conventions?
4. Does each added abstraction remove more coupling or ambiguity than navigation and maintenance cost it adds?
5. Do tests and checks provide independent evidence for the intended behavior?
6. Are unrelated edits, redundant comments, mechanical churn, and speculative flexibility absent?

Fix in-scope issues found by this pass. Do not perform unrelated cleanup. Distinguish verified outcomes from assumptions and unrun checks in the handoff.

## PHP and Laravel application

Use PHP native types and PHPStan/Psalm array shapes or generics when they express a material contract unavailable in native types. Prefer explicit enum cases for closed domain states where supported. Treat Eloquent relationships, scopes, casts, container wiring, observers, and events as legitimate conventions; trace their effects when consequential instead of replacing them by default. For traits, document or declare required host methods rather than assuming invisible members. Prefer focused actions or services only when the operation has meaningful business orchestration; simple controller/model code may remain simpler.

## Evaluation, not assumed superiority

Claims that a style is more AI-friendly are hypotheses unless measured. For representative tasks, compare behavioral correctness, regressions, human review time, context/tool use, and change scattering. Prioritize correctness and reviewability over token savings. Do not impose arbitrary function-length, class-count, or file-count limits as proxies for quality.
