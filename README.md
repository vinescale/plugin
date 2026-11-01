# VineScale for coding agents

VineScale is a safety gate for AI coding agents: Claude Code, Codex, Cursor, Antigravity and Grok Build. Before the agent runs a command, edits a file or calls a database tool, VineScale works out what the action would touch. It lets the action through, asks you first, or blocks it, with one line that says why. It runs on your machine and makes no network calls while it decides.

This repository holds only the Claude Code plugin, the Scoop bucket and the release builds of `vine`, version 0.2.0. VineScale's source code is not here.

## Install

In Claude Code:

```
/plugin marketplace add vinescale/plugin
/plugin install vinescale@vinescale
```

macOS and Linux, into `~/.local/bin`:

```
curl -fsSL https://github.com/vinescale/plugin/releases/latest/download/get.sh | sh
```

Windows PowerShell, into `%LOCALAPPDATA%\Programs\VineScale`:

```
irm https://github.com/vinescale/plugin/releases/latest/download/get.ps1 | iex
```

Scoop:

```
scoop bucket add vinescale https://github.com/vinescale/plugin
scoop install vinescale
vine install
```

None of these needs admin rights. Each checks the SHA-256 of what it downloads. The docs are at https://vinescale.com/docs/.

## Codex, Cursor, Antigravity and Grok Build

The one-line commands and Scoop run `vine install`, which adds the gate to Claude Code and to each of these agents it finds on your machine. To add it to one agent, for example after installing that agent:

```
vine install --agent codex        # or cursor, antigravity, grok
```

Then:

- Codex: run `/hooks` and trust VineScale's hook. Codex skips a new hook until you do.
- Cursor: restart Cursor.
- Antigravity: start a new conversation.
- Grok Build: start a new session.

Codex and Grok Build can't stop to ask you, so where VineScale would ask, it blocks there and tells the agent to hand the step to you. `vine uninstall` takes the gate out of every agent.

## Checking a release

Each release has a `SHA256SUMS` file. This repository's release workflow signs it with Sigstore's cosign, keyless, as the workflow itself, into `SHA256SUMS.sigstore.json`:

```
cosign verify-blob --bundle SHA256SUMS.sigstore.json \
  --certificate-identity-regexp '^https://github\.com/vinescale/plugin/\.github/workflows/release\.yml@refs/heads/release/v' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com SHA256SUMS
sha256sum --ignore-missing -c SHA256SUMS
```

A release whose notes say it isn't signed has only the checksums.

`THIRD_PARTY_LICENSES.txt` lists the open-source licenses of the code built into `vine`.

VineScale is not open source.
