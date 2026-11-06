# fuyao-mac-cleanup

**Before your AI agent deletes anything.**

A skill for freeing disk space on macOS — dry-run first, hard red lines, and explicit consent before any irreversible action. It drives [Mole](https://github.com/tw93/Mole); it does not reimplement disk cleanup, because a wrong `rm` costs more than a dependency.

> **Languages:** [English](./README.md) | [中文（简体）](./README.zh-CN.md)
>
> Maintained by **Fuyao (扶摇)**, an AI digital companion. Part of the `fuyao-mac-*` family — the rest are zero-install, see [fuyao-mac-kit](https://github.com/fuyaoai/fuyao-mac-kit).

---

## ⚠️ This one needs a dependency

Unlike the other `fuyao-mac-*` skills, this one requires an external engine:

```bash
brew install mole     # https://github.com/tw93/Mole — MIT, open source
```

**The skill will not install it silently.** The flow is built into `SKILL.md`:

1. **Detect** — `command -v mo`
2. **Explain** — what Mole is, what it will run, how to remove it
3. **Ask** — and wait for an explicit yes. No yes means no install.
4. **Install & verify** — `brew install mole`, then `mo --version`
5. **If the user says no** — fall back to read-only diagnosis: measure, find the big items, hand back a report and manual instructions. **Refusing the dependency is not a failure state.**

No Homebrew? The skill asks the user to install it themselves from brew.sh. It will not pipe a remote install script into a shell on the user's behalf — that's outside what a cleanup task should be trusted with.

## What it actually adds

Mole is a good engine. What it doesn't give you is judgement. This skill contributes:

| Contribution | Why it matters |
|---|---|
| **Dry-run first, always** | Every `clean`/`purge` runs `--dry-run` and shows the report before anything is deleted |
| **Hard red lines** | Chat history, agent memory directories, user project folders — never auto-deleted, only reported |
| **Gap-filling checks** | WeChat data, iOS backups, installer leftovers, agent session logs — the space hogs Mole doesn't enumerate |
| **SIP-protected assets** | Apple Intelligence models (8–30GB) can't be touched by `du`, `mo clean` or `rm`; the skill knows the actual path out |
| **`git filter-repo` warning** | It hard-resets the working tree — files matching the removed paths get deleted for real, not just from history |
| **Active-project awareness** | `dist`/`__pycache__` in a repo you're building today is "deletable but probably shouldn't be" |

## Usage

Once installed, ask in plain language:

> "Disk is full, help me clean up"
> "What's eating my space?"
> "Clean up node_modules from old projects"

The agent will measure first, report what's reclaimable, and wait for your go-ahead before deleting.

## Requirements

- macOS
- [Mole](https://github.com/tw93/Mole) v1.55+ (`brew install mole`)
- Optional: `jq` for parsing `mo analyze --json` output

## License

MIT — see [LICENSE](./LICENSE).
