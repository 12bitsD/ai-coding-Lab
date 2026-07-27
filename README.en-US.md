# AI Coding Experiment Notes

[English Version](./README-en.md)

> From product definition to code generation to continuous maintenance, can the entire process be handled by AI?

This is a real project-driven experiment series. I am using ConceptTree (an AI learning path planner) as the experimental subject to verify an "AI-friendly" development workflow.

## Core Hypothesis

**If I can design an "AI-friendly" development workflow and project structure, the output quality and consistency of AI will improve significantly.**

My role: Decision Maker + Reviewer; I do not write code.

Prerequisite for this hypothesis: A sufficiently clear understanding of the product. AI can execute, but it cannot think through what needs to be done for you. If the product direction itself is vague (and you lack the ability to finalize the product), this method will not help you.

## Article Series

> Want to dive deeper into the methodology? Start with the [first article](articles/01-ai-readable-prd.md).

| # | Title | Status | Core Content |
|---|------|------|----------|
| 01 | [Let AI write the PRD, then let AI understand it](articles/01-ai-readable-prd.md) | Completed | How to write AI-readable PRDs, Socratic questioning process |
| 02 | [From PRD to Code](articles/02-prd-to-code.md) | TBD | How to feed the PRD to AI, comparison of different generation strategies |
| 03 | [AI-friendly Project Structure Design](articles/03-project-structure.md) | TBD | Directory organization, naming conventions, AI maintenance guidelines |
| 04 | [Let AI perform incremental development on existing codebases](articles/04-incremental-dev.md) | TBD | Context management, change isolation, regression testing |
| 05 | [Retrospective: Where AI can replace humans and where the pitfalls are](articles/05-retrospective.md) | TBD | Experience summary, applicable boundaries, pitfall records |

## Case Studies

| Project | Content | Highlights |
|------|------|------|
| [OpenClaw (ClawdBot)](case-studies/openclaw/) | Analysis of Peter's Vibe-coding exemplar | AGENTS.md + SKILL.md + Workflow three-layer model |

## Prompt Templates

| File | Purpose | Applicable Scenario |
|------|------|----------|
| [prd-start.md](prompts/prd-start.md) | PRD Initiation Prompt | Quickly start the PRD process for a new product |
| [product-cofounder.md](prompts/product-cofounder.md) | Product Co-founder Prompt | Use when you need the AI to proactively challenge assumptions |

Difference between the two templates: `prd-start` is fast and suitable for situations where you already know what to build; `product-cofounder` is slower and suitable for exploration stages where you need the AI to help expose self-deceiving assumptions. Using the co-founder prompt to clear the vision first, then switching to the start prompt to rapidly produce the PRD—this is the workflow I currently find most effective.

## Experimental Project: ConceptTree

An AI learning path planner.

Repository: https://github.com/12bitsD/CodeMonkey.git

Reasons for choosing this project:

1. Appropriate scale: 3-5 core pages, complex enough to expose problems but not so much that it spirals out of control.
2. Skill gap: I am a backend engineer; this is a perfect test to see if AI can compensate for my frontend weaknesses.
3. Recursive structure: The product itself integrates AI, creating an "AI writing an AI product" structure.

Development Tools:
- Development + Debugging: Trae, opencode(oh-my-opencode), ClaudeCode, Droid
- Product + DEMO Validation: Claude 4.5 Opus + Gemini 1.5 Pro

## Quick Start

Want to use this method to build a new product?

1. Copy the content of [prompts/prd-start.md](prompts/prd-start.md)
2. Replace `[Product Name]` and `[Background]`
3. Start chatting with the AI and making choices

## License

MIT

---

*I step into the pits with real projects, then write down the experience after climbing out.*
