---
name: plan
description: "Plans a change to a codebase"
disable-model-invocation: true
---

## Workflow

- Planning always takes place in a Markdown plan document.
- Always update the plan document as you go.
- Follow the flows described below when applicable, or else converse with the user to create or update a plan.

### Start a new plan

- To start a new plan, you should be given a rough draft or a description of requirements.
- If a plan file is not attached, AskUserQuestion where to create it (e.g. ./foo-plan.md).
- Read files as needed for context. Stop searching/reading if you see diminishing returns.
- Prompt to clarify Requirements if needed. The scope should be clear, but defer detailed questions.
- Write the plan document.
  - Open Questions should be thorough. Breadth (considering various layers/components of the problem/system) is more important than depth (diving into fine details).
  - Leave Tasks empty for now.
- Say "Writing Tasks..."
- Fill in Tasks.
  - Update other sections if appropriate.
- Summarize what you did.

### Resolve an Open Question

At the start of a conversation, if given a plan with Open Questions, jump right into this flow.
Or do this when asked (e.g. "next question").

- Prompt a question
  - Pick the Open Question that seems most impactful to the plan.
  - Call AskUserQuestion with the question and provide possible answers.
  - Include an option to "Research" the question (search the codebase or the web).
- After the user selects an answer,
  - Read relevant files to understand the answer if needed.
  - If the answer seems incompatible with the plan/context, raise this to the user and stop.
  - Update the plan according to the answer.
  - Delete the question.
  - If the response is a question or request for more information, respond accordingly and don't repeat the AskUserQuestion.
- After resolving an open question,
  - Briefly summarize what you just changed in the plan, and say how many questions remain.
  - If the resolved question leads to follow-up questions, then AskUserQuestion including an "Add to Open Questions" option.
  - If there is another open question that is highly relevant to the recent conversation, repeat this flow with that question.
  - Otherwise, stop.

### Fix a Bad Plan

If the plan document is clearly diverged from the guidelines described here, offer to fix it.
You may do this at the start of a conversation or at any natural transition point.

## Plan Document Contents

### General Guidelines

- Use the structure in the Template below.
- If a section is empty, just write "(none)".
- Be succinct.
- Write the plan as a standalone document. Do not allude to the conversation which created it.
- Don't bold things a lot.
- Avoid implementation details unless it is important for correctness or significant to the system design.
- Don't document established patterns.

### Template

```markdown
# [Title]

## Requirements

- ...

## Tasks

- [ ] ...

## Open Questions

- ...?

## Notes

- ...

## References

- `EntityName` (./path/to/entity) - Short description
```

### Section: Requirements

- All of the plan should be in service of Requirements as written.
- Describe the result, not the path to get there.
- Describe system behavior, not implementation.

### Section: Tasks

- A task should have a clear output.
- A task has a checkbox `[ ]`.
- Use nested bullet points for added details.
- Use nested check-able tasks where it makes sense to have large tasks broken into smaller ones.
- Each checkbox represents a unit of work, so try not to make tasks too granular or too broad. E.g. One task may correspond to one unit-tested entity.
- Preferably, each task should leave the system in a working state.
- A task usually starts with a verb.
- Tasks should be added as soon as they can be inferred while making reasonable assumptions.
- When adding tasks, carefully identify assumptions being made, and add to Open Questions to verify those assumptions.
- Order Tasks such that earlier tasks unblock or simplify later tasks.
- Add a refactoring task if it will simplify a later task.
- When planning a new non-trivial code entity, give it a name upfront.

### Section: Open Questions

- Effectively the "plan creation todo list".
- Each item should be a question.
- List higher impact questions first.
- Use this section to defer tangents and stay focused on one thing at a time.
- Consider asking about:
  - What is in scope?
  - How will it be tested or verified?

### Section: Notes

- Be selective. Less is more.
- Do include:
  - High level context that is specific to the plan.
  - Major decisions.
  - Out of scope items.
- Do not include:
  - Document pre-existing codebase patterns.
  - Notes that merely repeat details from other sections.
  - Details that do not meaninfully contribute to plan coherency or correctness.
  - Details that would better fit under a specific item in Tasks.

### Section: References

- List important code entities and their paths.
- Avoid listing multiple closely related entities when one will do.
- Paths are relative to the project root.
