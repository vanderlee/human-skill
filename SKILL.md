---
name: human
description: Make code changes easier for humans to understand and review. Use during implementation, refactoring, or code review when readability, focused diffs, structural clarity, and evidence-backed handoffs matter.
---

# Code humans can review

Make code review after AI changes easier on humans. Produce code and a change explanation that let a reviewer understand the purpose, follow the implementation, identify consequential differences, and assess the evidence without reconstructing the AI's work. Readable code supports that goal; keep improvements proportionate to the requested task.

For review requests, do not edit files. Lead with actionable findings, their locations, concrete consequences, and suggested remedies. Label demonstrated defects, contract violations, and applicable mandatory-rule violations **Required**; label preferences and readability improvements **Optional**. Use **FYI** only for useful context that requests no change. Prioritize by impact, distinguish pre-existing findings from introduced ones, and do not invent findings to fill a list.

## Keep communication easy to read

Apply these rules to chatbot conversations, code comments, documentation, review findings, handoffs, and other writing produced for the task. Respect explicit output requirements and established document formats.

- Use as few words as possible while preserving meaning, necessary evidence, and consequential limitations. Remove repetition, filler, and unnecessary background.
- Use short sentences and familiar words. Prefer plain-language explanations over specialist vocabulary.
- When a specialist term is needed, briefly explain it in parentheses on first use. For example: "invariant (a rule that must always hold)." Avoid repeating definitions when the reader already has them.
- Prefer bullet points for multiple facts, choices, findings, or steps. Keep each bullet focused. Use a short sentence for a single point rather than forcing it into a list.
- Lead with the answer, outcome, or action. Add only the context needed to understand it or decide what to do next.
- Keep technical names precise when needed to locate code or describe a contract. Do not shorten text by hiding important conditions, changing meaning, or replacing useful explanations with unexplained jargon.
- In code documentation, explain non-obvious intent and constraints. Do not add comments merely to restate clear code or define routine syntax.

## Ground changes in the code

Read repository instructions, relevant implementations, callers, and tests. Identify the supported contracts and local conventions before changing structure. Use current code and maintained documentation to verify old reviews or roadmap claims.

Treat architectural roadmaps as context, not authorization for later phases. Check current implementation and phase status, and respect explicit removal decisions rather than reintroducing superseded APIs or compatibility paths during cleanup.

For changes to parsers, speculative execution, lazy or cached results, effect commits, or alternate execution entry points, read [references/execution-lifecycle.md](references/execution-lifecycle.md) when those contracts are affected. Ordinary changes do not require this reference.

## Make intent and responsibilities visible

- Keep functions and classes centered on a coherent responsibility. Prefer an existing appropriate owner over a new layer.
- Name operations and values using the domain and their actual meaning. Make units, consumption rules, ownership, and meaningful distinctions visible where ambiguity can cause mistakes. Follow established naming conventions.
- Separate small groups of closely related statements with a single blank line. Keep each group focused on one local step, such as checking a precondition, preparing inputs, performing an operation, or constructing a result. Keep tightly related lines together; do not insert a blank line after every statement, split an expression, or add redundant blank lines immediately inside braces. Apply this grouping to code you edit and respect syntax and enforced repository formatting rules.
- Make dependencies, state changes, and side effects visible. Restore temporary state on exceptions and early returns. Do not hide important behavior behind magic dispatch or implicit fallback.
- Consolidate duplicated policy when the implementations have the same semantics and change together. Similar-looking code with different contracts need not share an abstraction. Avoid speculative interfaces, generic frameworks, and fixed size or complexity limits.
- Preserve the affected contracts, state lifetime, and effect timing. Derive required boundaries from the project's behavior and documentation.
- Follow language and repository conventions for imports and qualified names. In PHP, prefer imported short names for external classes, interfaces, traits, enums, functions, and constants, including global exception classes; use descriptive aliases for collisions. Preserve symbol resolution and dynamic lookup behavior. Do not add redundant imports for same-namespace symbols or built-in types, or rewrite literal identifiers required by external contracts. See [references/formatting.md](references/formatting.md) for details.
- Use types to clarify contracts where the language and project support them. Keep validation and failure handling near the boundary that owns them; preserve useful error context.
- Write names, comments, and durable explanations for a reader who did not see the conversation or intermediate AI revisions. Within the applicable file layout, make significant entry points easy to find and place detailed helpers after the operations they explain when that improves the reading route. Do not reorder declarations or initialization when order affects behavior.
- Write comments for intent, constraints, and surprising decisions. When touching relevant code, remove misleading comments and narration that merely repeats the implementation. Explain why a special case exists rather than labeling it special.

## Refactor methods touched by the task

Before editing a function or method, read enough of its body and affected callers to understand the requested change and its contracts. Investigate mixed responsibilities, nesting, repeated policy, or state when they obstruct implementing or verifying that change.

