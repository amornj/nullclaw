# Nullclaw Setup — Brodiedog iMac

## Current Setup

- **Version:** 2026.2.26
- **Binary:** `./zig-out/bin/nullclaw` (built with `zig build -Doptimize=ReleaseSmall`)
- **Zig version:** 0.15.2 (required exactly)
- **Config:** `~/.nullclaw/config.json`
- **Workspace:** `~/.nullclaw/workspace`

## Provider

- **Provider:** openai-codex (OAuth, imported from Codex CLI)
- **Model:** gpt-5.3-codex
- **Auth:** `~/.nullclaw/auth.json` (OAuth token from `~/.codex/auth.json`)
- **Token format:** Bearer token, auto-refreshed

## Telegram Bot

- **Bot:** @Brodiedog_bot
- **Bot ID:** 8611245804
- **Token:** stored in config under `channels.telegram.accounts.main`
- **Access:** `allow_from: ["*"]` (all users allowed)
- **Start command:** `./zig-out/bin/nullclaw channel start telegram`

## File Access

Nullclaw has access to:
- `/Users/home/Downloads`
- `/Users/home/projects`
- `~/.nullclaw/workspace` (default)

Configured via `autonomy.allowed_paths` in config.

## Commands Reference

```bash
# Build
zig build -Doptimize=ReleaseSmall

# Chat
./zig-out/bin/nullclaw agent -m "message"
./zig-out/bin/nullclaw agent                    # interactive

# Telegram
./zig-out/bin/nullclaw channel start telegram

# Status & diagnostics
./zig-out/bin/nullclaw status
./zig-out/bin/nullclaw doctor

# Gateway server
./zig-out/bin/nullclaw gateway --port 3000
```

## Future: Multiple Telegram Bots

Nullclaw supports multi-account natively. To add more bots, add accounts under `channels.telegram.accounts` in config:

```json
"telegram": {
  "accounts": {
    "bot1": {"bot_token": "TOKEN_1", "allow_from": ["*"]},
    "bot2": {"bot_token": "TOKEN_2", "allow_from": ["*"]},
    "bot3": {"bot_token": "TOKEN_3", "allow_from": ["*"]},
    "bot4": {"bot_token": "TOKEN_4", "allow_from": ["*"]}
  }
}
```

Each bot runs its own polling loop within a single process (~1 MB RAM total). Get tokens from @BotFather on Telegram.

To start all bots: `./zig-out/bin/nullclaw channel start telegram`

Optional: use Redis memory backend to share memory across bots.

## Future: Other Channels

Nullclaw supports 18 channels. To add more later:
- Discord, Slack, Signal, WhatsApp, Matrix, IRC, iMessage, Nostr, etc.
- Each configured under `channels.<name>.accounts` in config.
