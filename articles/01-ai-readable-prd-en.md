# 1 Writing an AI-Readable PRD and Helping AI Understand It

## A Number That Broke Me: 2,000 Words

I asked Claude to write the frontend code for a single page.

Version 1: Missing empty states. Fix.

Version 2: Missing loading states. Fix.

Version 3: Missing error handling. Fix.

Version 4: State transition logic was wrong. Fix.

Version 5: Finally working.

Then I counted the length of my requirement description: **2,000 words**.

Just for one page. 2,000 words. I could have written the code myself in maybe 1,500 lines.

**Something is wrong. Isn't AI supposed to make me faster? Why am I spending more time than writing it myself?**

Later, I realized: The problem wasn't the AI; it was me.

My requirement descriptions were written for humans—vague, jumping between points, and assuming the reader would fill in the blanks.

But AI doesn't "fill in the blanks." If you leave something out, it simply doesn't do it.

------

## Why: AI Doesn't "Fill in the Blanks"

Traditional PRDs are written for humans: they are vague, non-linear, and omit common sense. Humans use intuition.

AI does not: omit a state, and it won't be handled; forget an error, and it won't be considered; use an ambiguous word, and it will implement it randomly.

That’s why we need an **AI-readable PRD**—it's not about a change in format, but a change in the granularity of decision-making.

------

## Method: I Choose, AI Writes

```
Traditional: I write doc → Dev asks me → I edit → Dev asks again → I edit again → ...
Now: AI asks me → I choose A/B/C → AI writes doc → Code generated directly
```

**Shift the "questions developers will ask" to the PRD stage, and let AI do the asking.**

### Starting Prompt

```
I want to build [Product Name]. Background: [One sentence].
Help me create a PRD. Use Socratic questioning—ask only one question at a time, providing 2-3 options for me to choose from.
```

AI will follow up: Who are the users? Core features? State transitions? What should the empty state look like?

I only choose A, B, or C. No need to organize language, and nothing gets missed.

------

## 8 Steps

| Step | Action | My Role |
| :--- | :--- | :--- |
| 1. Start | AI asks clarifying questions | Answer |
| 2. Define Concepts | AI lists core entities and states | Choose/Confirm |
| 3. Define Page Structure | AI lists pages and navigation | Confirm |
| 4. Detail Page-by-Page | AI draws ASCII mockups, asks element by element | Choose A/B/C |
| 5. Define AI Interface | AI defines input/output JSON | Confirm |
| 6. Define Data Structure | AI outputs table schema | Confirm |
| 7. Export PRD | Say "Update PRD" | Accept |
| 8. Verify Implementation | Feed code to AI to check for discrepancies | Decide |

**3-page MVP, approx. 2 hours, 50-80 choices.**

------

## Tips After Hitting Walls

### 1. ASCII Prototypes > Figma

Don't give AI Figma screenshots. Let AI output ASCII directly:

```
+------------------+
|     Header       |
+------------------+
|  [Input Box]     |
|  [Generate Btn]  |
+------------------+
|    Result Area   |
|  - Empty: "..."  |
|  - Loading: spinner|
|  - Error: retry  |
+------------------+
```

AI can both generate and parse this. It forces coverage of all states and is fast to modify.

### 2. State Enums > Natural Language

Lesson learned: I wrote "the plan can start, finish, or be archived," and the AI generated identical UIs for "start" and "finish."

Now I write:

```
States:
- draft → editable, no progress shown
- active → show progress, can mark complete
- completed → read-only, show summary
- archived → hidden, can be restored

Transitions:
draft → active: click "Start"
active → completed: all tasks done
any → archived: click "Archive"
```

Enum + UI differences + transition conditions = no omissions.

### 3. JSON Schema for AI Interfaces

Got AI features? Don't use natural language; define the JSON directly:

```json
// Input
{"user_goal": "Learn PyTorch image classification", "time_budget_hours": 20}

// Output
{"path": [{"node": "Tensor Basics", "hours": 2}, {"node": "Autograd", "hours": 3}]}
```

AI can copy this directly when writing code, ensuring field names match.

### 4. SOP for AI Maintenance

After AI generates code, future updates can get messy—the AI might not know which file to edit or project conventions.

Add this to the PRD:

```
## AI Maintenance Guide
Add page: Copy existing structure → Update routes → Add nav entry
Add state: Update type defs → Update state management → Check all references
```

This section is for the AI, not humans. When you say "add a new page," the AI follows the guide and doesn't break things.

------

## Quick Commands

During the PRD process, use these to speed up the conversation:

| Scenario | You Say |
| :--- | :--- |
| Advance process | Continue |
| Indecisive | You decide / Not important |
| Get document | Update PRD |
| Change decision | Change XX to YY |
| Check for omissions| Review for anything missed |
| Skip current page | Skip this page |

------

## Time Reference

| Phase | Duration | Your Action |
| :--- | :--- | :--- |
| Start + Clarify | 5 min | Answer 3-5 questions |
| Define Concepts | 5 min | Choose 1-2 times |
| Define Structure | 5 min | Confirm once |
| Detail Pages | 15-30 min per page | 10-20 choices per page |
| Define AI Interface | 10 min | Confirm 2-3 times |
| Define Data Struct | 5 min | Confirm once |
| Export PRD | 1 min | Say "Update PRD" |

**3-page MVP**: ~2 hours, 50-80 choices.

------

## Next Preview

Now that the PRD is done, the next step is generating code from it.

I will try several approaches:

- **Approach A**: Feed the entire PRD to Claude/GPT for one-shot generation.
- **Approach B**: Split by page, generate individually, then assemble.
- **Approach C**: Let AI generate the project structure/skeleton first, then fill in details.

Then I'll compare which method produces higher quality and more maintainable code.

------

*This series will be continuously updated. I hit the walls in real projects so you don't have to.*
