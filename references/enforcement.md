# Enforcement

Conventions hold for weeks; enforcement holds forever. This file shows how to make the workspace self-enforcing with Claude Code hooks, so the load order and capture protocol fire without anyone remembering them.

This layer is opt-in. Build the workspace first, work in it for a week or two, then add enforcement for the rules you catch yourself skipping. Adding hooks to a workspace that has not proven out is premature.

## The two hooks that matter

### 1. Session briefing (SessionStart)

Injects the current state of the workspace when a session opens: the "next session briefing" section of a system file, or simply the routing reminder. The agent starts oriented instead of starting cold.

`.claude/settings.json`:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          { "type": "command", "command": "cat system/briefing.md" }
        ]
      }
    ]
  }
}
```

Keep `system/briefing.md` short and current; the capture protocol's session-end write should update it when the next session needs to know something.

### 2. Load-order reminder (UserPromptSubmit)

Injects a compressed version of the load-order contract with every prompt. This is the enforcement that stops "just this once I'll skip loading hot.md":

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "hooks": [
          { "type": "command", "command": "cat system/load-order-reminder.md" }
        ]
      }
    ]
  }
}
```

The reminder file is 5-10 lines: the base load set for any client task, plus the per-task additions. It is a compressed pointer to the full contract, not a copy of it (rule 5: one home per fact; the reminder says "full table in system/context-load-order.md").

## Escalation: from reminder to gate

Reminders inject text; gates block actions. If a convention keeps being violated despite the reminder, a `PreToolUse` hook can check the action and reject it with a message (for example: block writes of files containing a forbidden pattern, or warn when a campaign output is written without the evaluation step having run). Gates are a heavier tool: each one adds friction to every session, so add them one at a time, for rules with a track record of being skipped, and make the rejection message say exactly how to comply.

## What not to enforce

- **Do not gate the capture protocol.** Capture is agent-judgment work (which client, which files, what matters); a hook cannot do it, only remind about it. Put the capture instruction in the workspace's root `CLAUDE.md` as a standing instruction instead.
- **Do not inject large files.** Hook output lands in every context. A hook that injects a 200-line file taxes every prompt in the session; inject pointers and summaries, keep payloads on shelves.
- **Do not enforce what you have not first done manually for two weeks.** Hooks encode proven habits. Encoding an aspiration produces a workspace that nags.

## Beyond hooks

The same pattern works with any agent runtime that supports startup context or per-message injection: a system-prompt preamble, a scheduled job that rebuilds a briefing file, a git pre-commit hook that rejects a client fact landing outside its folder. The principle is constant: the workspace should not depend on anyone's memory, including the agent's.
