# Contracts, OOP, SOLID, and structural review criteria

Apply these rules to the requested structural work and its affected code. Classify by behavior and maintenance consequences, not by names, file length, or a pattern catalog. Follow the user's requirements and the repository's actual contracts.

## Contracts and object boundaries

Make affected contracts explicit through the project's types, interfaces, implementation, tests, and documentation. Establish valid inputs, observable results, failure behavior, state changes, and lifecycle constraints where relevant. Do not duplicate every contract in every form.

Keep objects responsible for protecting their own invariants. Prefer operations that express domain intent over exposing mutable internals and requiring callers to coordinate correctness. Keep dependencies visible and distinguish who owns state from who merely uses it. A contract is behavioral; an interface declaration alone does not establish it.

## SOLID as reasoning for reviewers

| Principle | Reasoning and review question | Boundary against unnecessary structure |
|---|---|---|
| Single responsibility | Group behavior around a coherent responsibility and reason to change. Can the reviewer locate the owner of a policy without reconstructing several unrelated concerns? | Responsibility does not mean one method; do not create a class for every step. |
| Open/closed | Where supported variation exists, isolate it so adding an implementation does not repeatedly disturb stable policy. What actual variation does this boundary accommodate? | Do not introduce extension points for hypothetical requirements or avoid necessary changes to an existing contract. |
| Liskov substitution | Implementations must honor the contract callers rely on: accepted inputs, results, failures, mutation, and lifecycle behavior. Can a caller use each implementation under the same promised conditions? | Shared signatures alone do not establish substitutability. Do not strengthen preconditions or weaken guarantees relative to the supported contract. |
| Interface segregation | Give consumers operations they meaningfully need. Can a reviewer understand the dependency without learning unrelated capabilities? | Split interfaces when consumer roles differ, not mechanically into one-method interfaces. |
| Dependency inversion | Keep domain policy independent of incidental infrastructure where that boundary matters. Does the dependency express the consumer's needs, and can the reviewer see which policy depends on which mechanism? | Dependency injection does not require a container or an interface for every class. Retain concrete dependencies where another abstraction adds no useful boundary. |

Prefer composition when behavior must vary independently or inheritance cannot preserve the parent contract. Use inheritance when the subtype relationship and supported extension contract are real. A straightforward function or concrete class remains appropriate when additional structure would add navigation without protecting a meaningful boundary.

For consequential structural changes, identify what is promised, who guarantees it, and how callers rely on it. Justify an abstraction by the concrete uncertainty, coupling, or repeated change it removes. These criteria guide the requested implementation; they do not authorize architectural rewrites or turn preferences into mandatory findings. Apply the evidence threshold below.

## Pattern selection

Start with the concrete problem and the current owners of its state and behavior. Choose the simplest design that protects the relevant invariant, removes duplicated policy, or isolates an actual source of change. Prefer composition when reuse does not require a substitutable specialization. Introducing interfaces, inheritance, or additional objects needs an actual boundary or variation to represent; anticipated future flexibility alone is insufficient.

| Problem in the current task | Pattern to consider | Required fit and tradeoff |
|---|---|---|
| Several implementations of one policy must vary independently of the caller | Strategy | Share a real contract and keep selection explicit. A callable or small function may be sufficient; a single stable branch does not demand a strategy hierarchy. |
| A domain object must consistently satisfy construction invariants or support several real construction paths | Factory or Builder | Centralize meaningful creation policy. Do not wrap an ordinary constructor in a redundant factory, or introduce mutable builder state without a construction problem. |
| Clients compose individual and grouped operations through one contract | Composite | Preserve consistent delegation and child ownership. Avoid special-case dispatch that defeats the shared contract. |
| A behavior wraps an existing operation while retaining its contract | Decorator | Make effect timing and ordering explicit. Wrapping must not unexpectedly change consumption, caching, mutation, or return semantics. |
| An external interface differs from an owned internal contract | Adapter or Facade | Translate at the real boundary or simplify entry into the same implementation. Do not create a second runtime or bypass a repository's ban on compatibility adapters. |
| Behavior varies across explicit lifecycle states with meaningful transition rules | State | Use when transition complexity warrants it. Keep legitimate distinctions; a few understandable flags do not require a class per state. |
| A sequence of phases is invariant while supported implementations vary a phase | Template Method | Use only where the inheritance and extension contract are intentional and substitutable. Private helper extraction alone is not a reason to add extension hooks. |

