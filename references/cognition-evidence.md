# Cognitive limitations and code maintenance

Research checked 14 October 2026. This reference explains the skill's guidance; it is not a diagnostic checklist for people or a claim to cover every cognitive limitation. Use it when assessing or explaining a recommendation, not as required reading for every edit.

## Evidence and application

The observations below summarize research. The code-maintenance applications are engineering inferences, except where a software study directly examines the behavior. They should be evaluated against the actual code, task, and intended maintainers.

| Concern | Evidence and limits | Application in the skill |
|---|---|---|
| Working memory and chunking | [Cowan (2001)](https://pubmed.ncbi.nlm.nih.gov/11515286/?dopt=Abstract) describes restricted short-term storage under carefully specified conditions; chunking, rehearsal, and prior knowledge affect estimates. | Reduce simultaneously relevant state and organize coherent operations into meaningful concepts. No four-item, seven-item, parameter-count, or line-count rule follows from this work. |
| Forgetting context and intended next steps | [Parnin and Rugaber (2009)](https://chrisparnin.me/pdf/parnin-icpc09.pdf) observed substantial navigation and recovery activity when developers resumed programming sessions. Session boundaries do not establish that every break was an unrelated interruption. | Keep locality and durable task context; record unresolved assumptions and next steps when a handoff or interruption warrants it. Do not prescribe a universal recovery time. |
| Retrieval cues and external memory | [Parnin and DeLine (2010)](https://www.microsoft.com/en-us/research/publication/evaluating-cues-for-resuming-interrupted-programming-tasks/) studied developers' resumption strategies and tested automated contextual cues. Results concern those cues and tasks, not every documentation technique. | Provide useful names, discoverable contracts, and focused examples so readers can recover meaning without relying on memory. Keep maintained rationale near its owner. |
| Selective attention and unnoticed changes | The [Visual Cognition Lab](https://simonslab.com/research.html) describes its inattentional- and change-blindness research: visible events or changes can be missed outside the current focus of attention. These are perceptual studies, not direct code-review trials. | Make consequential differences visible; inspect changed conditions, error paths, ordering, and defaults deliberately. Avoid burying a semantic change in unrelated churn. |
| Misleading syntax and mental execution | [Langhout and Aniche, Atoms of Confusion in Java (2021)](https://arxiv.org/abs/2103.05424) found that some small syntax patterns increased tracing errors in their studied snippets. Not every candidate pattern showed the same effect. | Prefer explicit precedence and visible state changes when expressions are easy to misread. Inspect actual language semantics; do not ban all compact syntax or assume Java effect sizes transfer to PHP. |
| Naming and semantic decoding | [Hofmeister, Siegmund, and Holt (2019)](https://link.springer.com/article/10.1007/s10664-018-9621-x) compared word, abbreviated, and single-letter identifiers in defect-finding tasks with professional C# developers. | Prefer names that communicate domain meaning over unexplained abbreviations. Naming quality depends on semantics and context; longer names are not automatically better. |
| Missing rationale and mismatched mental models | [Venolia, DeLine, and LaToza (2005)](https://www.microsoft.com/en-us/research/publication/software-development-at-microsoft-observed/) surveyed and interviewed Microsoft developers about code understanding, rationale, coordination, and mental models. Self-reported difficulties are not causal proof of one optimal structure. | Explain surprising decisions and make API behavior match names. Preserve useful distinctions between source, recognition, projected meaning, and committed effects. Write for intended maintainers rather than only the original author. |
| Confirmation bias and initial assumptions | [Salman, Turhan, and Vegas (2019)](https://link.springer.com/article/10.1007/s10664-018-9668-8) found predominantly confirmatory test design in an experiment with graduate students. The study did not find increased confirmatory behavior caused by its time-pressure treatment. | Seek disconfirming examples and verify old findings against current code. Do not assume that deadlines invariably increase bias or that experience and passing tests eliminate it. |

## Interpretation safeguards

[Wyrich, Bogner, and Wagner's mapping study (2022)](https://arxiv.org/abs/2206.11102) documents diversity in how code-comprehension experiments are designed. Treat comprehension as task- and context-dependent; a metric or one experiment is not a universal human limit.

Distinguish understanding a familiar pattern from knowing this implementation's contract. A concise idiom can help an experienced maintainer, while an unfamiliar abstraction can add work. Preserve necessary domain complexity and distinct states; simplify incidental difficulty. Evaluate a change by tracing representative success and failure paths, checking relevant contracts, and explaining the concrete burden removed.

Do not infer a reader's diagnosis, ability, or attention span. Do not invoke disputed broad claims such as a universal decision-fatigue budget, fixed focus duration, or mandatory method size. This skill addresses code and review practices; it does not impose workplace schedules or medical advice.

## Understanding a change, not only the resulting code

[Microsoft's study of how engineers understand code changes (2012)](https://www.microsoft.com/en-us/research/publication/how-do-software-engineers-understand-code-changes-an-exploratory-study-in-industry/?lang=ja) investigated engineers' information needs and identified value in decomposing composite changes around individual development issues. The skill applies this to focused increments and handoffs explaining purpose, behavior, structural moves, a reading route, and verification evidence. This application is an engineering recommendation, not proof of a fixed optimal diff size or an AI-specific experimental result.

## Adapted review practices

The skill adapts these workflow ideas in its own wording, preserving task scope and supported contracts:

- [bholmesdev's simplify](https://github.com/bholmesdev/skills/blob/main/skills/simplify/SKILL.md): code that remains understandable without the author's conversation history, accessible significant operations, and consideration of derivable state. The skill retains deliberate caches and snapshots rather than banning stored derived values.
- [Addy Osmani's code-review-and-quality](https://github.com/addyosmani/agent-skills/blob/main/skills/code-review-and-quality/SKILL.md): distinguish required fixes from optional feedback and propose specific structural remedies. The skill does not adopt fixed change-size thresholds or blanket mutation-testing requirements.
- [Superpowers requesting-code-review](https://github.com/obra/superpowers/blob/main/skills/requesting-code-review/SKILL.md): give reviewers requirements, a change description, and accurate review boundaries without relying on full session history. The skill does not mandate subagents or external actions.

These are inspected workflow instructions, not experimental evidence of effectiveness.
