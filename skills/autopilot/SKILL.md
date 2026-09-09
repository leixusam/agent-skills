---
name: autopilot
description: Use only when the user explicitly asks for an autonomous work session because they are stepping away for hours. Establishes a verifiable goal, runs independently within approved boundaries, and keeps an append-only backbone document as durable state across context compactions.
---

# Autopilot

The user is stepping away for hours or longer. Set up an autonomous run that survives context compactions, makes reversible decisions independently, and writes all material state to a backbone Markdown document on disk. The document is the source of truth; chat context is not.

Autopilot is an asynchronous supervision mode for an already chosen stage such as implementation, evaluation, research, or polish. It does not expand the authority the user granted.

*Drawn from:* novel — the persistent append-only backbone document as durable state across compactions, combined with explicit goal and evaluation gates for unattended agent work.

## Phase 1: Define the contract while the user is present

Resolve these before starting:

1. **Goal** — a verifiable end state. “Fix the dashboard” is not sufficient. “The sessions table loads in under 500 ms on a 10,000-row dataset, verified in a real browser” is.
2. **Evaluation criteria** — concrete thresholds that prove completion: test suites, accuracy, rubric scores, latency, screenshot comparison, or another measurable result.
3. **Scope boundaries** — what may and may not be changed.
4. **Resource ceilings** — require a numeric time ceiling. If paid APIs or delegated workers are available, also require a numeric cost, token, or call budget plus maximum iterations and concurrent workers.
5. **Verification plan** — user-level, visual, integration, security, or performance checks required by the work.
6. **Authority boundaries** — external messages, deployments, purchases, destructive actions, and other irreversible steps remain prohibited unless separately authorized.

Ask one concise follow-up at a time until the contract is crisp. Recommend stronger criteria when the proposed goal is subjective or untestable. Do not proceed until the user explicitly approves the goal and evaluation criteria.

## Phase 2: Choose the durable docs location

Follow the repository's documented convention. If none exists:

- use `thoughts/shared/autopilot/` when the repository already uses `thoughts/shared/`;
- otherwise use `docs/autopilot/`.

Create the directory if needed. Name the backbone document:

```text
YYYYMMDD-HHMMSSZ-<slug>.md
```

Use UTC and a descriptive slug.

## Phase 3: Write the backbone document

After the initial contract is written, treat the body as append-only: never erase prior discoveries or decisions. Add corrections as new timestamped entries. Frontmatter fields such as `status` and `last_update` may be refreshed in place.

```markdown
---
started: <ISO timestamp>
mode: autopilot
status: planning | running | blocked | complete | abandoned
last_update: <ISO timestamp>
worktree: <absolute path>
branch: <feature branch>
goal: <exact approved goal>
---

# Autopilot Backbone — <topic>

## Goal
<Verifiable end state, verbatim from the approved contract.>

## Evaluation criteria
- <criterion with concrete threshold>

## Scope boundaries
- In scope: <...>
- Out of scope: <...>

## Authority boundaries
- Allowed: <...>
- Requires user approval: <...>

## Ceiling
- Time: <numeric duration>
- Paid usage: <numeric cost, token, or call budget; not applicable only when no paid runtime is available>
- Maximum iterations: <number>
- Maximum concurrent workers: <number>

## Verification plan
<How the result will be verified, or “not applicable.”>

---
## Learnings
### <ISO timestamp> — <summary>
<What was learned, why it matters, and supporting path or evidence.>

---
## Decisions
### <ISO timestamp> — <title>
- **Chose:** <...>
- **Over:** <...>
- **Why:** <...>
- **Reversibility:** <...>
- **Confidence:** low | medium | high

---
## Open questions for the user
1. <Question, tentative direction, and what work can continue without the answer.>

---
## Trials and dead ends
### <ISO timestamp> — <approach>
- **Tried:** <...>
- **Failed because:** <failure mode and root cause>
- **Do not retry unless:** <condition>

---
## Files touched
- `path/file.ext:line` — <change and timestamp>

---
## Evaluation status
| Criterion | Target | Current | Status | Last checked |
|---|---:|---:|---|---|
| <criterion> | <target> | <result> | PASS / FAIL / not run | <timestamp> |

---
## Summary
<At completion: outcome, evidence, decisions, remaining risks, and what the user should inspect first.>
```

