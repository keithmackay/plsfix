# Contributing to plsfix

Contributions are welcome. The principles in this skill are grounded in published research, so proposed changes should be backed by evidence — either from the official prompt engineering docs of a major LLM provider or from a peer-reviewed paper.

## Reporting Bugs

Open an issue using the bug report template. Include:

- What you gave plsfix (document type, approximate length)
- What you expected it to do
- What it actually did (include the change report if relevant)

## Suggesting Features or New Principles

Open an issue using the feature request template. For new principles, include at least two independent sources (two different LLM provider docs, or one provider doc + one peer-reviewed paper). A single blog post is not sufficient.

## Development Setup

plsfix has no build system — it's a `SKILL.md` file and a `README.md`. Getting started is:

```bash
git clone https://github.com/keithmackay/plsfix.git
cd plsfix
```

To test your changes, install the skill locally in Claude Code:

```bash
ln -sf "$(pwd)" ~/.claude/skills/plsfix
```

Then invoke `/plsfix` in Claude Code and run it against a test document.

## Pull Request Process

1. Fork the repo
2. Create a feature branch (`git checkout -b improve-principle-x`)
3. Make your changes to `SKILL.md` and/or `README.md`
4. If adding or modifying a principle, include a link to the supporting source
5. Submit a pull request with a clear description of what changed and why

### What makes a good PR

- **Adding a principle** — Must be supported by at least two independent sources. The principle should address a failure mode not covered by the existing 12.
- **Modifying a principle** — Explain what's wrong with the current wording and provide evidence for the improvement.
- **Reordering principles** — The current order (Structure → Content → Delivery) reflects consensus across all four providers. Changes to ordering should cite evidence.
- **Fixing bugs or typos** — Just submit the PR. No citation needed.
- **Adding references** — Always welcome.

### What doesn't belong

- Principles that are model-specific or version-specific (e.g., "use `reasoning_effort: medium` for GPT-5"). plsfix targets fundamentals that work across all LLMs.
- Prompt tricks that don't have a corresponding human communication principle. The core insight of plsfix is that effective human writing and effective AI prompting converge.

## Code Style

There is no code. For prose in `SKILL.md` and `README.md`:

- Match the existing register (direct, evidence-backed, no buzzwords)
- Every claim about LLM behavior should cite a source
- Use `[CONFIRM]` tags in examples where assumptions are made
