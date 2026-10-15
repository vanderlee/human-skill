# Execution and lifecycle contracts

Read this reference only when the requested work affects parser phases, speculative execution, lazy or cached results, effect commits, or alternate execution entry points. Apply only the relevant guidance, using current project contracts as the source of truth. This reference does not authorize architectural cleanup or require every system to adopt these phases.

## Responsibilities and execution paths

- In parsers, distinguish recognition, projection, validation, and effects when their timing or lifetime differs. Preserve boundaries required by the current contract rather than imposing a new phase model.
- For convenience APIs or alternate construction styles, verify promised acceptance, validation, defaults, results, and effect timing. Prefer sharing policy that must remain consistent. Equivalent behavior alone does not require a single implementation; preserve deliberate separate paths unless the task demonstrates a reason to consolidate them or the project requires a shared runtime.

## State and effects

- Preserve boundaries between speculative work, validation, cached computation, and committed effects where the system defines them. Discarded attempts and lazy reads must not trigger effects unless the contract explicitly requires them.
- Preserve documented repeat-call behavior, including whether reads are cached and commits are idempotent. Do not infer either property from a method name.
- Establish whether retained results are snapshots or live views of mutable inputs before removing stored state or changing evaluation timing. A snapshot, cache, or retained failure is not redundant merely because its value could be recomputed now.
- Restore scoped policy and temporary state on relevant failures, exceptions, and early returns according to the owning contract.

## Verification selection

Choose checks for the affected contracts: discarded attempts, repeated reads or commits, mutation after result creation, and restoration of scoped state after failure. Compare alternate entry points when their shared behavior is affected. Derive expected outcomes from current contracts and callers; these are conditional checks, not a mandatory suite for every structural change.
