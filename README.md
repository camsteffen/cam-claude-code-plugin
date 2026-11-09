# `/cam:plan`

A Claude skill for planning complex changes.

## When to use

- You need to work out the technical design of a complex feature.
- You need to break down a complex task into smaller tasks.
- You want an agent to guide you in thinking through all aspects of a multi-faceted plan.
- You want an opportunity the correct the agent's bad ideas before it executes them.
- You need a plan document to feed to an agent.
- You need a plan document to share with a co-worker.

## When NOT to use

- Your task is simple enough to be planned in one quick agent conversation (perhaps using `/plan`).
- You want the agent to make assumptions and execute with minimal input.

## "The Plan Document"

To use this skill, you first need to understand The Plan Document.
For every plan, the skill creates and maintains a Markdown file with these sections:

| Section        | Purpose                                              |
|----------------|------------------------------------------------------|
| Requirements   | The desired outcome of the plan                      |
| Tasks          | Actionable list of steps (the actual plan)           |
| Open Questions | Questions to be answered before the plan is complete |
| Notes          | Major decisions, context, out-of-scope items, etc.   |
| References     | Relevant code entities, file paths                   |

**How this DOES NOT work:** _Create a plan. Write it down. Done._

**How this DOES work:** _Write a plan that is wrong and/or incomplete. Fix the plan one thing at a time. Done._

Think of "Open Questions" as a planning progress indicator - it shows how much refinement you should do before you have a completed plan.
Questions are deleted from the list as they are answered,
and when there are no questions left, the plan is complete!
At the start of planning, the Open Questions is the most important section since it captures the areas that need to be explored.
As those questions are answered, the "Tasks" section becomes more complete and trustworthy.

Note that the skill tries to order _more impactful_ questions _higher_ in the list.
The questions toward the bottom are more likely to be "good as-is, just delete".

The "Tasks" section may _look_ complete even when there are lots of open questions.
This represents _the best plan we have so far_.
But beware, it may contain bad assumptions that the agent made,
and those assumptions are meant to be addressed by items under "Open Questions".
That said, you might decide at any moment to take the Tasks section as-is and run with it.
I won't stop you.

The skill takes responsibility for keeping the document up-to-date as you progress towards a finished plan.
So you should never need to edit the document manually (of course you can if you want to).

**It's a good idea to open the document and watch it evolve as you use the skill.**

### The Plan Document as Context

The document is designed be a complete source of context for the agent at every point in the planning process.
So you can always start a new conversation and pick up where you left off with `/cam:plan @plan.md`.
This is a good way to save tokens, especially after having a long and winding conversation to arrive at some decision.

## Using the skill to create a plan

### Start a plan

To get started, invoke `/cam:plan` and provide a description of your requirements.
If you have a ticket, paste the description in the prompt.
Or, consider writing requirements to a file, then attach it with `@`.
Don't worry if some details are missing - the skill will help fill the gaps.

```
/cam:plan Add a Reset button the the form page.
OR
/cam:plan @reset-button-notes.txt
```

The agent will work with you to write the first draft of the Plan Document (see the description above).
You can specify a filename and location to suit your preference.
Or it will suggest one for you, like `./reset-button-plan.md`.

Then it will do some (light) research before writing the document with little or no further input.
The aim here is to get a reasonable first draft quickly, making assumptions as needed.
But it will also write questions under Open Questions so that you can verify its assumptions.
It will also try to include every question that you should consider before arriving at a complete plan,
considering all aspects of the given problem space and the system being worked on.

### Resolve Open Questions

After creating the initial draft, you should refine the plan by working through items in the "Open Questions" section. The process goes like this:

- Pick a question from Open Questions.
- Decide on an answer to the question.
- Update the plan document according to the answer.
  - Sometimes answering one question reveals more questions! 😩
- Delete the answered question from the list.
- Repeat.

**But of course, the skill can do most of that for you. See "Interview style" below!**

### Interview style

The skill includes instructions for conducting an "interview style" workflow to iterate on the plan,
resolving the "Open Questions" one at a time.

Interview style can be started in a new session.
Just invoke the skill and attach the in-progress plan document with `@`.
The skill assumes interview style by default.

```
/cam:plan @my-plan.md
```

Or, to start interview style in a conversation where you've already invoked the skill,
just say "next question".

It will immediately prompt you with a question taken from the "Open Questions" section of the plan.

- It selects the question that seems most impactful.
- It provides possible answers for you to select from, often with one that is "Recommended".
- It gives an option to "Research" the question for you.
- You can also type an answer or ask for clarification.

After you select an answer, it will:

- Read relevant files to understand your decision with more complete context.
- Update all sections of the plan document with respect to your answer.
- Delete the question from Open Questions.

If you opted to research the question, or replied in some other way,
then it falls back to conversational style where you can discuss or research as needed.
Then you can give an answer to the question when you're ready,
and the plan document will be updated.

After answering the question, it might ask you some follow-on questions.

Then it will give a summary and stop, meaning you've completed one iteration on your plan!

Since the plan document should capture all context needed moving forward,
you may choose to close the session at this point.

_Repeat this flow until Open Questions is empty._

> Tip: Launch interview style using this command in your terminal so that it's easy to re-run for every question:
> 
> ```
> claude "/cam:plan @my-plan.md"
> ```

### Conversational style

While the interview style often works well,
you should not feel constrained to use it all the time.
Sometimes it's better to just tell the agent about something you want to change in the plan.

Invoke the skill, attach the plan, and say any instructions or questions you want.
You can continue to converse with the agent,
and the skill will ensure that any decided changes to the plan are reflected in the document.

```
/cam:plan @my-plan.md The Reset button should be disabled when the form is empty.
```

## I have a plan. Now what?

That is completely up to you! The skill is agnostic of what strategy (or skill) you choose to implement the plan.
Although the plan document strives to be both human-readable and agent-friendly.
You could feed the plan to an agent and tell it to implement the whole thing.
Or you could work through the Tasks yourself, possibly using an agent's help along the way.
It depends on your goals, preferences, budget, risk aversion, etc.

Let me know how it goes!
