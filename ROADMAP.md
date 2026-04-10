# JET Roadmap ⚡

This is the public roadmap for JET. It shows where we are, where we're going, and what we need help with.

**Contributions are welcome at every stage.** If something is "In Progress" or "Planned", jump in.

---

## Current Status: v0.1 — Foundation

We have a working Rust CLI with Anthropic and OpenAI support, experimental speech modes, and a `/connect` command. The codebase is a clean-room Rust implementation with solid foundations to build on.

---

## v0.2 — Multi-Provider (Active Development)

**Theme:** No more lock-in. Connect to any AI.

| Feature | Status | Issue |
|---|---|---|
| Provider trait abstraction | ✅ Done | #1 |
| Gemini provider | 🔨 In Progress | #2 |
| Groq provider | ✅ Done | #3 |
| Ollama (local) provider | ✅ Done | #4 |
| OpenRouter provider | ✅ Done | #5 |
| Mistral provider | ✅ Done | #6 |
| Custom/self-hosted OpenAI-compatible endpoint | ✅ Done | #7 |
| `/model` command (switch models mid-session) | ✅ Done | #8 |
| `/providers` command (list all providers) | ✅ Done | #9 |
| Encrypted config file (API keys stored safely) | 🔨 In Progress | #10 |

---

## v0.3 — Power Features

**Theme:** Make developers faster than they've ever been.

| Feature | Status | Notes |
|---|---|---|
| `/cost` command — real-time token usage + cost | 📋 Planned | Per-session and cumulative |
| Session export (Markdown / GitHub Gist) | 📋 Planned | Share your sessions |
| Multi-agent mode | 📋 Planned | Spin up parallel agents on your machine for complex tasks |
| Plugin system | 📋 Planned | Community-built tools (web search, git, etc.) |
| `/benchmark` mode | 📋 Planned | Same prompt → all providers, compare side by side |
| Terminal themes | 📋 Planned | Customize your JET terminal |
| GitHub Issues agent | 📋 Planned | Point JET at a GitHub issue, it works on it |

---

## v0.4 — Messaging Bridge (Optional Module)

**Theme:** Run JET from anywhere — not just your terminal.

This is a **CLI-first** feature. The base agent runs in your terminal. The messaging bridge is an optional module you activate if you want to control JET from your phone.

| Feature | Status | Notes |
|---|---|---|
| WhatsApp bridge | 📋 Planned | Send messages, get back status updates |
| Telegram bridge | 📋 Planned | Bot-style control |
| Discord bridge | 📋 Planned | For teams using Discord |
| Async job queue | 📋 Planned | "Start this, ping me when done" |
| Progress notifications | 📋 Planned | JET messages you when a long task completes |

**How it will work:**
```
You (WhatsApp): "Review the PR #142 and fix all lint errors"

JET (WhatsApp): "Starting on PR #142... I'll message you when done."

[20 minutes later]

JET (WhatsApp): "Done ✅
  - Fixed 14 lint errors across 6 files
  - Opened PR #143 with the changes
  - 2 issues need your review (see PR comments)"
```

---

## v1.0 — Stable Release

**Theme:** Production-ready. Trusted by teams.

| Goal | Notes |
|---|---|
| All v0.x features stable | No experimental flags needed |
| Full test coverage on core paths | CI is green on every commit |
| Published to crates.io | `cargo install jet-code` works globally |
| Security audit | External review of API key handling, permissions |
| Full documentation | Every feature documented with examples |
| Official branding + logo | The JET identity is complete |

---

## How to get involved

- Pick any 📋 **Planned** or 🔨 **In Progress** item
- Open or comment on the linked GitHub issue
- See [CONTRIBUTING.md](./CONTRIBUTING.md) for how to start

We especially need help with **v0.2 providers** — each provider is self-contained and a great first contribution.

---

*Roadmap is updated as priorities shift. Last updated: April 2026.*