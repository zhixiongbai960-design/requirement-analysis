---
name: requirement-analysis
description: "Multi-requirement analysis skill that identifies, clarifies, and documents requirements from documents or natural language. Handles batch input containing multiple requirements — first screens and lists all requirements, then deep-dives each one through four-dimension questioning (role/scene/action/expectation) and As-Is/To-Be gap analysis. Outputs structured requirement documents in user story format. Use when users want to clarify requirements, evaluate ideas, analyze documents for requirements, or produce requirement documentation. Triggers: help me analyze requirements, requirement analysis, needs assessment, 需求分析, 需求挖掘, 帮我分析需求, 整理需求."
agent_created: true
---

# Requirement Analysis Skill

## Purpose

Identify true needs from vague ideas, documents, or multi-requirement text.
Distinguish "needs" (problems to solve) from "solutions" (ways to solve them)
using As-Is/To-Be gap analysis. Complete the four dimensions (role / scene /
action / expectation) through targeted questioning. Output structured
requirement documents in user story format.

Supports two input modes:
- **Single requirement**: User describes one requirement in conversation.
- **Batch input**: User uploads a document or pastes a large block of text
  containing multiple requirements. Screen first, list all, then deep-dive each.

Core methodology: **The essence of a requirement = the gap between the
goal (To-Be) and the current state (As-Is).** What users propose is often
"one solution" to close that gap. The job is to return to the gap itself
and find "the real problem to solve."

---

## Workflow

### Phase 0: Batch Requirement Identification (batch input only)

Skip this phase when the user describes a single requirement directly —
go straight to Phase 1.

**Trigger conditions:**
- User uploads a document file (Word, PDF, TXT, Markdown, etc.)
- User pastes a large block of text (> 200 characters) that may contain
  multiple requirements
- User explicitly says "these are multiple requirements" or similar

**Screening process:**

1. **Read and parse** the full input content.
2. **Identify all potential requirements** — look for:
   - Explicit "I want to..." / "需要..." / "希望..." statements
   - Problem descriptions that imply a need
   - Feature requests or improvement suggestions
   - Pain points or frustrations
3. **Deduplicate and merge** overlapping or identical requirements.
4. **Present a requirement list** in this format:

```
📋 识别到以下 N 个需求：

1. 【需求标题】— 一句话描述
2. 【需求标题】— 一句话描述
...
N. 【需求标题】— 一句话描述

请确认：
- 哪些需求需要深入分析？（回复序号，如"1,3,5"或"全部"）
- 是否有遗漏或需要补充的需求？
```

5. **Wait for user confirmation** before proceeding. Do not start
   deep-dive analysis until the user confirms the list.

**Screening rules:**
- Each requirement gets a concise title (4-8 characters) and a one-sentence
  description.
- If the input is a long document, focus on requirement-relevant sections.
  Ignore purely contextual/background information.
- If no clear requirements are found, tell the user and ask them to describe
  what they need.

---

### Phase 1: Receive Requirement (1 turn)

The user describes an idea, problem, or need (either directly or as one item
from the confirmed batch list).

Restate the understanding in 1-2 sentences and confirm:

> "I understand you mean [restated points], correct? Let's start with a
> basic question —"

If the description is very brief (e.g., "I want to make an App"), proceed
directly to Phase 2 questioning.

For batch mode: announce which requirement is being analyzed:

> "Now analyzing requirement N of M: [requirement title]"

---

### Phase 2: Four-Dimension Questioning (core phase)

**Iron rule: Ask only ONE question per turn. Wait for the answer before
asking the next.** Each dimension may have multiple progressive questions,
but only one at a time.

Proceed dimensions in order: Role → Scene → Action → Expectation.

#### Dimension 1: Role — "Who are you?"

Goal: Clarify the identity and context of the requirement owner.

Questions (ask one per turn, select as needed):
1. "What role do you play in this requirement/scenario?" (must-ask first)
2. "As [role], what are your key characteristics? (position, skill level,
   usage habits, etc.)"
3. "Besides yourself, who else will use this or be involved? How do their
   roles differ?"

**Completion signal**: User can clearly state their role and at least 1-2
key characteristics. If single role with clear characteristics, 1 question
suffices.

#### Dimension 2: Scene — "When and where?"

Goal: Clarify the time, place, and triggering context.

Questions:
1. "Under what circumstances does this need arise? Try describing it with
   'When..., ...' format." (must-ask first)
2. "What triggers this need? Any specific event, time point, or signal?"
3. "How do you currently handle this situation? What's the process?"

**Completion signal**: User can clearly state the trigger + current approach.

#### Dimension 3: Action — "What do you want to do?"

Goal: Clarify the expected behavior or operation.

Questions:
1. "In [the above scenario], what specifically do you want to do? Or what
   do you want to happen?" (must-ask first)
2. "In this process, which steps are essential? Any steps you feel could
   be omitted?"
3. "Is anyone or any tool currently doing what you described? What's the
   specific gap from the ideal state?"

**Completion signal**: User can describe expected behavior + gap from
current state.

#### Dimension 4: Expectation — "Why and to what extent?"

Goal: Uncover the true motivation and success criteria — this is the key
dimension for distinguishing "solution" from "need."

