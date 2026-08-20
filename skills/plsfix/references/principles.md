# The 12 Principles — Full Reference

Consult this during Step 1 (Read and Diagnose) to check a document against each principle, or to look up which principle applies to a symptom you've spotted.

Principles are ordered by application sequence: structure the document first, then sharpen content, then refine delivery.

### Phase 1: Structure (get the bones right)

| # | Principle | One-liner | Symptom it fixes |
|---|-----------|-----------|------------------|
| P1 | **Context first, ask last** | Lead with background and constraints, close with the actual request; never bury the ask in the middle | Key requirements get missed; output addresses secondary concerns. *Diagnostic: core requirement buried in paragraph 4 of 6.* |
| P2 | **One ask per section** | Split multi-goal paragraphs so each section has exactly one objective | Output oscillates between competing goals or drops some. *Diagnostic: section tries to accomplish multiple goals.* |
| P3 | **Break it into steps** | Decompose compound instructions into sequential, numbered steps | Output jumbles or skips parts of the task. *Diagnostic: run-on paragraph with 3+ distinct instructions.* |
| P4 | **Use structural markup** | Separate instructions, context, examples, and inputs with consistent delimiters (XML tags, markdown headers, or `---` separators) | Reader/AI confuses instructions with examples, or context with the ask. *Diagnostic: instructions, examples, and context mixed together with no separators.* |

### Phase 2: Content (sharpen what you're saying)

| # | Principle | One-liner | Symptom it fixes |
|---|-----------|-----------|------------------|
| P5 | **Be specific, not abstract** | Replace vague nouns with concrete details: audience, format, scope, quantities | Output is generic or surface-level. *Diagnostic: "Build a good X", "make it effective", "ensure quality".* |
| P6 | **Name your audience** | State who will read/act on the output and what they already know | Tone, depth, or vocabulary is wrong for the reader. *Diagnostic: no mention of who the output is for.* |
| P7 | **Define the output contract** | Specify what "done" looks like: format, length, structure, required fields | Output is correct in substance but wrong in shape, length, or structure. *Diagnostic: no specification of output format, length, or structure.* |
| P8 | **Show, don't tell** | Add 1-3 examples of desired output | Reader/AI guesses wrong about what "good" looks like. *Diagnostic: no examples of desired output anywhere in doc.* |

### Phase 3: Delivery (refine how you're saying it)

| # | Principle | One-liner | Symptom it fixes |
|---|-----------|-----------|------------------|
| P9 | **Say what to do, not what to avoid** | Rewrite "don't" and "avoid" instructions as positive directives | Forbidden behavior still appears; instructions feel restrictive rather than enabling. *Diagnostic: multiple "don't", "avoid", "never" instructions.* |
| P10 | **Make the stakes real** | State why this matters: who benefits, what breaks if done wrong, what success enables | Instructions followed mechanically without judgment or care. *Diagnostic: no explanation of why the task matters.* |
| P11 | **Give an out for uncertainty** | Explicitly state what to do when information is missing, the request is ambiguous, or the task is out of scope | Fabricated answers, confident guesses, or silent failures when the task can't be completed as written. *Diagnostic: no guidance on what to do when uncertain or out of scope.* |
| P12 | **Resolve contradictions** | Ensure no two instructions conflict; when tensions exist, state which takes priority | Reader/AI wastes effort resolving ambiguity, or silently picks the wrong side of a conflict. *Diagnostic: two instructions that contradict each other (e.g., "be brief" and "be comprehensive").* |
