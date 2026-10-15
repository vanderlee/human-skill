# Evidence for review decisions

Optional supporting evidence, selected from the research inventory reviewed 15 October 2026. Consult a row only when its decision is relevant to the task. This reference does not introduce mandatory checks or authorize cleanup.

The practical rules in SKILL.md apply proportionately within the requested scope. The applications below are engineering inferences unless a study directly investigates the stated programming/review behavior. Supporting evidence for related existing guidance remains in [cognition-evidence.md](cognition-evidence.md).

## Layout decisions

| Decision | Evidence and limitation | Application |
|---|---|---|
| How to lay out affected control flow | [Morzeck et al. (2023)](https://doi.org/10.5220/0012087500003538) studied 20 students/professionals answering Java control-flow questions and found indentation improved response time. The task does not establish a universal optimal indentation width or method size. | Align visual nesting and grouping with behavior and the project formatter. Do not impose aesthetic symmetry on operations with different contracts. |

## Pattern selection

| Decision | Evidence and limitation | Application |
|---|---|---|
| Whether a familiar pattern fits this change | [Sergeyuk et al. (2023)](https://arxiv.org/abs/2307.06673) investigated familiar solutions constraining programming problem solving. Their tested editor-color and forget-previous-solutions interventions were insufficient to overcome the effect. | Verify that a familiar design pattern fits the present problem; consider a simpler alternative. Do not claim that a theme change or an instruction to forget reliably removes bias. |

## Reasoning, attention, and review

| Decision | Evidence and limitation | Application |
|---|---|---|
| What counts as verification evidence | [Perry et al. (2023)](https://arxiv.org/abs/2211.03622) found security and confidence problems under a particular code-assistant model and task set. | Separate generated explanations from checked evidence. Use independent contract checks for consequential decisions without imposing ritual friction on every small edit. Do not generalize effect sizes to current assistants. |
| Where to direct review attention | [Fregnan et al. (2022)](https://arxiv.org/abs/2208.04259) combined repository analysis with a 106-participant experiment: position affected detection of one seeded defect type, not the other. | Provide a meaningful reading route and highlight consequential later files. Do not assume alphabetical order distributes attention evenly or invent a universally optimal ordering. |
| How to connect a requirement to its implementation | [Lawrance et al. (2008)](https://web.engr.oregonstate.edu/~burnett/Reprints/chi08-infoForage.pdf) modeled programmers' navigation during maintenance using linguistic cues and code topology. This supports navigation reasoning, not a guarantee of correctness from naming alone. | Make requirement vocabulary, names, module ownership, and test links guide reviewers to relevant behavior; avoid wrappers that hide its location. |
| How to use previous review comments | [Spadini, Calikli and Bacchelli (2020)](https://eprints.gla.ac.uk/258206/) studied 85 developers. Existing comments could help detect another instance of an otherwise overlooked bug type; the findings do not establish that prior comments are universally harmful. | Treat findings as useful search cues, verify each occurrence, and cover relevant areas beyond the existing discussion. Do not mandate hiding comments or equate previous approval with complete review. |

## Evaluation and limits

Prefer observable review outcomes: whether the intended maintainer understands the changed contract, finds relevant implementation and evidence, and identifies a known problem with less reconstruction work. Assess preference, response time, confidence, and defect detection separately. No single metric proves overall review quality.

Use these findings only to inform the decisions above within the requested scope. They do not establish universal limits, prove the skill improves review quality, or justify changes to program contracts. Generic advice about clear handoffs, presentation, and contract tracing remains in SKILL.md without a separate catalog of indirect psychological explanations.
