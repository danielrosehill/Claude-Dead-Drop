---
name: breakout-adopt
description: Pick up a repository that was created as a breakout and start work on it, rather than asking the user what to do. Triggers when a session starts in a repo containing docs/brief.md, when a cross-session message says a brief is waiting, when the user says "you were spawned for this" or "read your brief and start", or when the working directory is a near-empty repo whose only real content is a seed document. Reads the brief, confirms the first step in one line, and begins. Do NOT use in an established repo with its own history and README.
---

# breakout-adopt

You were spawned deliberately. Another agent decided an idea did not belong in the repo it was
working in, created this one, wrote you a brief, and pointed you at it.

**The failure this skill exists to prevent is opening with "what would you like to work on?"** That
question wastes the entire handoff — the answer is already written down in your working directory,
and the user has already moved on to something else.

## 1. Read the brief

`docs/brief.md`, or whatever path the incoming message named. Read it before anything else,
including before looking around the repo — there is almost nothing else here yet, and the brief is
the only source of intent.

It should give you: what the idea is, why it is not part of the origin repo, the first concrete piece
of work, what done looks like for a first pass, and any open questions.

## 2. Do not reintegrate

The brief states why this is not part of the origin repo. Take that as settled. A common and costly
failure is the receiving agent noticing the connection to the origin project and helpfully proposing
to merge the two back together — undoing the decision it was spawned to implement.

You own this repo. Nothing you produce goes back to the originating session.

## 3. Handle open questions without stalling

Open questions in the brief are **not** blockers by default. The originating session recorded them
because they were undecided, not because it wanted you to wait.

- Anything that does not depend on the answer: **do it now.**
- Anything that does: state your assumption in the commit or the doc, and proceed.
- Only stop if proceeding under any assumption would make the work useless or unsafe if wrong.

The user is not sitting waiting for you. Asking a question you could have answered by choosing costs
them a context switch back into a topic they deliberately handed off.

## 4. Confirm in one line, then start

> Picked up `<repo>` — brief read. Starting with `<first step>`. Assuming `<X>` where the brief left
> it open.

Then work. Do not produce a plan document restating the brief; the brief is the plan.

## 5. Reply only when it is worth a context switch

If the originating agent is still reachable, `SendMessage` is available — but the originating session
handed this off precisely so it could stop thinking about it. Message back only for:

- the brief being genuinely unworkable, or contradicting itself;
- discovering the work *does* belong in the origin repo after all, which means the breakout was
  misclassified and the origin session needs to know.

Routine progress is not worth a message. Remember your plain text output is not visible to other
agents — communicating requires the tool call.

## 6. Give the repo a real README

Before you finish the first pass, replace the seed README with one that describes what this repo is,
written for someone who has never heard of the origin project. A breakout that keeps its stub README
is still half-buried.
