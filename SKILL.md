---
name: human
description: Make code changes easier for humans to understand and review. Use during implementation, refactoring, or code review when readability, focused diffs, structural clarity, and evidence-backed handoffs matter.
---

# Code humans can review

Produce code and explanations that let a reviewer understand the purpose, follow consequential behavior, and assess evidence without reconstructing the conversation. Keep improvements proportionate to the requested task.

## Scope and review mode

Read repository instructions, relevant implementations, affected callers, and tests before changing code. Establish current contracts, supported versions, and local conventions. Verify old findings and roadmap claims against current code and maintained documentation. Roadmaps do not authorize future phases; respect explicit removals and compatibility decisions.

Choose the smallest coherent change that resolves the task. Avoid unrelated cleanup, public API changes, formatting churn, and speculative features. Preserve existing user changes. Report an out-of-scope improvement only when its concrete benefit warrants attention.

For review requests, report findings without editing files:

- Lead with actionable findings, locations, concrete consequences, and suggested remedies. Prioritize by impact and distinguish pre-existing issues from introduced ones. Do not invent findings to fill a list.
- Label demonstrated defects, contract violations, and applicable mandatory-rule violations **Required**; label preferences and readability improvements **Optional**. Use **FYI** for useful context requesting no change.
- Discuss behavior and consequences without attributing competence or motives. When a missing requirement determines whether something is defective, identify the uncertainty and seek the relevant contract before requiring a fix. Keep demonstrated defects direct.

## Communicate for the reader

Apply this guidance to conversation, comments, documentation, review findings, and handoffs. Respect explicit output requirements and established formats.

- Lead with the answer, outcome, or action. Use short sentences, familiar words, and only the context needed to understand or decide. Remove repetition and filler without losing conditions, evidence, or material limitations.
- Match explanations to demonstrated knowledge. Explain unfamiliar domain rules, local conventions, and specialist terms; omit basics already established. Keep technical names precise enough to locate code and describe contracts. When knowledge is uncertain, lead with the decision and add necessary supporting detail.
- Use focused bullets for parallel facts, choices, findings, or steps; use a sentence for a single point.
- Explain non-obvious intent, constraints, and surprising decisions rather than narrating syntax. Update misleading comments and obsolete active examples in affected code. When review uncovers understanding future maintainers need, preserve it in an appropriate comment, test, or maintained document within scope rather than only in conversation. Do not duplicate clear code.

## Make intent and contracts visible

Judge readability by what a maintainer must remember, infer, and navigate to assess the changed rule, rather than by length or visual appeal. Apply these principles to requested work; they do not authorize nearby cleanup.

- Keep functions and classes centered on coherent responsibilities. Prefer an existing appropriate owner over a new layer. Keep validation and failure handling at the boundary that owns them, preserving useful error context.
- Use consistent domain names. Expose units, defaults, ownership, compatibility constraints, consumption rules, and lifecycle preconditions where ambiguity matters. Use existing types and interfaces to clarify inputs, outputs, mutation, and failure behavior without wrapping every scalar or documenting every call.
- Keep related decisions, data, and consequences close. Narrow temporary-state lifetimes and make dependencies, mutations, effect ordering, and cleanup on exceptions or early returns visible. Avoid magic dispatch or implicit fallback that hides consequential behavior.
- Keep abstraction levels coherent. Let orchestration show meaningful steps; extract detailed mechanics when that reduces reconstruction work. Keep tightly coupled logic and important dependencies together; avoid trivial forwarding helpers, long parameter lists, and extraction merely to shorten methods.
- Consolidate policy only when semantics match and implementations must change together. Similar-looking branches with different contracts may remain separate. Use predictable vocabulary and ordering while exposing meaningful differences.
- Reduce unnecessary state combinations when contracts permit. Before removing derivable state, establish whether it is an intentional snapshot, cache, retained failure, or other lifetime distinction. Preserve legitimate states and transitions; do not introduce speculative types.
- Make conditions, defaults, failure paths, and state lifetime explicit. Prefer a clearer existing interface over ambiguous Boolean arguments, double negatives, or hidden mutation. Make consequential trust boundaries traceable, including where untrusted input enters privileged operations, persistence, external services, or executable contexts; keep relevant validation, authorization, and encoding decisions identifiable.
- When a demonstrated performance requirement justifies less straightforward code, make the constraint and reason for the trade-off discoverable; do not add complexity for speculative gains.
- Write durable explanations for readers who did not see the conversation. Make significant entry points easy to find and place detailed helpers after the operations they explain when useful and compatible with file conventions. Preserve declaration and initialization order when behavior depends on it.

