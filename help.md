plsfix — rewrite vague specs, prompts, and instructions into clear, actionable ones

WHAT IT DOES
  Improves spec documents, prompts, requirements docs, briefs, and other
  written instructions. These are instructions meant to drive action by
  humans or AI.

  plsfix applies 12 writing principles in three phases: Structure,
  Content, and Delivery. It draws these principles from Anthropic,
  Google, OpenAI, and Microsoft prompt-engineering guidance.

  plsfix flags each principle violation. It rewrites the document with
  minimal changes, and marks each assumed detail with a [CONFIRM] tag.
  It then produces a change report. Each report row names the
  principle and gives a Rationale for the edit.

WHAT IT NEEDS
  - A target document to read and improve: a spec, a prompt, a
    requirements doc, a brief, or any other instruction set

USAGE
  /plsfix <path-to-document>   Improve the given document
  /plsfix <path-to-document> --ste   Improve the document, enforcing ASD-STE100 language rules
  /plsfix --help                Show this message and exit

FLAGS
  --help    Show this help message without making any changes
  --ste     Also enforce ASD-STE100 (Simplified Technical English) language rules