Refactor touched code only when its existing structure creates a concrete obstacle to implementing or verifying the requested change. Identify that obstacle and make the smallest coherent refactor that removes it; readability improvement alone does not justify cleanup. This task-driven refactor does not require a separate request. Preserve public contracts, evaluation order, side effects, error handling, and resource lifetime. Keep supporting caller and test changes limited to what the requested change needs. If a broader rewrite is necessary, explain the concrete dependency rather than substituting optional cleanup for the requested work. Respect explicit user constraints.

Method length, nesting, and existing complexity alone do not trigger refactoring or a cleanup review. Leave unrelated complexity unchanged; report an optional follow-up only when its concrete benefit warrants the reviewer's attention.

Within an authorized task-driven refactor, simplify branches, calculations, state, or indirection only as needed to remove the identified obstacle. Extract meaningful operations when they serve that purpose, keeping orchestration and consequential effect ordering visible. Keep tightly coupled logic together; avoid trivial forwarding helpers, long parameter lists, and abstractions introduced only to shorten a method.

Before handoff, inspect any refactored orchestration and extracted helpers to confirm that the identified obstacle is resolved and behavior is preserved using the checks below. Remaining unrelated complexity needs neither cleanup nor a defensive explanation.

For review-only tasks, report suggested refactors without editing.

## Enforce formatting standards

Formatting is required for new and changed code, not optional polishing. Identify the applicable repository instructions, documented coding standard, formatter/linter configuration, EditorConfig, CI commands, and tool versions before editing. Apply the project's established standard consistently; do not substitute personal taste or a different language's conventions. When no standard is defined, use the recognized language-specific baseline in [references/formatting.md](references/formatting.md) and state the chosen default when it materially affects the diff.

Follow the project's pinned standard, including an intentionally older one. Discuss migration only when requested or necessary for the task. With no declared PHP standard, use the PHP-FIG PER baseline in the reference, retaining PSR-1 foundations and supported syntax. Distinguish syntax specifications from formatting conventions and claim only the compliance actually checked.

After code changes, run the repository's documented formatter and relevant check mode or formatting linter for affected files or supported changed-line scope, using its pinned version and configuration. Prefer the canonical project CLI or wrapper command used by CI. Inspect IDE settings only when repository instructions delegate formatting authority to them; a successful canonical project check needs no separate proof of IDE equivalence. Fix violations introduced by your change. Preserve existing user changes and inspect the formatter's diff for unrelated rewrites. Make necessary whole-file formatting separately identifiable when it obscures behavioral edits; do not run a repository-wide formatter unless that is part of the task.

Check compound statements and dense expressions in new and changed code. Put control-flow bodies and separate executable statements on separate lines; wrap long chained calls, ternaries, and argument lists across multiple lines at meaningful boundaries according to the configured style.

Follow project formatting for logical expressions. When layout is discretionary, prefer multiline layout for complex conditions and explicit grouping for mixed operators. Short, clear expressions may remain compact. Preserve operand order, parentheses, precedence, short-circuit behavior, and returned values; do not hoist effectful operands or introduce eager evaluation. Use enclosing parentheses where required for safe multiline syntax.

```php
if (
    (
        $isReady
        && $hasPermission
    )
    || $isAdministrator
) {
    execute();
}
```

The configured project formatter takes precedence over discretionary layout preferences in this skill. Do not change shared configuration or add suppression directives to enforce a preference.

Use blank lines to group closely related statements within the applicable standard's rules. Do not use formatter-ignore directives, lint suppression, or configuration changes merely to bypass formatting requirements. Preserve semantics, including import execution order, string contents, fixtures, and generated files. Review-only tasks report violations without formatting files.

If the authoritative formatter or check is unavailable, inspect affected code manually against the selected standard and report the unavailable automated verification. An unavailable IDE is not a verification gap when the canonical project check ran successfully. Do not silently install tools, change the standard, or claim a formatting gate passed. When documented rules and enforced configuration disagree, inspect the project's actual conventions and report unresolved conflicts rather than toggling formats repeatedly.

## Enforce design choices

Use contracts and object boundaries to reduce reviewer uncertainty. Make affected inputs, results, failure behavior, state changes, and lifecycle constraints explicit through the project's types, interfaces, implementation, tests, and documentation without duplicating every contract in every form. Keep objects responsible for protecting their invariants, dependencies visible, and state ownership distinct from use.

Apply basic OOP and SOLID through their reasoning: coherent responsibilities, justified extension boundaries, substitutable implementations, consumer-focused interfaces, and separation of policy from infrastructure. Use the detailed criteria in [references/design-patterns.md](references/design-patterns.md) for affected structural work. Principle names alone do not justify an abstraction or a finding, and these preferences do not expand the task-driven refactoring scope.

