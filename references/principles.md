# The 12 Principles — Full Reference

Consult this file during Step 1 (Read and Diagnose). Check the document against each principle, or look up which principle matches a symptom you have found.

These principles follow a fixed order: structure the document first, then sharpen its content, then refine its delivery.

### Phase 1: Structure (fix the document's organization)

| # | Principle | One-liner | Symptom it fixes |
|---|-----------|-----------|------------------|
| P1 | **Context first, ask last** | Lead with background and constraints. Close with the actual request. Never bury the ask in the middle. | Readers miss key requirements, and the output addresses secondary concerns instead. *Diagnostic: core requirement buried in paragraph 4 of 6.* |
| P2 | **One ask per section** | Split a multi-goal paragraph so each section has exactly one objective. | The output swings between competing goals, or it drops some of them. *Diagnostic: section tries to accomplish multiple goals.* |
| P3 | **Break it into steps** | Break a compound instruction into sequential, numbered steps. | The output jumbles steps, or it skips parts of the task. *Diagnostic: run-on paragraph with 3+ distinct instructions.* |
| P4 | **Use structural markup** | Separate instructions, context, examples, and inputs with consistent delimiters (XML tags, markdown headers, or `---` separators). | A reader or an AI confuses the instructions with the examples, or confuses the context with the actual request. *Diagnostic: instructions, examples, and context mixed together with no separators.* |

### Phase 2: Content (make the message clear)

| # | Principle | One-liner | Symptom it fixes |
|---|-----------|-----------|------------------|
| P5 | **Be specific, not abstract** | Replace vague nouns with concrete details: audience, format, scope, quantities. | The output is generic or shallow. *Diagnostic: "Build a good X", "make it effective", "ensure quality".* |
| P6 | **Name your audience** | State who will read or act on the output, and what they already know. | Tone, depth, or vocabulary is wrong for the reader. *Diagnostic: no mention of who the output is for.* |
| P7 | **Define the output contract** | Specify what "done" looks like: format, length, structure, required fields. | The output is correct in substance but wrong in shape, length, or structure. *Diagnostic: the document never states the output's format, length, or structure.* |
| P8 | **Show, don't tell** | Add 1-3 examples of the output you want. | A reader or an AI guesses wrong about what "good" looks like. *Diagnostic: the document has no examples of the output you want, anywhere.* |

### Phase 3: Delivery (improve the tone and wording)

| # | Principle | One-liner | Symptom it fixes |
|---|-----------|-----------|------------------|
| P9 | **Say what to do, not what to avoid** | Turn "don't" and "avoid" instructions into positive ones. | The forbidden behavior still appears. The instructions feel restrictive, not helpful. *Diagnostic: multiple "don't", "avoid", "never" instructions.* |
| P10 | **Make the stakes real** | State why this matters: who benefits, what breaks if done wrong, what success enables. | Readers follow the instructions automatically, without judgment or care. *Diagnostic: the document never explains why the task matters.* |
| P11 | **Give an out for uncertainty** | Clearly state what to do when information is missing, the request is ambiguous, or the task is out of scope. | The AI fabricates answers, guesses with false confidence, or fails silently when it cannot finish the task as written. *Diagnostic: the document gives no guidance for when the task is uncertain or out of scope.* |
| P12 | **Resolve contradictions** | Make sure no two instructions conflict. When a conflict exists, state which instruction takes priority. | A reader or an AI wastes effort trying to resolve the ambiguity, or silently picks the wrong side of a conflict. *Diagnostic: two instructions that contradict each other (for example, "be brief" and "be comprehensive").* |
