---
name: refute
description: Adversarially verify a code change by dispatching fresh sub-agents told the change is broken and to prove it, fixing what they confirm, and looping until a round comes back clean or stops converging. Use when a change is about to be called done, or when the user asks to refute, break, or adversarially test a branch or commit range.
license: MIT
metadata:
  author: cmdctrlai
  version: "1.0.0"
---

# Refute

You are the author's side of the loop. A **fresh** sub-agent does the refuting. Never refute your own change in your own context – you share its blind spots.

## 1. Gather the brief

Collect exactly these, and nothing about how you built it:

- **Requirement** – verbatim, as the user gave it. If it was spread across messages, quote each part.
- **Commit range** – `<base>..<head>` on the branch under test. If the work is uncommitted, ask the user whether to commit it first.
- **Test command** – the command that runs the relevant tests.
- **How to exercise it for real** – a server on a spare port, a test database, real data to replay. Not every environment supports this. If the user hasn't said, ask whether it does and how to proceed.
- **Out of scope** (optional) – known issues tracked elsewhere.

Never commit anything without the user's explicit approval – not the work, not fixes, not the refuter's tests.

If the requirement is missing or ambiguous, ask the user before dispatching. A refuter with the wrong requirement proves the wrong thing.

## 2. Dispatch a fresh refuter

Fill in [the refuter prompt](references/refuter-prompt.md) and hand it to a new sub-agent. Add a few concrete angles to attack that fit this change – concurrency, another user's data, empty or huge input, older data formats, every code path that must agree.

Do **not** include your summary, your reasoning, or a claim that the code is sound. A refuter told it works stops looking.

If your agent cannot spawn sub-agents, tell the user to run the filled-in prompt in a new session and paste the report back.

## 3. Triage the report

A finding counts only with evidence: a failing test, a command plus its output, or a line-numbered path from input to wrong result. Give prose-only findings one round-trip to produce a repro, then drop them.

For each confirmed defect:

- **Inside the requirement** – fix it, and keep the refuter's failing test as the guard.
- **Outside the requirement** – report it to the user; don't fix it in this change.

The refuter's tests are left uncommitted. Name the file and ask the user what to do with each test – keep, skip, move or delete.

If the refuter could not exercise the change live, say what that leaves unproven and ask the user how to proceed.

If you believe a running repro is wrong, ask the user. Don't argue it away.

## 4. Loop

Once the user approves committing the fixes, dispatch a **new** refuter with the same prompt and the updated commit range. Track each round's confirmed defects and their severity.

- **Stop** when a round confirms nothing, or only low-severity defects outside the requirement.
- **Escalate to the user** when it stops converging:
  - a round finds as many defects as the last, or worse ones;
  - the same invariant breaks again in a new form;
  - or five rounds have passed.

Non-convergence usually means the design is wrong, not the code. That is the user's call.

## 5. Hand off

Tell the user, briefly:

- each round's confirmed defects and what fixed them;
- anything escalated or out of scope;
- a short check list of what only a human can verify – numbered rows of **Do / Expect / Proves**. If it runs past about six rows, verify more yourself first.
