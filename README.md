# plsfix

Turn vague instructions into clear ones. A [Claude Code skill](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/skills) that diagnoses and rewrites spec documents, prompts, requirements docs, briefs, and any written instructions meant to drive action from humans or AI.

Give it a document. It identifies what's unclear and why, rewrites it, and hands back both the improved version and a change report mapping every edit to the principle it implements.

## Highlights

- **12 research-backed principles** — synthesized from official prompt engineering guidance by Anthropic, Google, OpenAI, and Microsoft, plus 18 academic papers
- **Three-phase workflow** — Structure first, then Content, then Delivery. Fixes the skeleton before polishing sentences.
- **Change report with every rewrite** — every edit cites the principle it implements and why, so the author learns while reviewing
- **Voice preservation** — rewrites sharpen clarity without stripping the author's tone or inventing new requirements
- **Safe assumptions** — anything the skill infers is marked `[CONFIRM]` so the author can verify or override

## Getting Started

### Prerequisites

- [Claude Code CLI](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview) installed and authenticated

### Installation

Copy `SKILL.md` into your Claude Code skills directory:

```bash
mkdir -p ~/.claude/skills/plsfix
cp SKILL.md ~/.claude/skills/plsfix/SKILL.md
```

Or clone and copy:

```bash
git clone <repo-url> && cp plsfix/SKILL.md ~/.claude/skills/plsfix/SKILL.md
```

### Verify

Run Claude Code and type `/plsfix`. If the skill loads, you're set.

## Usage

Invoke the skill by typing `/plsfix` in Claude Code, then paste or point it at the document you want improved.

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
