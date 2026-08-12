# Should this be part of Claude-Hopper?

An open question, recorded honestly rather than settled by whoever wrote the second repo.

## The overlap is real

[Claude-Hopper](https://github.com/danielrosehill/Claude-Hopper) already ships:

- **`sideclaude`** — "capture a tangent as a plan file and spawn a parallel Claude session to execute
  it, keeping the current thread clean."
- **`spawn-planning-repo`** — "create a sibling planning repo, seed it with context."
- **`new-claude-at`** — open a new Konsole window running a fresh Claude session at a given path.

Read together, that is: notice a tangent, write it to a file, make a sibling repo, spawn a session
there. Which is most of the breakout mechanism, written before breakout was named.

Anyone proposing a new plugin here owes an account of what is left over. Here it is.

## What is actually new

**1. The push layer.** Hopper predates cross-session `SendMessage` (shipped 7 August 2026, after
Hopper was written). `sideclaude` spawns a session and seeds a file — the new agent must be told
where to look, and the originating agent has no channel to it afterwards. It is a fire-and-forget
pull model. Breakout can address the spawned agent by name, confirm it is reachable, and follow up.
That is a different topology, not a refinement.

**2. The decision rule.** Hopper's skills are mechanisms; they execute once you have decided. None
of them answers *when does an idea deserve its own repo rather than a task in this one* — which is
the part that actually gets got wrong, and the part that connects to interrupt's `out-of-scope`
class.

**3. The receiving side.** Nothing in the prior art gives the spawned agent a procedure. An agent
that wakes up in a near-empty repo asks "what would you like to work on?" and the handoff is wasted.

**4. The brief format.** `sideclaude` writes "a plan file"; `spawn-planning-repo` seeds "context".
Neither specifies what has to be in it for an agent with no memory of the originating conversation
to start work without guessing.

## The argument for folding it into Hopper

- Hopper is already the session-spawning plugin. A second plugin that spawns sessions splits a
  concern that was deliberately consolidated — Hopper itself was carved out of Claude-Rudder for
  exactly this reason.
- A user who installs Breakout without Hopper gets skills that reference spawn primitives they do
  not have.
- `sideclaude` and `breakout` will read as near-duplicates in a skill listing, and the model has to
  pick between them at dispatch time. Overlapping trigger descriptions are a practical problem, not
  an aesthetic one.

## The argument for keeping it separate

- The bulk of this repo is *documentation of a pattern*, not code. Hopper's README is a skill index;
  it is not the natural home for a written argument about when to split a project.
- Breakout composes with `interrupt` at least as tightly as with Hopper — it is the continuation of
  interrupt's `out-of-scope` route. It does not sit cleanly under either parent.
- One idea, one repo. The doctrine this pattern exists to serve applies to the pattern itself.

## Current position

Kept separate, for now, on the third argument — with the spawn step explicitly delegated to Hopper
rather than reimplemented, so the two do not drift into rival implementations of the same thing.

If `sideclaude` grows a `SendMessage` handoff, this repo should probably become documentation only,
and the skills should move. That is the condition to watch for.
