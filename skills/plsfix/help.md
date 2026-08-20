plsfix — rewrite vague specs, prompts, and instructions into clear, actionable ones

WHAT IT DOES
  Improves spec documents, prompts, requirements docs, briefs, or any
  written instructions meant to drive action from humans or AI. Applies
  12 writing principles (Structure, Content, Delivery phases,
  synthesized from Anthropic/Google/OpenAI/Microsoft prompt-engineering
  guidance) to flag violations, rewrite the document with minimal
  changes (marking assumed details with [CONFIRM] tags), and produce a
  change report mapping each edit to the principle and rationale
  behind it.

WHAT IT NEEDS
  - A target document (spec, prompt, requirements doc, brief, or any
    instruction set) to read and improve

USAGE
  /plsfix <path-to-document>   Improve the given document
  /plsfix --help                Show this message and exit

FLAGS
  --help    Show this help message without making any changes
