# ⚡ JET (J-code)

### *The AI coding agent that belongs to no one cloud.*

> Built in Rust. Multi-provider. Local-first. Yours.

[![Built with Rust](https://img.shields.io/badge/Built_with-Rust-CE4D2B?style=for-the-badge&logo=rust&logoColor=white)](https://www.rust-lang.org/)
[![Multi-Provider](https://img.shields.io/badge/Providers-8%2B-6C3EF0?style=for-the-badge)](./PROVIDERS.md)
[![Local AI](https://img.shields.io/badge/Local_AI-Ollama_Ready-2EA44F?style=for-the-badge)](./PROVIDERS.md#ollama)
[![No Telemetry](https://img.shields.io/badge/Telemetry-None-FF6B35?style=for-the-badge)](./CONTRIBUTING.md)
[![License: GPL-3.0](https://img.shields.io/badge/License-GPL--3.0-blue?style=for-the-badge)](./LICENSE.md)
[![Version](https://img.shields.io/badge/Version-0.1.0-2E8B57?style=for-the-badge)](./CHANGELOG.md)

---

## What is JET?

**JET** is a terminal-based AI coding agent written in Rust. It lets you pair-program with any AI provider — Claude, GPT-4, Gemini, or even a local model running on your own machine — directly from your terminal.

No lock-in. No forced subscriptions. No telemetry. Just fast, powerful AI coding assistance wherever you work.

```
╔══════════════════════════════════════════════════════╗
║  ⚡ JET  v0.1.0  │  Provider: claude-sonnet-4-6     ║
║  /connect to switch │  /cost to see usage            ║
╚══════════════════════════════════════════════════════╝

> Refactor the auth module to use JWT tokens

  ✦ Reading src/auth/mod.rs...
  ✦ Analyzing dependencies...
  ✦ Writing changes to 3 files...
  ✓ Done. 47 lines changed.
```

---

## Why JET?

| Problem | JET's Answer |
|---|---|
| Claude Code only works with Anthropic | JET works with **any** provider |
| API costs add up fast | Use **local models** (Ollama) for free |
| Heavy, slow, Electron-based tools | Pure Rust — **blazing fast, tiny binary** |
| Vendor lock-in | Switch providers with one command |
| Black-box behavior | **Open source**, clean-room Rust implementation |
| Hard to self-host or extend | Designed for contributors from day one |

---

## Supported Providers

| Provider | Status | Notes |
|---|---|---|
| 🟣 Anthropic (Claude) | ✅ Stable | claude-sonnet, claude-opus, claude-haiku |
| 🟢 OpenAI | ✅ Stable | gpt-4o, o3, gpt-4-turbo |
| 🔵 Google Gemini | 🔨 In Progress | gemini-1.5-pro, gemini-flash |
| 🟡 Groq | ✅ Stable | llama3, mixtral — ultra fast inference |
| 🏠 Ollama (Local) | ✅ Stable | Run **any model locally, for free** |
| 🌐 OpenRouter | ✅ Stable | 200+ models via one API key |
| 🔴 Mistral | ✅ Stable | mistral-large, codestral |
| ⚙️ Custom / Self-hosted | ✅ Stable | Any OpenAI-compatible endpoint |

Want to add a provider? See [CONTRIBUTING.md → Adding a Provider](./CONTRIBUTING.md#adding-a-new-provider).

---

## Quick Start

### Install

```bash
# From crates.io (recommended)
cargo install jet-code

# Or build from source
git clone https://github.com/benie-joy-possi/J-code
cd J-code
cargo build --release
```

### Connect your first provider

```bash
jet /connect
```

JET will walk you through selecting a provider and entering your API key. That's it.

### Start coding

```bash
jet
```

---

## Core Commands

| Command | What it does |
|---|---|
| `/connect` | Connect or switch your AI provider |
| `/model <name>` | Switch models mid-session |
| `/cost` | Show token usage and estimated cost for this session |
| `/providers` | List all available providers and their status |
| `/help` | Show all commands |
| `/exit` | Exit JET |

### Fun / Experimental

| Command | What it does |
|---|---|
| `/Rocky` | Switch to Rocky speech style |
| `/Caveman` | Switch to Caveman speech style |
| `/Normal` | Back to normal speech |

---

## Features

- **Multi-provider** — switch between any AI provider with `/connect`
- **Local-first** — use Ollama to run models on your own hardware, no API key needed
- **Session cost tracking** — always know what you're spending
- **Blazing fast** — written in Rust, single binary, no runtime dependencies
- **No telemetry** — your code and conversations stay on your machine
- **Extensible** — clean provider trait system makes adding new providers easy
- **Open source** — built in public, contributions welcome

---

## Architecture

JET is built as a clean-room Rust implementation. The architecture is split into two layers:

```
spec/          → Behavioral specifications (what JET should do)
src-rust/      → Rust implementation (how it does it)
```

This separation means contributors can work on specs without writing Rust, and Rust contributors have clear contracts to implement against.

See [ARCHITECTURE.md](./ARCHITECTURE.md) for a full breakdown.

---

## Contributing

JET is fully open source and welcomes contributors of all skill levels — whether you're a Rust expert or just learning. AI writes most of the code; what matters is understanding the implementation.

**The easiest way to contribute:** Add a new provider. It requires implementing one Rust trait and about 50 lines of code.

See [CONTRIBUTING.md](./CONTRIBUTING.md) to get started.

---

## Roadmap

| Version | Focus |
|---|---|
| **v0.1** | Core CLI, Anthropic + OpenAI stable (Done ✅) |
| **v0.2** | Multi-provider support, Groq, Ollama (Active 🔨) |
| **v0.3** | Power features, cost tracking, sessions (Planned 📋) |
| **v1.0** | Stable, production-ready release |

See [ROADMAP.md](./ROADMAP.md) for details and open tickets.

---

## Who built this?

JET is built by **Ju-nine** and **Joy** — two developers who wanted a coding agent they actually owned.

The name says it all: **fast, sharp, cuts through anything.**

---

## License

GPL-3.0 — see [LICENSE.md](./LICENSE.md)