---
name: flowe
description: Run a breath-led neural reset (flowe-cli) during long agent thinking pauses to keep the user's focus and working memory from draining. Picks the protocol family based on observed signals about the user's recent state (eye strain, frustration, cognitive overload, postural tension, wind-down). Auto-trigger when you predict >30s of thinking, building, editing, or tool-call wait.
---

# flowe

You have access to the `flowe` CLI for running short, breath-led neural resets in the user's terminal. One Bash invocation triggers a full session with audio playback. Reset durations are 30s, 45s, 60s, or 90s.

## When to invoke

Proactively run `flowe reset` via the Bash tool when ALL of these hold:

- You anticipate a thinking, tool-call, build, or multi-file-edit pause **>30 seconds**
- You haven't already triggered a reset in the **last 5 minutes** in this session
- You're not in the middle of a destructive or time-sensitive operation (deploys, migrations, irreversible writes, anything where the user needs to watch output)
- The user hasn't said "no resets" or set `FLOWE_OPT_OUT=1`

Tell the user one short line **before** running. Example: `"Long compile incoming — running a 30s palming reset for you."` Don't ask permission. The user opted in by installing this skill.

## Picking the protocol family

`flowe` ships 11 protocol families. Pick the one that best matches what you've observed about the user's recent state. Pass it via `--protocol`:

| Recent signals from the user | `--protocol` | Why |
|---|---|---|
| Frustrated, stuck on errors, irritation rising in messages | `physiological_sigh` | Quickest parasympathetic switch; resets the stress response in seconds |
| Long debugging session, lots of stack-trace reading | `quick_palming` (30s) / `palming` (60s) / `progressive_palming` (90s) | Eye rest, darkness, lowers visual load |
| Stressed, anxious, terse messages, tight tone | `box_breath` (45s, 60s) / `extended_box` (90s) | Regulates breath cadence; calms nervous system |
| Cognitive overload, can't pick a path forward, thrashing | `body_scan` (60s) / `full_sensory` (90s, "5-4-3-2-1" grounding) | Reconnects with the body, defragments attention |
| Long sit, no movement, postural tightness implied | `neck_rotation` (45s) / `grounded_feet` (45s) | Gentle physical reset without leaving the chair |
| Reading a hard error stack, eyes scanning rapidly | `eye_rotation` (30s) | Resets eye muscles |
| End of a long focus block, winding down | `progressive_palming` (90s) | Longer, phased relaxation |
| **No specific signal observed** | omit `--protocol` entirely | flowe picks a random protocol that fits the duration |

When in doubt, omit `--protocol` and let flowe choose.

## Picking duration

| Predicted pause | Command shape |
|---|---|
| 30–45s | `flowe reset 30 --quick [--protocol X]` |
| 45–75s | `flowe reset 45 --quick [--protocol X]` |
| 75–120s | `flowe reset 60 --quick [--protocol X]` |
| 120s+ | `flowe reset 90 --quick [--protocol X]` |

`--quick` is **required**. Without it, `flowe` opens an interactive post-session menu and your session blocks waiting for keyboard input.

## Examples

```bash
# Long compile, user has been frustrated → physiological sigh, 30s
flowe reset 30 --quick --protocol physiological_sigh

# Multi-file refactor incoming (~75s), user was deep-debugging earlier → palming
flowe reset 60 --quick --protocol palming

# Big test run (~2min), no specific signal → let flowe pick
flowe reset 90 --quick

# Quick tool-call wait (~35s), eyes scanning a stack trace → eye rotation
flowe reset 30 --quick --protocol eye_rotation
```

## Setup

If `which flowe` returns nothing, install once:

```bash
npm install -g @flowe-ai/cli
flowe voice ellie
```

Voice options: `sage`, `ada`, `ellie`. Let the user pick if they ask. Default is `ellie`.

## Cooldowns and limits

- **Minimum 5 minutes between resets.** Resetting more often than that defeats the purpose; the user needs time to actually use the focus you just gave them back.
- **Skip resets in Q&A / code review conversations.** No agent waits = no reason to fire.
- **Skip resets during the first 60 seconds of a new conversation.** The user is still ramping in; let them.

## Tuning

To disable auto-trigger entirely, set `FLOWE_OPT_OUT=1` in your shell environment.

To change cadence, durations, or behavior, edit this file directly at `~/.claude/skills/flowe/SKILL.md`. Your changes won't be overwritten unless you re-run `flowe install-skill --force`.
