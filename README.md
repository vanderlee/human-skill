# Human

A Codex skill for code changes that humans can understand and review. It guides implementation, refactoring, and code review toward clear intent, focused diffs, and evidence-backed explanations.

## What it can do

- Improve names, responsibilities, control flow, and comments while preserving behavior.
- Apply repository formatting standards and choose appropriate design patterns.
- Keep changes focused and verify relevant contracts with proportionate checks.
- Report actionable review findings, separating required fixes from optional improvements.
- Explain the final changes, verification, and remaining limitations for reviewers.

See [SKILL.md](SKILL.md) for the full instructions and [formatting guidance](references/formatting.md) for language-specific defaults.

## Install

Clone this repository into your personal Codex skills directory:

```sh
git clone https://github.com/vanderlee/human-skill.git ~/.agents/skills/human
```

For one project instead, clone into `.agents/skills/human` at the project root. Keep `SKILL.md`, `references/`, and `agents/` together.

Codex detects installed skills automatically; restart it if Human does not appear. See the [official skill documentation](https://learn.chatgpt.com/docs/build-skills) for supported locations.

## Use

Mention `$human` in your Codex prompt, for example:

```text
$human Refactor this method for readability while preserving its behavior.
```

For review requests, the skill reports findings without editing files.
