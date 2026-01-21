# 🧪 AI Coding Lab Notes

> From product definition to code generation to continuous maintenance, can AI handle the entire process?

This is a series of experiments driven by real-world projects. I'm using **ConceptTree** (an AI-powered learning path planner) as the experimental subject to validate and refine a set of "AI-friendly" development processes and methodologies.

## Core Hypothesis

**If I can design an "AI-friendly" development process and project structure, the quality and consistency of AI output will significantly improve.**

My role: **Decision Maker + Reviewer**, no manual coding.

## Article Series

> Want to dive deep into the methodology?
>
> Start reading from [the first article](articles/01-ai-readable-prd-en.md).

| # | Title | Status | Core Content |
|---|------|------|----------|
| 01 | [Writing an AI-Readable PRD and Helping AI Understand It](articles/01-ai-readable-prd-en.md) | ✅ Completed | How to write an AI-readable PRD, Socratic questioning process |
| 02 | [From PRD to Code](articles/02-prd-to-code.md) | 📝 Drafting | How to feed content to AI, comparison of generation strategies |
| 03 | [AI-Friendly Project Structure Design](articles/03-project-structure.md) | 📝 Drafting | Directory organization, naming conventions, AI maintenance guidelines |
| 04 | [Incremental Development on Existing Codebases](articles/04-incremental-dev.md) | 📝 Drafting | Context management, change isolation, regression testing |
| 05 | [Retrospective: Where AI Replaces Humans and Where the Pitfalls Are](articles/05-retrospective.md) | 📝 Drafting | Lessons learned, application boundaries, pitfall records |

## Prompt Templates (Continuously Updated)

| File | Purpose | Application Scenario |
|------|------|----------|
| [prd-start.md](prompts/prd-start.md) | PRD Starting Prompt | Quickly start the PRD process for a new product |
| [product-cofounder.md](prompts/product-cofounder.md) | Product Co-founder Prompt | Use when you need AI to more actively challenge assumptions |

## Experimental Project:

ConceptTree (AI Learning Path Planner)

Repository: https://github.com/12bitsD/CodeMonkey.git
SSH: git@github.com:12bitsD/CodeMonkey.git

Feel free to give it a ⭐

Why choose this project:

1. **Appropriate Scale**: 3-5 core pages, complex enough to expose problems but not out of control.
2. **Skill Gap**: I am a backend engineer, which makes it perfect to test if AI can fill my frontend gaps.
3. **Recursive Structure**: The product itself integrates AI, creating an "AI writing an AI product" loop.

Development Toolstack:

- **Dev + Debug**: Trae, opencode (oh-my-opencode), ClaudeCode, Droid
- **Product + Demo Validation**: Claude (4.5 Opus) + Gemini 1.5 Pro

## Quick Start

### Want to build a new product using this methodology?

1. Copy the content from [prompts/prd-start.md](prompts/prd-start.md)
2. Replace `[Product Name]` and `[Background]`
3. Start conversing with AI and make your choices

## License

MIT

---

*This series will be continuously updated. I'm hitting the walls in real projects so you don't have to.*
