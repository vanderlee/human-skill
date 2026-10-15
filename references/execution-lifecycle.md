# Execution and lifecycle contracts

Read this reference only when the requested work affects parser phases, speculative execution, lazy or cached results, effect commits, or alternate execution entry points. Apply only the relevant guidance, using current project contracts as the source of truth. This reference does not authorize architectural cleanup or require every system to adopt these phases.

## Responsibilities and execution paths

- In parsers, distinguish recognition (matching input), projection (deriving output), validation, and effects (observable actions or state changes) when their timing or lifetime differs. Preserve boundaries required by the current contract rather than imposing a new phase model.
- For convenience APIs or alternate construction styles, verify promised acceptance, validation, defaults, results, and effect timing. Prefer sharing policy that must remain consistent. Equivalent behavior alone does not require a single implementation; preserve deliberate separate paths unless the task demonstrates a reason to consolidate them or the project requires a shared runtime.

## State and effects

- Preserve boundaries between speculative work (attempts that may be discarded), validation, cached computation (reusing stored results), and committed effects where the system defines them. Discarded attempts and lazy reads (work delayed until needed) must not trigger effects unless the contract explicitly requires them.
- Preserve documented repeat-call behavior, including whether reads are cached and commits are idempotent (repeating an operation has the same effect as doing it once). Do not infer either property from a method name.
- Distinguish repeated successful calls, retries after failure, and simultaneous calls. Establish their guarantees separately when affected; one does not imply the others. Do not introduce concurrency controls unless the requested work requires them.
- Establish whether retained results are snapshots (values fixed at creation) or live views of mutable inputs before removing stored state or changing evaluation timing. A snapshot, cache, or retained failure is not redundant merely because its value could be recomputed now.
- Restore scoped policy and temporary state on relevant failures, exceptions, and early returns according to the owning contract.

## Make affected lifecycle rules visible

Apply only the points affected by the requested change. Use existing types, code, tests, or documentation; do not duplicate a clear contract in several places.

- **Failure guarantees:** show whether failure leaves state unchanged, partly updated, or recoverable. Identify any effects already performed and what callers can safely do next. Keep this information near the operation or its maintained contract.
- **Valid transitions:** when call order matters, show allowed sequences and how invalid calls fail. Use a small state table only when it is clearer than short prose. Do not add states or a state-machine framework merely to document an existing sequence.
- **Resource ownership:** identify who opens, closes, releases, or transfers each affected resource. Make cleanup after early returns and exceptions visible. Distinguish owned resources from borrowed ones.
- **Deferred work:** identify what triggers execution, when errors appear, and which inputs are retained. Explain whether later input changes can affect the result.
- **Consequential example:** when a lifecycle contract changes, provide a short call sequence and expected result where it helps review. Include the relevant failure or retry when consequential. Prefer an existing test or example over duplicate documentation; do not enumerate every possible path.
- **Plain language:** briefly explain necessary specialist terms in parentheses on first use. Keep sentences short and omit definitions already clear in the surrounding document.

## Verification selection

Choose checks for the affected contracts: discarded attempts, repeated reads or commits, retries, simultaneous calls where supported, mutation after result creation, valid and invalid call order, failure guarantees, deferred errors, resource cleanup, and restoration of scoped state after failure. Compare alternate entry points when their shared behavior is affected. Derive expected outcomes from current contracts and callers; these are conditional checks, not a mandatory suite for every structural change.
