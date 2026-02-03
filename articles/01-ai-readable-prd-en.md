# 1 Writing an AI-Readable PRD

## A Number That Broke Me: 2,000 Words

I asked Claude to write frontend code for a single page.

Version 1: missing empty states. Fix. Version 2: missing loading states. Fix. Version 3: missing error handling. Fix. Version 4: state transition logic wrong. Fix. Version 5: finally works.

Then I counted my requirement description: 2,000 words.

For one page. I could've written the code myself in maybe 1,500 lines.

Something's off. Isn't AI supposed to make me faster? Why am I spending more time than writing it myself?

Figured it out later: the problem wasn't AI. It was me.

My requirements were written for humans—vague, jumping around, assuming the reader would fill in the gaps. But AI doesn't fill gaps. If you leave something out, it simply doesn't do it.

------

## AI Doesn't Fill in the Blanks. That's the Core Problem.

Traditional PRDs are for humans: vague, non-linear, skipping common sense. Humans infer.

AI doesn't: skip a state, it won't handle it; forget an error case, it won't consider it; use an ambiguous word, it'll implement whatever.

Hence AI-readable PRD—not a format change, but a granularity change in decision-making.

Put simply: questions that developers used to ask you? Now you answer them in the PRD phase. Don't answer, AI guesses.

------

## The Method: I Choose, AI Writes

```
Traditional: I write doc → Dev asks me → I edit → Dev asks again → I edit again → ...
Now: AI asks me → I pick A/B/C → AI writes doc → Code generated directly
```

Core idea: front-load "questions developers would ask" to the PRD phase. Let AI do the asking.

### Starting Prompt

```
I want to build [Product Name]. Background: [One sentence].
Help me create a PRD. Socratic questioning—ask one question at a time, give 2-3 options for me to choose.
```

AI will follow up: Who are the users? Core features? State transitions? What does empty state look like?

I just pick A, B, or C. No need to organize language, nothing gets missed.

The upside: fast decisions, high coverage. The downside: if you don't deeply understand your product, you'll find yourself picking options without really knowing why. When that happens, either pause to think, or ask AI to break down the tradeoffs before you choose.

------

## 8 Steps

| Step | What Happens | My Role |
| :--- | :--- | :--- |
| 1. Start | AI asks clarifying questions | Answer |
| 2. Define Concepts | AI lists core entities and states | Choose/Confirm |
| 3. Define Page Structure | AI lists pages and navigation | Confirm |
| 4. Detail Each Page | AI draws ASCII mockups, asks element by element | Pick A/B/C |
| 5. Define AI Interface | AI defines input/output JSON | Confirm |
| 6. Define Data Structure | AI outputs table schema | Confirm |
| 7. Export PRD | Say "Update PRD" | Accept |
| 8. Verify Implementation | Feed code to AI, check for discrepancies | Decide |

3-page MVP: ~2 hours, 50-80 choices.

That's my feel. Your mileage may vary. More complex pages, more states, more time. But compared to the cost of "PRD done, then realize dev understood something completely different"—worth it.

------

## Lessons From Hitting Walls

### 1. ASCII Prototypes Beat Figma

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

Why better than Figma?

AI can generate and parse it. Forces coverage of all states. Fast to modify. Figma screenshots AI can only "see," can't "operate on," and easily ignores states not drawn in the screenshot.

### 2. State Enums Beat Natural Language

Lesson learned: I wrote "the plan can start, finish, or be archived," AI generated identical UIs for "start" and "finish."

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

Enum + UI differences + transition conditions = nothing gets missed.

Natural language's problem is ambiguity. Is "start" an action or a state? AI can't tell. Enums force you to spell out what each state looks like in UI.

### 3. JSON Schema for AI Interfaces

Got AI features? Don't use natural language. Define JSON directly:

```json
// Input
{"user_goal": "Learn PyTorch image classification", "time_budget_hours": 20}

// Output
{"path": [{"node": "Tensor Basics", "hours": 2}, {"node": "Autograd", "hours": 3}]}
```

AI copies this directly when writing code. Field names won't mismatch.

Problem with natural language interface descriptions: you say "return the learning path," AI might return an array, might return an object, field name might be path, might be learningPath, might be result. JSON Schema locks it down. Saves fixing later.

### 4. SOP for AI Maintenance

After AI generates code, future edits often go sideways—doesn't know which file to change, doesn't know project conventions.

Add this to PRD:

```
## AI Maintenance Guide
Add page: Copy existing structure → Update routes → Add nav entry
Add state: Update type defs → Update state management → Check all references
```

This section is for AI, not humans. Say "add a new page," AI follows the guide, doesn't break things.

There's a trade-off here: more detailed guide = less AI chaos, but maintaining the guide itself costs time. My approach: only document the operations most likely to go wrong, let AI figure out the rest.

------

## Quick Commands

During PRD process, these speed up the conversation:

| Scenario | You Say |
| :--- | :--- |
| Move forward | Continue |
| Don't want to overthink | You decide / Not important |
| Get document | Update PRD |
| Change decision | Change XX to YY |
| Check for gaps | Review for anything missed |
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

3-page MVP: ~2 hours, 50-80 choices.

------

## Next Up

PRD done. Next step: getting AI to generate code from it.

I'll try three approaches:

- Approach A: Feed entire PRD to Claude/GPT, one-shot generation
- Approach B: Split by page, generate individually, then assemble
- Approach C: Have AI generate project structure/skeleton first, then fill in details

Then compare which produces higher quality, more maintainable code. My bet's on C, but gotta run it to know.

------

*I hit walls in real projects, then write them down. Next one coming.*
