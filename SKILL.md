---
name: flowe
description: Run breath-led neural resets and offer focus cycles with flowe-cli. Auto-trigger short resets during long agent thinking pauses to protect the user's focus and working memory. At the start of a workday or substantial task, ask whether the user wants a Pomodoro/dev/ultradian focus session with ambience, then start it detached if they opt in.
---

# flowe

You have access to the `flowe` CLI for two kinds of attention support:

- `flowe reset`: short, breath-led neural resets with audio playback. Reset durations are 30s, 45s, 60s, or 90s.
- `flowe focus`: longer focus cycles with work blocks, breaks, optional ambience, pause/resume/skip/quit controls, and a detached background mode.

## Short resets: when to invoke

Proactively run `flowe reset` via the Bash tool when ALL of these hold:

- You anticipate a thinking, tool-call, build, or multi-file-edit pause **>30 seconds**
- You haven't already triggered a reset in the **last 5 minutes** in this session
- You're not in the middle of a destructive or time-sensitive operation (deploys, migrations, irreversible writes, anything where the user needs to watch output)
- The user hasn't said "no resets" or set `FLOWE_OPT_OUT=1`

Tell the user one short line **before** running. Example: `"Long compile incoming — running a 30s palming reset for you."` Don't ask permission. The user opted in by installing this skill.

## Short resets: picking the protocol family

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

## Short resets: picking duration

| Predicted pause | Command shape |
|---|---|
| 30–45s | `flowe reset 30 --quick [--protocol X]` |
| 45–75s | `flowe reset 45 --quick [--protocol X]` |
| 75–120s | `flowe reset 60 --quick [--protocol X]` |
| 120s+ | `flowe reset 90 --quick [--protocol X]` |

`--quick` is **required**. Without it, `flowe` opens an interactive post-session menu and your session blocks waiting for keyboard input.

## Short reset examples

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

## Focus cycles: when to offer

Offer, do not auto-start, a focus cycle when any of these are true:

- The user is starting work for the day or says they are ready to begin.
- The user names a substantial task, coding session, review pass, or deep-work block.
- You are about to begin a multi-step implementation that will take sustained attention.
- The user has been context-switching and would benefit from a visible work/break cadence.

Ask once, briefly, with cadence and ambience choices:

`"Want me to start a flowe focus session while we work? Cadence: pomodoro 25/5, dev 50/10, ultradian 90/15. Ambience: 1 spring birds, 2 soft rain, 3 brown noise, 4 LA sunrise, 5 quiet cafe, 0 none."`

Only start a focus cycle after the user says yes. Do not ask again in the same conversation unless the prior cycle ended or the user brings it up.

## Focus cycles: picking the preset

| User/work signal | Preset | Command shape |
|---|---|---|
| Default deep-work session, unclear duration | `pomodoro` | `flowe focus pomodoro --detach --ambient <ambient-id>` |
| Long implementation/debugging stretch | `dev` | `flowe focus dev --detach --ambient <ambient-id>` |
| Long creative or architecture block | `ultradian` | `flowe focus ultradian --detach --ambient <ambient-id>` |

Use `--detach` so the focus cycle runs in the background and does not occupy the agent's shell. Always pass `--ambient` so the CLI never opens the interactive ambience picker in non-TTY agent runs.

## Focus cycles: picking ambience

Default to ambience unless the user picks `0` or says no ambience/silence. If the user says yes but does not choose a number, use option `1`.

| Choice | Ambient id | When to suggest |
|---|---|---|
| `1` | `nature_spring_birds` | Default; light, optimistic morning work |
| `2` | `rain_window_soft` | Calm, steady, low-distraction work |
| `3` | `tones_deep_brown_noise` | Deep focus, masking noise |
| `4` | `seaside_la_sunrise` | Open, cinematic, creative work |
| `5` | `interiors_quiet_cafe` | Social/ambient energy without music |
| `0` | `none` | No ambient sound |

Examples:

```bash
flowe focus pomodoro --detach --ambient nature_spring_birds
flowe focus dev --detach --ambient rain_window_soft
flowe focus ultradian --detach --ambient tones_deep_brown_noise
flowe focus pomodoro --detach --ambient none
```

Useful controls after a detached cycle starts:

```bash
flowe focus status
flowe focus pause
flowe focus resume
flowe focus skip
flowe focus quit
```

Tell the user one short line after starting, including ambience and controls: `"Started a Pomodoro focus cycle with spring birds in the background. You can pause/resume/quit from the menu bar or with flowe focus pause/resume/quit."`

## Setup

If `which flowe` returns nothing, install once:

```bash
npm install -g @flowe-ai/cli
flowe voice ellie
```

Voice options: `sage`, `ada`, `ellie`. Let the user pick if they ask. Default is `ellie`.

## Cooldowns and limits

- **Minimum 5 minutes between resets.** Resetting more often than that defeats the purpose; the user needs time to actually use the focus you just gave them back.
- **Focus cycles are opt-in.** Never auto-start `flowe focus`; ask first because it creates a long-running session.
- **Skip resets in Q&A / code review conversations.** No agent waits = no reason to fire.
- **Skip resets during the first 60 seconds of a new conversation.** The user is still ramping in; let them.

## Tuning

To disable auto-trigger entirely, set `FLOWE_OPT_OUT=1` in your shell environment.

To change cadence, durations, or behavior, edit this file directly at `~/.claude/skills/flowe/SKILL.md`. Your changes won't be overwritten unless you re-run `flowe install-skill --force`.