Group closely related statements with a single blank line within the project's formatting rules. Keep each group focused on one local step. Do not separate every statement, split expressions, or add redundant blank lines immediately inside braces.

For affected parser phases, speculative execution, lazy or cached results, effect commits, or alternate execution entry points, read [references/execution-lifecycle.md](references/execution-lifecycle.md). Apply only the relevant contracts and checks.

## Refactor only to serve the task

Before editing a function, read its body and affected callers sufficiently to understand its contracts. Refactor existing structure only when it creates a concrete obstacle to implementing or verifying the requested change, or when refactoring is itself requested. Identify the obstacle and make the smallest coherent refactor that removes it. Task-driven refactoring needs no separate request; respect explicit user constraints.

Length, nesting, or complexity alone does not trigger cleanup. Keep supporting caller and test changes limited to the task. If a broader rewrite is necessary, explain the concrete dependency. Leave unrelated complexity unchanged without a defensive explanation.

Preserve public contracts, evaluation and effect order, errors, outputs, cleanup, and resource/state lifetime during readability refactors. For intended behavior changes, update affected callers, tests, and documentation together. Before handoff, confirm that refactored orchestration and helpers resolve the identified obstacle and preserve the required behavior.

## Apply design criteria

For structural edits and reviews, read [references/design-patterns.md](references/design-patterns.md) and assess the affected code against its contract, OOP, SOLID, and pattern-selection criteria. Keep objects responsible for their invariants, dependencies visible, and state ownership distinct from use. Express contracts through appropriate types, implementation, tests, or documentation without duplicating every contract in every form.

Choose an established pattern only when it solves the present problem and improves comprehension, invariants, or change isolation. Reuse a suitable existing mechanism; a direct function or cohesive class remains valid. Justify new abstractions by actual uncertainty, coupling, or repeated change they remove, rather than principle names or hypothetical flexibility.

Require concrete harm in affected behavior or maintenance, or an explicit applicable project/user prohibition, before treating a design concern as mandatory. Respect established framework conventions unless that threshold is met. Correct demonstrated in-scope violations; report broader unresolved issues with concrete impact. Preferences do not authorize architectural cleanup, and renaming a concern as a pattern does not resolve it.

For structural findings, propose a specific remedy and its owner, such as consolidating policy, moving a concern, simplifying a condition, or removing a wrapper. Explain the mental work or coupling it removes. For consequential structural changes, explain the affected contract, its invariant owner, callers' dependencies, and any non-obvious pattern tradeoff. Scale this to the change; it is an agent assessment, not an automated gate or mandatory handoff checklist.

## Enforce formatting

Formatting is required for new and changed code. Identify applicable repository instructions, coding standards, EditorConfig, formatter/linter configuration, CI commands, and pinned versions before editing. Follow that standard, including an intentionally older one. Discuss migration only when requested or necessary.

Read [references/formatting.md](references/formatting.md) when selecting a baseline, changing imports, or resolving formatting/compliance questions. With no declared standard, use its language-specific default and state the choice when material to the diff. For PHP, use its PHP-FIG PER baseline with PSR-1 foundations and supported syntax. Prefer imported short names for external PHP symbols, including global exceptions, with descriptive aliases for collisions; preserve symbol resolution and literal identifiers required by external contracts.

- Follow project layout for control flow and expressions. Put control-flow bodies and separate executable statements on separate lines; wrap dense chains, ternaries, and argument lists at meaningful boundaries. Where discretionary, use multiline complex conditions and explicit grouping for mixed operators; compact clear expressions may remain.
- Preserve precedence, operand order, short-circuit behavior, return values, import execution order, strings, fixtures, and generated-file ownership. Do not hoist effectful operands or make evaluation eager for presentation.
- After final edits, run the canonical project formatter and relevant check mode or formatting linter for affected files or supported changed-line scope, with pinned configuration/version. Fix introduced violations and inspect its diff for unrelated rewrites. Keep necessary whole-file formatting distinguishable; repository-wide formatting requires task scope.
- The project formatter overrides discretionary preferences here. Do not change configuration, add suppressions, or use formatter-ignore directives merely to enforce or bypass a preference. Inspect IDE settings only when the repository delegates authority to them; a canonical check needs no separate IDE proof.
- If authoritative tooling is unavailable, inspect manually and report unavailable automated verification. Do not silently install tools or claim a gate passed. Resolve documented/configured disagreements from actual conventions and report unresolved conflicts rather than toggling formats. Syntax and behavioral tests do not establish formatting compliance.