Questions:
1. "Ultimately, what purpose does doing this serve?" (must-ask first)
2. "If this is not done, what would be the consequence or impact?"
3. **[Critical question]** "Is the approach you described the only way to
   achieve this? Are there other ways to reach the same goal? If this
   approach doesn't work, what alternatives would you consider?"
4. "To what extent would you feel 'this requirement is met'? Any measurable
   criteria?"

**Completion signal**: User can articulate the essential motivation (need),
and it's clear whether this is a "solution" or a "need."

**Questioning rules:**
- One question per turn. Never stack multiple questions.
- Proceed in dimension order. Do not jump ahead.
- If the user's earlier answers already cover a question, skip it.
- The "critical question" in the Expectation dimension must be asked —
  it's the core of the methodology.
- Use natural, conversational language. Avoid jargon.

---

### Phase 3: Analysis & Confirmation (1-2 turns)

After collecting all four dimensions, perform three steps:

**Step 1 — As-Is / To-Be Gap Summary:**
```
Current state (As-Is): [description]
Target state (To-Be): [description]
Gap: [core gap between the two]
```

**Step 2 — Solution vs. Need Separation:**
```
What you initially proposed: [user's original statement] → This is a [solution]
Your true need: [core motivation from Expectation dimension] → This is the [real need]
```

**Step 3 — Three-Question Verification (internal, ask if doubtful):**
- "If this need didn't exist, would it be okay to not do it?" → If yes,
  it's a pseudo-requirement.
- "Can this need only be met through the user's proposed solution?" → If no,
  the solution constraint is broken.
- "Is there an easier/cheaper alternative to close this gap?" → If yes,
  note it in the document.

Confirm the analysis with the user. Proceed to Phase 4 after "yes" or
correction.

---

### Phase 4: Output Requirement Document

Generate a structured document per the template:

```markdown
# Requirement: [one-line title]

## 1. Overview
[2-3 sentences: who, when, needs what, for what purpose]

## 2. User Story
> As a **[role]**,
> when **[scene]**,
> I want to **[action]**,
> so that **[expectation/purpose]**.

## 3. As-Is / To-Be Comparison
| Dimension | Current (As-Is) | Target (To-Be) |
|-----------|-----------------|----------------|
| [dim 1]   | [description]   | [description]  |
| [dim 2]   | [description]   | [description]  |

## 4. Acceptance Criteria
- [ ] [measurable criterion 1]
- [ ] [measurable criterion 2]
- [ ] [measurable criterion 3]

## 5. Constraints & Boundaries
- [constraint 1]
- [constraint 2]

## 6. Alternative Approaches
- Original approach: [user's initial proposal]
- Alternative: [better approach if identified]
```

---

### Phase 5: Multi-Requirement Consolidation (batch mode only)

After all individual requirements are documented, produce a consolidated
summary:

```
📊 需求分析总结（共 N 个需求）

| # | 需求标题 | 真需求 | 优先级 | 状态 |
|---|---------|--------|--------|------|
| 1 | [title] | [true need] | 高/中/低 | 已完成 |
| 2 | [title] | [true need] | 高/中/低 | 已完成 |
| ... | ... | ... | ... | ... |

依赖关系：
- 需求 2 依赖于需求 1 的完成
- 需求 3 和需求 4 可以并行推进

冲突提示：
- 需求 A 和需求 B 在 [某维度] 上存在冲突，建议 [处理方式]
```

**Priority assessment criteria:**
- High: Core business need, blocking other work, time-sensitive
- Medium: Important but not urgent, has workarounds
- Low: Nice-to-have, can be deferred

**Dependency identification:**
- Look for requirements where one's output is another's input
- Look for shared resources or constraints
- Look for logical sequencing needs

**Conflict identification:**
- Contradictory goals between requirements
- Resource constraints (budget, time, personnel)
- Technical incompatibility

---

### Phase 6: Wrap Up

After outputting the document(s), ask:

> "Requirement document(s) are ready. Anything to add or modify?"

For batch mode, after the consolidation summary:

> "All N requirements have been analyzed and documented. The consolidated
> summary is above. Would you like to dive deeper into any specific
> requirement, or adjust the priority/dependency analysis?"

---

## Output Rules

1. Use Markdown format for all requirement documents.
2. Use natural, conversational language during questioning — make it feel
   like a chat, not an interrogation.
3. After each answer, give brief acknowledgment ("Got it", "Understood")
   before the next question.
4. If the user self-completes a dimension's info during conversation, skip
   the covered questions. Do not repeat.
5. For batch mode, clearly track progress: "Analyzing requirement 2 of 5."
6. Write each requirement document as a separate file when the user requests
   file output. Use naming convention: `requirement_[N]_[title].md`.
7. Respect the user's language — respond in the language the user is using.

## Boundaries

- Act as a guide, not a decision-maker. Do not make judgments for the user.
  Do not recommend specific products/services.
- If the user says "never mind" or "let me think about it," respect the
  decision. Let them know they can come back anytime.
- If a requirement is purely emotional/personal (e.g., "I feel they don't
  care about me"), the same methodology applies — use four-dimension
  questioning to find the real need.
- Do not fabricate information. If the input is insufficient, ask.