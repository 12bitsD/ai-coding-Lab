# AI Coding Lab Notes

> From product definition to code generation to ongoing maintenance, can AI handle the whole thing?

This is a series of experiments driven by a real project. I'm using ConceptTree (an AI learning path planner) to validate an "AI-friendly" development workflow.

## Core Hypothesis

**If I can design an "AI-friendly" development process and project structure, AI output quality and consistency will significantly improve.**

My role: decision maker + reviewer. I don't write code.

This hypothesis has a prerequisite: you need sufficiently clear product understanding. AI can execute, but it can't figure out what you should build. If your product direction is fuzzy, this method won't help.

## Article Series

> Want to dive deep into the methodology? Start from [the first article](articles/01-ai-readable-prd-en.md).

| # | Title | Status | Core Content |
|---|------|------|----------|
| 01 | [Writing an AI-Readable PRD](articles/01-ai-readable-prd-en.md) | Done | How to write AI-readable PRDs, Socratic questioning process |
| 02 | [From PRD to Code](articles/02-prd-to-code.md) | Pending | How to feed to AI, comparing generation strategies |
| 03 | [AI-Friendly Project Structure Design](articles/03-project-structure.md) | Pending | Directory organization, naming conventions, AI maintenance guides |
| 04 | [Incremental Development on Existing Codebases](articles/04-incremental-dev.md) | Pending | Context management, change isolation, regression testing |
| 05 | [Retrospective: Where AI Replaces Humans and Where It Doesn't](articles/05-retrospective.md) | Pending | Lessons learned, boundaries of applicability, pitfall records |

## Prompt Templates

| File | Purpose | When to Use |
|------|------|----------|
| [prd-start.md](prompts/prd-start.md) | PRD Starting Prompt | Quickly start a PRD process for a new product |
| [product-cofounder.md](prompts/product-cofounder.md) | Product Co-founder Prompt | When you need AI to actively challenge your assumptions |

Difference between the two: prd-start is fast, good when you already know what you're building; product-cofounder is slower, good when you're still exploring and need AI to poke holes in your assumptions. Use cofounder to think it through, then switch to start to quickly produce the PRD—that's the flow I've found works best.

## Experimental Project: ConceptTree

AI learning path planner.

Repo: https://github.com/12bitsD/CodeMonkey.git

Why this project:

1. Right scale: 3-5 core pages, complex enough to expose problems, not complex enough to spiral out of control
2. Skill gap: I'm a backend engineer, perfect for testing if AI can fill my frontend gaps
3. Recursive structure: the product itself integrates AI, creating an "AI writing an AI product" loop

Dev tools:
- Dev + Debug: Trae, opencode (oh-my-opencode), ClaudeCode, Droid
- Product + Demo Validation: Claude 4.5 Opus + Gemini 1.5 Pro

## Quick Start

Want to build a new product using this methodology?

1. Copy the content from [prompts/prd-start.md](prompts/prd-start.md)
2. Replace `[Product Name]` and `[Background]`
3. Start conversing with AI, make your choices

## License

MIT

---

*I hit walls in real projects, then write them down.*
