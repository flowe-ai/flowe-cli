# flowe

> Neural resets for engineers using AI coding assistants.

Built by a certified clinical hypnotherapist who also ships production code. `flowe` runs short, breath-led resets in your terminal during Claude / Cursor / Aider thinking pauses. Local audio. No network round-trip. Instant start.

## Install

```bash
npm install -g @flowe-ai/cli
```

Requires Node.js ≥ 20. Currently macOS only; Linux and Windows support land after v1 if there's demand.

## Quickstart

```bash
flowe reset           # interactive picker — choose duration + protocol
flowe reset 60        # 60-second reset, random protocol
flowe reset 60 --protocol box_breath   # deterministic — box-breathing only
flowe reset --quick   # skip the picker, use defaults
```

Press `q` or `Ctrl+C` to abandon a session. The TUI returns you to a clean prompt.

## What flowe does

When your agent is "thinking" for 30–120 seconds, your nervous system has two real options: tab out to Twitter (which dumps your working memory of the codebase) or reset. `flowe reset` is the second option, sized for the agent wait window.

Eleven protocols ship with v1:

| Family | Durations | What it is |
|---|---|---|
| `box_breath` | 45s, 60s | Box breathing — inhale, hold, exhale, hold |
| `extended_box` | 90s | Box breathing with longer rounds |
| `physiological_sigh` | 30s | Double inhale through the nose, long exhale |
| `quick_palming` | 30s | Short palming — cover the eyes |
| `palming` | 60s | Standard palming reset |
| `progressive_palming` | 90s | Longer palming with phased relaxation |
| `body_scan` | 60s | Brief somatic check-in |
| `eye_rotation` | 30s | Eye movement reset |
| `neck_rotation` | 45s | Neck mobility break |
| `grounded_feet` | 45s | Pressure-into-ground grounding |
| `full_sensory` | 90s | 5-4-3-2-1 sensory grounding |

Run `flowe protocols` to see the full list.

## Voices

```bash
flowe voice            # show current + available voices
flowe voice sage       # set voice
```

Three voices ship with v1: **`sage`**, **`ada`**, **`ellie`**. Pick the one you want to hear at 3pm. Default is `sage`.

## Login (optional)

```bash
flowe login
```

Pairs your CLI with a flowe.ai account so resets feed your retention dashboard. Anonymous use works fine — events still flow with a stable anonymous ID. Telemetry can be turned off entirely with `FLOWE_OPT_OUT=1` in your shell environment.

## Status

```bash
flowe status   # voice, backend, telemetry, first reset
flowe whoami   # current auth status
```

## Configuration

Config lives at `~/.config/flowe/config.json`. Voice preference, telemetry mode, and the anonymous ID. Login tokens go in the OS keychain via `keytar`, never on disk.

## Privacy

- Anonymous mode (default): events sent with a generated stable ID, no email or device fingerprint.
- Logged-in mode: events tied to your flowe account.
- `FLOWE_OPT_OUT=1` in your shell: events skipped entirely.

## Troubleshooting

**No audio.** macOS uses `afplay`. If `afplay --version` fails, check System Settings → Privacy → Microphone is not blocking the binary. The CLI exits with code `4` on audio device errors.

**`flowe: command not found` after install.** Your npm global bin path isn't on `$PATH`. Run `npm config get prefix` and add `$(npm config get prefix)/bin` to your shell profile.

**Session won't start, "agent_busy".** A previous reset is still running. Wait for it to finish or run `flowe status` to see what's open.

**Login redirects but never completes.** The device-grant flow waits up to 5 minutes. If the browser closes early, run `flowe login` again.

## Exit codes

| Code | Meaning |
|---|---|
| `0` | Success |
| `1` | User error (bad flag, unknown protocol family) |
| `2` | Network error (telemetry, login, macro session) |
| `3` | Auth required or expired |
| `4` | Audio device unavailable |

## License

MIT. See `LICENSE`.

## Repo

Source: [github.com/flowe-ai/flowe-cli](https://github.com/flowe-ai/flowe-cli)
Issues + feature requests: [github.com/flowe-ai/flowe-cli/issues](https://github.com/flowe-ai/flowe-cli/issues)