Read the file back after creating it to catch missing or malformed state.

## Phase 4: Activate persistent execution

If the runtime provides a persistent goal or autonomous-run mechanism, ask the user to activate it with the exact approved goal and backbone path. For example:

```text
Run autonomously until <verifiable end state> is true. Otherwise iterate. The backbone is at <path>; update it after every material learning, decision, trial, and evaluation.
```

If the runtime has no separate goal mechanism, the user's explicit approval of the contract starts the run. Continue using the backbone as the goal ledger.

## Phase 5: Run autonomously

Once activated:

- **Do not block on ordinary questions.** Choose the safest reversible default, record the decision, and continue.
- **Do not cross authority boundaries.** A long-running instruction does not authorize publishing, deploying, purchasing, deleting data, contacting people, or mutating production.
- **Defer only the blocked subtask.** Continue all independent work rather than halting the run because one question remains.
- **Re-evaluate after every iteration.** Run the approved checks and update the evaluation table with measured results.
- **Keep the backbone current.** Record material learnings, decisions, dead ends, files changed, evidence, and `last_update` as work happens—not at the end.
- **Use isolated workers when available.** Delegate briefable research, implementation, or review tasks, then verify their claims before relying on them. Give workers the relevant contract and backbone path.
- **Commit incrementally when appropriate.** Work on a feature branch or worktree, never directly on a protected default branch. Reference the backbone document in commit messages or descriptions.
- **Sync durable state only when authorized.** If the user has already approved the destination and content boundary for a notes or thought-sync mechanism, run it after major checkpoints. Otherwise keep the backbone local; never upload paths, evidence, or repository context merely because a sync tool exists.

## Phase 6: Completion gate

Declare completion only when all applicable conditions are true:

1. Every approved evaluation criterion passes with fresh evidence.
2. User-level and visual verification are complete where applicable.
3. When the runtime supports isolated workers, an independent fresh-eyes review has no unresolved blocking findings. Use the environment's review skill when available; otherwise brief a context-isolated reviewer with the artifact, goal, criteria, and evidence. In a single-agent runtime, perform a deliberate cold re-read against the approved criteria and disclose in the summary that independent review was unavailable.
4. Every completion claim is supported by evidence produced during the current run.
5. The backbone summary is complete.
6. Frontmatter `status` is `complete`.
7. The open-questions section is empty, or every remaining question includes a tentative direction and does not invalidate the completion claim.

If a check fails, record it and iterate. Do not lower the approved threshold merely because the result is close.

## Phase 7: Hand back

The returning user should be able to read only the backbone document and understand:

- whether the goal was met;
- the exact evaluation evidence;
- what changed;
- important discoveries and failed approaches;
- reversible decisions made in their absence;
- unresolved risks or questions;
- what to inspect first.

If the backbone does not communicate all of that, the run is not ready for handback.

## Hard rules

- The backbone document is the source of truth.
- Stay inside the approved scope and authority boundary.
- Never work directly on a protected default branch.
- Never perform an irreversible action without explicit prior authorization.
- Never block the unattended run for a reversible decision.
- Never mark the run complete without meeting the approved evaluation criteria.
- Enforce the approved time, usage, iteration, and concurrency ceilings; do not silently raise them.
- Stop at the time or cost ceiling if the run has not converged. Record the current state and the recommended next move.
- If genuinely stuck with no useful work remaining, set `status: blocked`, document the blocker and recommended resolution, then stop instead of thrashing.

## Anti-rationalization

- “I will write the document at the end.” → Context may disappear first. Update it continuously.
- “The user would obviously approve this irreversible step.” → They did not. Record it and stop at the boundary.
- “The result is close enough.” → Completion requires the approved number, not a feeling.
- “The reviewer is optional because the user is away.” → Unattended work requires stricter independent review.
- “One question blocks everything.” → Defer that subtask and continue independent work.
- “More retries might eventually work.” → Record the root cause and stop when the ceiling or convergence rule says to stop.