Use an established design pattern when the requested change presents the problem that pattern solves and the pattern improves comprehension, invariants, or change isolation. Reuse a suitable existing pattern rather than introducing a parallel mechanism. A direct function or cohesive class remains valid when no pattern adds value; do not introduce a pattern merely to satisfy a naming or architecture checklist.

For structural edits and reviews, read [references/design-patterns.md](references/design-patterns.md) and apply its selection criteria. Treat a design concern as a mandatory violation only when concrete evidence demonstrates harm in the affected behavior or maintenance task, or an explicit applicable project or user rule prohibits it. A pattern label or this skill's preference alone is insufficient. Respect established framework integration conventions unless that evidence or prohibition applies. Correct demonstrated in-scope violations while preserving contracts; otherwise report the concrete impact and smallest coherent follow-up. Keep unsupported design preferences optional. Do not silently expand the assignment or conceal an unresolved violation by renaming it as a pattern.

Before completing a structural change, assess the affected code against these rules. Explain a new non-obvious pattern in terms of the concrete problem, responsibilities, and tradeoff. For reviews, substantiate violations with observable coupling, state, duplication, or failure behavior, rather than labels or size alone. These are agent review requirements, not an automated static-analysis gate.

For a consequential structural change, explain the affected contract, the owner of its invariants, and how callers depend on it. Justify a new abstraction by the concrete uncertainty, coupling, or repeated change it removes. Assess violations through observable consequences or explicit project requirements; keep design preferences optional. Scale this explanation to the change rather than adding a separate checklist to every handoff.

## Reduce cognitive load and check assumptions

Judge readability by what an unfamiliar maintainer must remember, infer, and navigate to assess the change, rather than by code length alone. Ask what the reviewer must keep in their head to judge each changed rule.

Apply these principles when writing the requested change. Refactor existing code only under the task-driven rules above, including when its structure creates a concrete obstacle to verifying the change. These principles do not authorize cleanup of nearby code merely because it could be clearer.

- Keep related decisions and data close, narrow temporary-state lifetimes, and use consistent domain vocabulary. Expose units, defaults, consumption rules, and lifecycle preconditions where ambiguity matters.
- Judge extraction by its net effect on comprehension using the refactoring rules above. Readers should understand meaningful operations without navigating between trivial helpers.
- Keep a decision and its consequences close: the condition, the action it permits, and relevant failure handling should be understandable together. Extract detailed mechanics when useful, while keeping consequential control flow and dependencies visible.
- Make local contracts visible at call sites. Names and types should communicate inputs, outputs, mutation, and failure behavior where practical. Explain non-obvious behavior that the interface cannot reasonably communicate; do not narrate every call.
- Reduce unnecessary state combinations. Prefer values that become valid once and remain valid over repeated reassignment or coordinated flags when this preserves the contract. Make possible states and transitions apparent without erasing legitimate lifecycle distinctions or introducing speculative types.
- Keep abstraction levels consistent. Let orchestration describe meaningful steps; separate low-level parsing, indexing, or transport mechanics when doing so clarifies the changed rule. Keep details inline when extraction would hide an important dependency.
- Make related branches predictable through consistent vocabulary and ordering. Expose meaningful differences explicitly so similar appearance does not conceal different behavior. Do not consolidate branches merely because they look alike.
- Make consequential differences visible: conditions, defaults, ordering, failure paths, and state lifetime. Avoid ambiguous Boolean arguments, double negatives, and hidden mutations where a clearer existing interface is available.
- Treat names, comments, old reviews, and proposed fixes as hypotheses. Seek a counterexample to changed assumptions and trace representative consequential success and failure paths from the caller. Check inputs, outputs, ordering, and effects against the contract.
- Treat attractive code and fluent explanations as presentation, not evidence of correctness. Update obsolete active comments and examples within scope; distinguish completed, cancelled, and pending work.
- When work spans interruptions or handoffs, retain relevant locations, unresolved assumptions, completed checks, and the next step in existing task context. Do not scatter temporary notes through the repository.

For each consequential change, check whether a reviewer can locate the changed rule, understand its inputs and effects, and find evidence for its behavior. Identify unnecessary navigation, hidden state, or implicit assumptions that obstruct that judgment; resolve task-relevant obstacles within scope.

Optional evidence is organized by decision: [references/cognition-evidence.md](references/cognition-evidence.md) covers maintenance choices, and [references/review-cognition.md](references/review-cognition.md) covers review choices. Consult only relevant findings when evaluating or explaining a recommendation. These references add no mandatory checks or cleanup requirements, and their engineering applications are not universal cognitive limits.

## Keep changes reviewable