## Verify behavior and reviewability

Treat names, comments, proposed fixes, and prior reviews as hypotheses. Seek counterexamples to changed assumptions and trace consequential success, failure, and boundary paths from callers. Check inputs, results, ordering, and effects against current contracts. Verify new APIs, signatures, configuration keys, and dependencies against actual project versions.

Select checks from repository instructions and affected behavior. Add or adjust tests when behavior, contracts, or meaningful regression risk warrants them; avoid tests that merely mirror implementation, helper names, or layout. Derive expectations from the intended contract, independent examples, or known regressions. Use domain vocabulary and make triggering conditions, consequential setup, and expected outcomes clear; retain shared fixtures when they help comprehension. Small naming/comment changes may need only inspection or an existing syntax check.

Make important claims inexpensive to verify: connect the changed rule to implementation and tests/checks, using concrete inputs and expected outcomes when useful. For consequential changes, make the reasoning from contract and assumptions through implementation to evidence followable in the code and focused verification; do not rely on a handoff explanation to compensate for obscure behavior. State what ran, what was observed, what coverage is inferred, and material assumptions or untested behavior. Attractive code, fluent explanations, and a passing suite do not prove every claim. Do not imply unperformed checks, CI, or independent review.

During your own review, check whether a reviewer can locate the rule, predict a representative result, understand a relevant boundary/failure case, and find evidence. Resolve task-relevant missing causal links, unnecessary navigation, hidden state, and assumptions. Do not quiz the user or require lengthy explanations.

Inspect the final diff against the starting state or appropriate review base. Distinguish existing user work from your edits. Check for unintended changes, stale comments, weakened assertions, noise, unrelated dependencies, and behavioral differences hidden among moves or formatting. Remove accidental artifacts; report an unavailable review base or verification instead of claiming completion of those checks.

When interrupted or handing work over, retain relevant locations, unresolved assumptions, completed checks, and the next step in existing task context. Do not scatter temporary notes through the repository.

Optional research: consult relevant decisions in [references/cognition-evidence.md](references/cognition-evidence.md) for maintenance and [references/review-cognition.md](references/review-cognition.md) for review. These references add no mandatory checks or cleanup; their applications do not establish universal cognitive limits.

## Keep the diff and handoff reviewable

Give each increment one coherent purpose. Preserve recognizable names, order, and layout where they work. Keep necessary mechanical moves, renames, and formatting distinguishable from logic changes; use separate logical groups or commits/PRs only when warranted. Keep cross-file contract changes coherent rather than splitting them into invalid intermediate states. Check callers and references before removing a surface, and explain significant responsibility moves.

Base the handoff on final code and diff. A small edit may need one sentence and validation. For substantial work, follow the project's template or give a compact, self-contained brief covering:

- **Purpose:** the problem, accepted scope, and why this change addresses it.
- **Behavior:** concrete before/after results, consequential contract differences, assumptions, and structural moves.
- **Review route:** entry point, important logic/contract, and relevant tests, with current file/symbol links. Highlight decisions needing human attention and identify review boundaries.
- **Evidence:** checks and relevant coverage, including formatting results, remaining uncertainty, and unresolved issues.

Use accurate base/head commits when they capture the work; otherwise identify the starting snapshot and owned changes. Describe scope and risk through concrete behavior and dependencies, avoiding unsupported labels such as "trivial," "obvious," or "too complex."

Keep durable rationale in code or maintained documentation and change-specific explanation in the handoff. Omit chronological logs, exhaustive inventories, repeated checklist boilerplate, and unsupported confidence labels. Independent review is useful when warranted and authorized; this skill does not require delegation or authorize publishing PRs or messaging others.