These are candidates, not a required list to implement. Other patterns require the same concrete fit and proportionality. A familiar pattern can still be inappropriate when it increases navigation, hides coupling, or changes observable behavior.

## Structural concerns in new or changed code

The criteria below guide investigation; they are not independent mandatory project rules. Require concrete evidence of harm in the affected behavior or maintenance task, or an explicit applicable project or user prohibition, before classifying a concern as a mandatory violation. Explain the affected path or concrete change burden. Pattern names and hypothetical future difficulty alone are insufficient; otherwise classify a proposed improvement as optional. Established framework integration conventions remain valid unless this evidence or prohibition applies.

- **God object or unrelated responsibilities:** do not add unrelated policies, resource lifetimes, or domain decisions to a catch-all owner. Cohesive orchestration is allowed; diagnose mixed responsibilities from independently changing concerns, not line count.
- **Spaghetti control flow and hidden effects:** do not introduce tangled branches or dispersed mutations that obscure execution order, cleanup, or failure handling. Make ownership and scope restoration explicit; preserve required early exits.
- **Hidden dependencies and ambient mutable state:** investigate service-locator lookups, mutable globals, or singleton state when they obscure required dependencies or leak per-operation state. Show the concrete failure or maintenance burden before requiring a change. Framework-prescribed lookups, explicitly scoped state, and immutable shared data are not violations by themselves.
- **Duplicated policy and divergent execution paths:** do not copy the same invariant or algorithm into paths that must stay consistent. Centralize the shared policy without merging operations whose semantics genuinely differ.
- **Undocumented temporal coupling:** do not require callers to infer a prerequisite call sequence, mutation phase, or resource lifetime. Make the lifecycle explicit in the interface or maintained contract; legitimate ordered workflows are allowed when enforced or clear.
- **Speculative generality and needless indirection:** do not add unused abstractions, redundant forwarding layers, or inheritance whose only purpose is hypothetical reuse. Do not create an interface per class or a factory per constructor by default.
- **Inheritance that violates the parent contract:** do not use subclassing solely to reuse code when the subclass cannot honor supported parent behavior. Preserve deliberate typed extension contracts and substitutable implementations.
- **Primitive representations that hide a material invariant:** do not use indistinguishable values or Boolean switches where callers can readily confuse units, ownership, or incompatible states. Prefer an existing appropriate type or clear interface; do not wrap every scalar or change public APIs incidentally.
- **Swallowed errors and misleading fallbacks:** do not silently catch failures, substitute success, or use fallback dispatch that conceals a violated contract. Preserve explicit optional/probe semantics and documented recovery; report meaningful failure context at its owning boundary.
- **Change scattering caused by misplaced ownership:** do not distribute one concern across unrelated owners so a normal policy change requires coordinated edits throughout them. Distinguish this from necessary updates to implementations, callers, tests, and documentation.

When a label could apply to a valid design, use the concrete definition above to establish a violation. For example, a parser session owning scoped state is not a god object, a source boundary facade is not needless indirection, and distinct failure/consumption values are not redundant state.

## Enforcement and scope

Before finishing, trace the affected call path and identify the owners of dependencies, state, invariants, and effects. Confirm that a chosen pattern solves an existing problem and assess structural concerns using the evidence threshold above. Use relevant behavioral checks to verify the preserved contracts.

For implementation work, correct demonstrated in-scope violations instead of merely listing them; optional design preferences do not authorize architectural cleanup. If correction would require unrelated public API changes or broader architecture work, report the remaining violation and its concrete impact; do not claim full compliance or stop otherwise authorized work. For review-only requests, report findings without editing.

Preserve existing user work and behavior during maintenance. A structural concern does not authorize reintroducing removed APIs, changing compatibility policy, or implementing a future roadmap. The project's current contracts remain authoritative, including shared execution paths and distinct lifecycle phases.
