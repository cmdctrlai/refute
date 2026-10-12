# Refute

> This change is broken. Prove it.

An [Agent Skill](https://agentskills.io) that verifies a code change by telling a fresh sub-agent it's broken and to prove it. Your main agent fixes what the refuter confirms, sends a new refuter, and repeats until a round comes back clean – or escalates to you when it stops converging.

## Install

**Claude Code**

```
/plugin marketplace add cmdctrlai/refute
/plugin install refute@cmdctrl
```

**Any agent, via the [skills CLI](https://github.com/vercel-labs/skills)** – Codex, Cursor, Gemini CLI, GitHub Copilot, OpenCode and more

```
npx skills add cmdctrlai/refute
```

**Gemini CLI**

```
gemini skills install https://github.com/cmdctrlai/refute.git --path skills/refute
```

**By hand** – copy `skills/refute/` into your agent's skills directory:

| Agent | Directory |
|---|---|
| Claude Code | `~/.claude/skills/` or `.claude/skills/` |
| Codex, Cursor, Gemini CLI | `~/.agents/skills/` or `.agents/skills/` |

## Use

Ask your agent to refute the change, or invoke it directly – `/refute` in Claude Code (`/refute:refute` when installed as a plugin) and Cursor, `$refute` in Codex.

Give it the requirement verbatim. It needs the requirement, the commit range and the test command – never the author's summary.

## The loop

1. Fix every confirmed defect. Keep the refuter's failing test.
2. Dispatch a **new** refuter with the same prompt and the new commit range.
3. Stop when a round confirms nothing, or only low-severity defects outside the requirement.
4. Escalate to a human when it stops converging: a round finds as many defects as the last, or worse ones; the same invariant breaks again in a new form; or five rounds have passed.

Just want the prompt? It's in [`skills/refute/references/refuter-prompt.md`](skills/refute/references/refuter-prompt.md).

## License

MIT
