# flowe-cli — issues & feedback

This repo is the **issue tracker and community surface** for [`@flowe-ai/cli`](https://www.npmjs.com/package/@flowe-ai/cli), the neural reset CLI for engineers using AI coding assistants.

> Built by a certified clinical hypnotherapist who also ships production code. `flowe` runs short, breath-led resets in your terminal during Claude / Cursor / Aider thinking pauses. Local audio. No network round-trip. Instant start.

This repo doesn't host the code. Use it to report bugs, request features, vote on platform support, or follow releases.

## Install

```bash
npm install -g @flowe-ai/cli
```

Requires Node.js ≥ 20. Currently macOS only.

Full install instructions: **[cli.flowe.ai/docs/install](https://cli.flowe.ai/docs/install/)**

## Use with Claude Code

`flowe` ships a Claude Code skill that fires resets for you during long thinking pauses. Claude picks the protocol based on what it's observed about your recent state — physiological sigh when you're frustrated, palming when your eyes are tired, body scan when you're overloaded.

If you have flowe-cli installed:

```bash
flowe install-skill
```

Or grab the skill file directly: **[SKILL.md](./SKILL.md)** → drop it at `~/.claude/skills/flowe/SKILL.md` (or your project's `.claude/skills/flowe/SKILL.md`).

To opt out of the auto-trigger entirely, set `FLOWE_OPT_OUT=1` in your shell or delete the skill file.

## Documentation

Everything lives at **[cli.flowe.ai/docs](https://cli.flowe.ai/docs/)**:

- [Quickstart](https://cli.flowe.ai/docs/quickstart/) — first reset in under a minute
- [Why flowe](https://cli.flowe.ai/docs/why/) — what it's for and what it isn't
- [Voices](https://cli.flowe.ai/docs/voices/sage/) — sage, ada, ellie
- [Protocols](https://cli.flowe.ai/docs/protocols/box-breath/) — the eleven reset families
- [Commands reference](https://cli.flowe.ai/docs/reference/commands/) — every flag
- [Privacy](https://cli.flowe.ai/docs/reference/privacy/) — what we collect, what we don't

## Filing issues

- **Bug?** Open an [issue](https://github.com/flowe-ai/flowe-cli/issues/new) with your OS, Node version, the command you ran, and what you saw.
- **Feature request?** Open an issue, +1 existing ones to vote.
- **Linux support?** [+1 here](https://github.com/flowe-ai/flowe-cli/issues/1)
- **Windows support?** [+1 here](https://github.com/flowe-ai/flowe-cli/issues/2)

Releases are mirrored to this repo's [Releases](https://github.com/flowe-ai/flowe-cli/releases) tab on every npm publish.

## Links

- npm package: [npmjs.com/package/@flowe-ai/cli](https://www.npmjs.com/package/@flowe-ai/cli)
- Docs: [cli.flowe.ai/docs](https://cli.flowe.ai/docs/)
- Landing page: [cli.flowe.ai](https://cli.flowe.ai)
- Company: [flowe.ai](https://flowe.ai)

## License

MIT. The published npm artifact ships with its `LICENSE` file.
