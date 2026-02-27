# plsfix

Turn vague instructions into clear ones. An agent skill that diagnoses and rewrites spec documents, prompts, requirements docs, briefs, and any written instructions meant to drive action from humans or AI.

Give it a document. It identifies what's unclear and why, rewrites it, and hands back both the improved version and a change report mapping every edit to the principle it implements.

Works as a skill in [Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/skills), [OpenAI Codex](https://developers.openai.com/codex/skills/), and [Google Antigravity](https://antigravity.google/docs/skills) — all three use the same `SKILL.md` format. Can also be used as raw principles for direct prompting with any LLM.

## Table of Contents

- [Highlights](#highlights)
- [Installation](#installation)
- [Usage](#usage)
- [What to Use It On](#what-to-use-it-on)
- [Using the Principles Without the Skill](#using-the-principles-without-the-skill)
- [When to Expect the Best Results](#when-to-expect-the-best-results)
- [When NOT to Use plsfix](#when-not-to-use-plsfix)
- [The 12 Principles](#the-12-principles)
- [Contributing](#contributing)
- [References](#references)
- [License](#license)

## Highlights

- **12 research-backed principles** — synthesized from official prompt engineering guidance by Anthropic, Google, OpenAI, and Microsoft, plus 18 academic papers
- **Three-phase workflow** — Structure first, then Content, then Delivery. Fixes the skeleton before polishing sentences.
- **Change report with every rewrite** — every edit cites the principle it implements and why, so the author learns while reviewing
- **Voice preservation** — rewrites sharpen clarity without stripping the author's tone or inventing new requirements
- **Safe assumptions** — anything the skill infers is marked `[CONFIRM]` so the author can verify or override
- **Cross-platform** — same `SKILL.md` file works in Claude Code, OpenAI Codex, and Google Antigravity

## Installation

The `SKILL.md` format is a shared standard across Claude Code, OpenAI Codex, and Google Antigravity. The file is identical — only the installation path differs.

### Claude Code

```bash
mkdir -p ~/.claude/skills/plsfix
cp SKILL.md ~/.claude/skills/plsfix/SKILL.md
```

Verify by running Claude Code and typing `/plsfix`. See [Claude Code skills docs](https://code.claude.com/docs/en/skills) for details.

### OpenAI Codex

```bash
mkdir -p .agents/skills/plsfix
cp SKILL.md .agents/skills/plsfix/SKILL.md
```

Codex scans `.agents/skills/` in your repo, then `$HOME/.agents/skills/` for user-global skills. Invoke with `/skills` or the `$plsfix` mention syntax. See [Codex skills docs](https://developers.openai.com/codex/skills/) for details.

### Google Antigravity

```bash
mkdir -p .antigravity/skills/plsfix
cp SKILL.md .antigravity/skills/plsfix/SKILL.md
```

Antigravity uses semantic triggering — the agent reads the skill's `description` field and activates it automatically when your request matches, or you can invoke it explicitly. See [Antigravity skills docs](https://antigravity.google/docs/skills) for details.

### Any other LLM or agent

If your tool doesn't support the `SKILL.md` format natively, paste the contents of `SKILL.md` into your system prompt or prepend it to your message. The principles and workflow are plain markdown — any LLM can follow them.

## Usage

Invoke the skill, then paste or point it at the document you want improved.

### Before

```text
Build a dashboard. Don't use too many colors. Make sure it's good for executives.
Avoid technical jargon. The data should be recent.
```

### After

```text
Build a Grafana dashboard for the payments service showing three metrics:
p95 latency, error rate, and throughput over the trailing 30 days.

Audience: VP-level executives who review operational health weekly.
Use plain language a non-technical stakeholder would understand.

Format: single-page view, dark theme, with one chart per metric arranged
in a 3-column grid.
```

### Change Report (excerpt)

| # | Principle | Before | After | Rationale |
|---|-----------|--------|-------|-----------|
| 1 | P5: Be specific | "Build a dashboard" | "Build a Grafana dashboard for the payments service showing p95 latency, error rate, and throughput" | Original lacked tool, service, and metrics |
| 2 | P6: Name your audience | "good for executives" | "VP-level executives who review operational health weekly" | Specifying the audience calibrates vocabulary and depth |
| 3 | P9: Say what to do | "Don't use too many colors" / "Avoid technical jargon" | "dark theme" / "Use plain language a non-technical stakeholder would understand" | Negative instructions replaced with positive directives |
| 4 | P7: Define output contract | (missing) | "single-page view, one chart per metric, 3-column grid" | No format specification existed; output shape was undefined |

## What to Use It On

plsfix works on any written instructions meant to drive action. The sweet spot is documents where vague or disorganized writing leads to misinterpretation — by humans or by AI.

### System instruction files

These are the highest-leverage targets. A poorly written system instruction file silently degrades every interaction the agent has. plsfix is particularly effective on:

- **CLAUDE.md / AGENTS.md** — Project-level instructions that shape how coding agents behave across your entire repo. Research shows that as instruction count increases, instruction-following quality decreases uniformly [[ref]](https://www.humanlayer.dev/blog/writing-a-good-claude-md). Every instruction that's vague, contradictory, or buried in the middle costs you compliance on the instructions around it.
- **SKILL.md files** — Other skills you've written. Skills are ephemeral instructions loaded on-demand; they need to be especially clear because the agent has no prior context when it loads them.
- **Custom agents and commands** — Agent definition files, slash command definitions, and any markdown that gets injected into an agent's context window.
- **MEMORY.md and knowledge base files** — Persistent context that agents reference across sessions. Vague memories produce vague behavior.

### Spec and requirements documents

- Product requirements docs (PRDs)
- Technical specs and architecture decision records
- User stories and acceptance criteria
- API contracts and interface definitions
- Test plans

### Prompts used directly with LLMs

- System prompts for chatbots, assistants, and agent loops
- Reusable prompt templates
- Few-shot example sets (plsfix can evaluate whether examples are clear, diverse, and well-structured)

### Operational documents

- Runbooks and playbooks
- Onboarding guides
- Process documentation
- Internal briefs and SOPs

## Using the Principles Without the Skill

You don't need the skill infrastructure to benefit from the 12 principles. They work as a mental checklist or as raw instructions pasted into any LLM conversation.

### As a one-shot prompt

Paste the principles table from `SKILL.md` into a conversation along with the document you want improved:

```text
Here are 12 principles for effective instructions. Review the document
below against each principle. For every violation, rewrite the relevant
section and explain which principle it fixes and why.

[paste principles table]

---

[paste your document]
```

### As a mental checklist

Before sending any important prompt or spec document, scan it against the quick reference:

1. Did I put context before the ask?
2. Does each section have one goal?
3. Are compound instructions broken into steps?
4. Are different content types (instructions, context, examples) visually separated?
5. Did I replace vague nouns with concrete details?
6. Did I say who the audience is?
7. Did I define what "done" looks like?
8. Did I include an example?
9. Did I frame instructions positively (what to do, not what to avoid)?
10. Did I explain why this matters?
11. Did I say what to do when uncertain?
12. Do any of my instructions contradict each other?

### In a custom GPT, assistant, or agent

Add the principles to your system prompt as standing instructions. The agent will apply them when asked to review or improve documents, or you can reference them when writing prompts yourself.

## When to Expect the Best Results

plsfix produces the most dramatic improvements when:

- **The document is medium-length (200-2000 words).** Short enough for the agent to hold the full document in context, long enough to have structural problems worth fixing.
- **The document has clear intent but poor execution.** The author knows what they want but hasn't articulated it well. plsfix clarifies; it doesn't invent.
- **The document will be consumed by an LLM.** System prompts, skills, agent instructions, and prompt templates benefit the most because LLMs are more sensitive to the exact issues the 12 principles address (buried asks, contradictions, missing output contracts) than human readers are.
- **The document is being used repeatedly.** A prompt template used 100 times gets 100x the value from a clarity improvement. A one-off message gets 1x.
- **Multiple people are interpreting the same document.** Ambiguity costs compound with each additional reader. plsfix reduces the surface area for misinterpretation.

Results are solid but less dramatic when the document is already well-written and mostly needs minor tightening — plsfix will correctly report that few principles were triggered.

## When NOT to Use plsfix

plsfix is designed for instruction documents — text that tells someone (or something) what to do. It is the wrong tool for:

- **Creative writing.** Fiction, poetry, essays, marketing copy. These have different goals (voice, persuasion, narrative arc) that the 12 principles don't address and may actively harm. A novel shouldn't have an "output contract."
- **Conversational messages.** Slack messages, emails, chat replies. These are too short and too context-dependent. The overhead of a change report isn't worth it for a three-sentence message.
- **Code.** plsfix works on natural language instructions, not source code. Use a linter.
- **Documents where ambiguity is intentional.** Some strategic documents are deliberately vague to allow flexibility in interpretation. If the author left something open-ended on purpose, plsfix will flag it as a problem. That's a false positive.
- **Documents you haven't read.** Don't run plsfix on a document and accept its output without reading both the change report and the rewrite. The `[CONFIRM]` tags exist because the skill sometimes has to guess at your intent — and it will occasionally guess wrong.
- **Very long documents (5000+ words).** The skill works best when the full document fits comfortably in context. For very long documents, consider breaking them into sections and running plsfix on each section separately.

## The 12 Principles

Ordered by application phase: structure the document first, then sharpen content, then refine delivery.

### Phase 1: Structure

| # | Principle | Symptom it fixes |
|---|-----------|------------------|
| P1 | **Context first, ask last** | Key requirements get missed; output addresses secondary concerns |
| P2 | **One ask per section** | Output oscillates between competing goals or drops some |
| P3 | **Break it into steps** | Output jumbles or skips parts of the task |
| P4 | **Use structural markup** | Reader/AI confuses instructions with examples, or context with the ask |

### Phase 2: Content

| # | Principle | Symptom it fixes |
|---|-----------|------------------|
| P5 | **Be specific, not abstract** | Output is generic or surface-level |
| P6 | **Name your audience** | Tone, depth, or vocabulary is wrong for the reader |
| P7 | **Define the output contract** | Output is correct in substance but wrong in shape, length, or structure |
| P8 | **Show, don't tell** | Reader/AI guesses wrong about what "good" looks like |

### Phase 3: Delivery

| # | Principle | Symptom it fixes |
|---|-----------|------------------|
| P9 | **Say what to do, not what to avoid** | Forbidden behavior still appears; instructions feel restrictive |
| P10 | **Make the stakes real** | Instructions followed mechanically without judgment or care |
| P11 | **Give an out for uncertainty** | Fabricated answers, confident guesses, or silent failures |
| P12 | **Resolve contradictions** | Reader/AI wastes effort resolving ambiguity or picks the wrong side |

See [SKILL.md](SKILL.md) for the full workflow, rewriting rules, diagnosis patterns, and change report format.

## Contributing

Contributions are welcome. The principles in this skill are grounded in published research, so proposed changes should be backed by evidence — either from the official prompt engineering docs of a major LLM provider or from a peer-reviewed paper.

### How to contribute

1. Fork this repo
2. Create a feature branch (`git checkout -b improve-principle-x`)
3. Make your changes to `SKILL.md` and/or `README.md`
4. If adding or modifying a principle, include a link to the supporting source (official docs or paper)
5. Submit a pull request with a clear description of what changed and why

### What makes a good PR

- **Adding a principle:** Must be supported by at least two independent sources (e.g., two different LLM providers' official docs, or one provider doc + one peer-reviewed paper). A single blog post is not sufficient. The principle should address a failure mode not already covered by the existing 12.
- **Modifying a principle:** Explain what's wrong with the current wording and provide evidence for the improvement. Research such as [Principled Instructions Are All You Need](https://arxiv.org/abs/2312.16171) [18] tested 26 prompting principles empirically — if your change conflicts with tested findings, explain why.
- **Reordering principles:** The current order (Structure → Content → Delivery) is based on the consensus across all four providers that structural fixes should precede content fixes. Changes to ordering should cite evidence for the alternative sequence.
- **Fixing bugs or typos:** Just submit the PR. No research citation needed.
- **Adding references:** Always welcome. If you find a paper that provides additional evidence for an existing principle, add it to the References section.

### What doesn't belong

- Principles that are model-specific or version-specific (e.g., "use `reasoning_effort: medium` for GPT-5"). plsfix targets the fundamentals that work across all LLMs.
- Prompt tricks or hacks that don't have a corresponding human communication principle. The core insight of plsfix is that effective human writing and effective AI prompting converge — contributions should reinforce that thesis.

---

## References

### Industry Prompt Engineering Guidelines

The 12 principles are synthesized from official prompt engineering documentation published by the four major LLM providers:

1. [Anthropic: Prompting Best Practices (Claude 4.x)](https://platform.claude.com/docs/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
2. [Anthropic: Prompt Engineering Overview](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview)
3. [Google: Prompt Design Strategies (Gemini API)](https://ai.google.dev/gemini-api/docs/prompting-strategies)
4. [Google Cloud: What is Prompt Engineering](https://cloud.google.com/discover/what-is-prompt-engineering)
5. [OpenAI: Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)
6. [OpenAI: Best Practices for Prompt Engineering](https://help.openai.com/en/articles/6654000-best-practices-for-prompt-engineering-with-the-openai-api)
7. [OpenAI: GPT-5 Prompting Guide](https://developers.openai.com/cookbook/examples/gpt-5/gpt-5_prompting_guide)
8. [Microsoft: Prompt Engineering Techniques (Azure OpenAI)](https://learn.microsoft.com/en-us/azure/ai-foundry/openai/concepts/prompt-engineering)
9. [Microsoft: System Message Design for Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/advanced-prompt-engineering)

### Research Papers

These papers provide the empirical evidence that the same communication principles effective with humans are also effective with LLMs:

10. [Grammarly and Harris Poll: The State of Business Communication (2022)](https://www.businesswire.com/news/home/20220125005525/en/Grammarly-and-Harris-Poll-Research-Estimates-U.S.-Businesses-Lose-%241.2-Trillion-Annually-to-Poor-Communication) — Poor communication costs US businesses $1.2 trillion per year.
11. [NACE Job Outlook Surveys: Employer-Desired Competencies](https://www.naceweb.org/talent-acquisition/candidate-selection/employers-rate-career-competencies-new-hires-proficiency/) — 73-82% of employers rank written communication as a must-have competency.
12. [Grammarly: Good Grammar, Good Career (LinkedIn Profile Analysis)](https://www.grammarly.com/blog/writing-tips/good-grammar-good-career/) — Professionals with fewer language errors tend to reach higher positions.
13. [Do LLMs Write Like Humans? Variation in Grammatical and Rhetorical Styles, PNAS (2025)](https://www.pnas.org/doi/10.1073/pnas.2422455122) — Instruction-tuned models default to noun-heavy, informationally dense style and struggle to deviate.
14. [MIT Sloan: Generative AI Results Depend on User Prompts as Much as Models](https://mitsloan.mit.edu/ideas-made-to-matter/study-generative-ai-results-depend-user-prompts-much-models) — Half the performance gains from a better model came from how users adapted their prompts, not the model itself.
15. [Grammarly 2025: The Critical Role of AI Literacy Across the Enterprise](https://www.grammarly.com/business/learn/role-of-generative-ai-literacy/) — AI-literate workers save 8.9 hours/week vs 6.3 hours for merely familiar workers (41% gap).
16. [Using Prompt Engineering to Better Communicate with People, Harvard Business Review](https://hbr.org/2024/01/using-prompt-engineering-to-better-communicate-with-people) — Skills flow both directions: better AI communication improves human communication and vice versa.
17. [Nielsen Norman Group: Good from Afar, But Far from Good: AI Prototyping in Real Design Contexts](https://www.nngroup.com/articles/ai-prototyping/) — Vague prompts produce generic, randomly assembled designs; detailed prompts produce professional, usable work.
18. [Principled Instructions Are All You Need for Questioning LLaMA-1/2, GPT-3.5/4](https://arxiv.org/abs/2312.16171) — 26 prompting principles tested, yielding 57.7% average quality improvement on GPT-4.
19. [Language Models are Few-Shot Learners (GPT-3, Brown et al., 2020)](https://arxiv.org/abs/2005.14165) — The foundational paper establishing that providing examples in-context dramatically improves LLM output.
20. [Large Language Models are Zero-Shot Reasoners (Kojima et al., 2022)](https://arxiv.org/abs/2205.11916) — Adding "let's think step by step" improved math accuracy from 18% to 79%.
21. [Large Language Models Understand and Can be Enhanced by Emotional Stimuli (EmotionPrompt)](https://arxiv.org/abs/2307.11760) — Positive emotional framing improved performance by up to 115% on BIG-Bench reasoning benchmarks.
22. [NegativePrompt: Leveraging Psychology for LLM Enhancement via Negative Emotional Stimuli](https://arxiv.org/abs/2405.02814) — Negative emotional framing boosted BIG-Bench performance by 46% and outperformed positive framing on instruction-following tasks.
23. [Degradation of Multi-Task Prompting Across Six NLP Tasks and LLM Families](https://www.mdpi.com/2079-9292/14/21/4349) — Multi-task prompts degrade LLM performance compared to single-task prompts, particularly in smaller models.
24. [Lost in the Middle: How Language Models Use Long Contexts (Liu et al., 2023)](https://arxiv.org/abs/2307.03172) — Models perform best when key information appears at beginning or end, with significant degradation for content buried in the middle.
25. [Google Research: Repeat Prompting Improves Non-Reasoning LLMs (2025)](https://arxiv.org/abs/2412.15233) — Simply repeating a prompt twice improved accuracy by up to 76% on non-reasoning tasks.
26. [Order Matters: Rethinking Prompt Construction in In-Context Learning](https://arxiv.org/abs/2511.09700) — Prompt component order matters enormously; context before question outperforms question before context.
27. [World Economic Forum Future of Jobs Report 2025](https://www.weforum.org/publications/the-future-of-jobs-report-2025/) — Analytical thinking, creative thinking, and leadership are increasing in importance as meta-skills.

### Related Reading

- [The Most Important AI Skill Isn't Technical. It's the One You Learned in English Class.](https://tlcmentor.substack.com/p/communication-skills-are-ai-skills) — The article that motivated this skill, exploring why clear writing is the highest-leverage AI skill.

## License

This project is not yet licensed. Consider adding a `LICENSE` file to clarify usage terms.