- Give each reviewable increment one coherent purpose. Keep incidental cleanup, formatting, and mechanical moves separately identifiable from behavioral changes; use logical groups or separate commits/PRs only when the task and workflow warrant them. Keep a necessary cross-file contract change together rather than fragmenting it into invalid intermediate states.
- Preserve observable behavior during readability refactors, including errors, ordering, outputs, and state lifetime. For intended changes, explain a concrete trigger and before/after behavior; update affected callers, tests, and documentation together. Compatibility requirements come from the project and user.
- Choose the smallest coherent change that resolves the problem. Avoid unrelated formatting churn, opportunistic public API changes, and future roadmap features. Flag improvements outside scope without adding them silently.
- Preserve recognizable structure where it still works: existing names, declaration order, and layout help reviewers compare behavior. Avoid combining a behavioral change with unnecessary renaming, moves, and formatting of the same code. Keep necessary mechanical edits distinguishable so reviewers need not mentally reverse them to find the changed rule.
- Creating, splitting, merging, moving, or deleting classes/files is allowed when justified by the task. Explain where important responsibilities moved and distinguish moved or renamed code from changed logic. Check callers and references before removing an existing surface.
- Make review-sensitive differences explicit: conditions, defaults, operation order, failure/recovery behavior, state and resource lifetime, dependencies, and public contracts. Call out consequential decisions and assumptions rather than inventorying every edit.


For structural findings, propose a concrete remedy and its owner: consolidate genuinely duplicated policy, move a concern to its owning module, simplify a condition, extract a meaningful operation, make an invariant explicit, or remove a redundant wrapper. Explain what mental work or coupling the remedy removes; do not merely label code complex or prescribe a pattern. Remove derivable duplicate state only after checking whether it represents a snapshot, cache, retained failure, or other intentional lifetime distinction.

## Verify the change and its evidence

Select existing checks from repository instructions and affected behavior. Add or adjust tests when behavior, contracts, or a meaningful regression risk warrants them; do not add tests that merely mirror code layout, helper names, or the generated implementation. Derive expected outcomes from the intended contract, an independent example, or a known regression.

When writing or changing tests, use the reviewer's domain vocabulary. Make the triggering condition and expected outcome clear in names and assertions. Prefer concrete examples and expose consequential setup rather than making reviewers trace elaborate fixture builders to understand the changed rule; retain shared fixtures when they improve comprehension.

For structural refactors, verify representative success, failure, and boundary behavior and relevant state/effect timing. Select specialized execution and lifecycle checks from the conditional reference when applicable. Verify that newly used APIs, signatures, configuration keys, and dependencies exist in the project's actual versions rather than accepting plausible generated names. For a small naming or comment edit, proportionate inspection or an existing syntax check may suffice.

Connect important behavior claims to relevant tests or checks. State what was actually run, what the evidence covers, and material assumptions or untested behavior. A passing suite alone is not proof of every behavior-preservation claim. Do not imply that a check, independent review, or CI matrix ran when it did not.

Before handing over implementation work, check affected code against the refactoring and formatting rules above. Confirm whether the authoritative project formatter/check ran after the final code edits, and report unavailable verification.

Before handing over implementation work, inspect the actual final diff against the task's starting state or appropriate review base. Separate existing user changes from your changes; preserve user work. Check for unintended edits, stale comments, weakened or removed assertions, generated noise, unrelated dependencies, and behavior changes hidden among moves or formatting. Remove your accidental artifacts. If the review base or a check is unavailable, state that limitation instead of claiming complete verification.

## Hand off for human review

Lead with the concrete outcome and explain the reason for the change. For substantial work, provide a short reading route through the entry point, important logic or contract, and relevant tests, using current file/symbol links. Identify the decisions that deserve human attention and map significant structural moves. Base this explanation on the final diff and code, not the abandoned plan or intermediate tool activity.

When handing work to a reviewer, provide a self-contained brief: the requirements or accepted scope, the final change summary, the relevant contract and evidence, and explicit review boundaries. Use base/head commits when they accurately identify the work; for a dirty working tree, identify the starting snapshot and owned changes instead of pretending a commit range captures them. Provide enough context to assess the artifact without sending the full session history. Independent review is useful when warranted and authorized; it is not mandatory delegation.

Scale the handoff to complexity. A small edit may need one sentence and its validation; a substantial change can use these four compact parts, following an existing PR template where applicable:

- **Purpose:** the problem and why this change addresses it.
- **Behavior:** the before/after result, important contract differences, and structural moves where relevant.
- **Review route:** where to start, followed by the consequential logic and its tests; omit for a trivial change.
- **Evidence:** checks run and relevant coverage, plus remaining uncertainty or unresolved issues.

Do not overwhelm the reviewer with a chronological work log, an exhaustive file inventory, repeated checklist boilerplate, or unsupported confidence labels. Keep durable rationale in the appropriate code or maintained documentation and the change-specific explanation in the handoff. Producing a handoff does not authorize publishing a PR or sending messages to others.
