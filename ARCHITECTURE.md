# JET Architecture ⚡

This document explains how JET is built, why decisions were made, and how the pieces fit together. Read this before making any structural changes.

---

## The Two-Layer Design

JET is intentionally split into two layers:

```
spec/         → What JET should do      (behavioral contracts, plain English + pseudocode)
src-rust/     → How JET does it         (Rust implementation)
```

**Why?** This is called clean-room engineering. The spec layer describes behavior without being tied to any specific implementation. This means:

- A contributor who doesn't know Rust can write or improve specs
- A Rust contributor has a clear target to implement against
- The project isn't accidentally tied to any third party's code

The spec is authoritative. If the Rust code disagrees with the spec, the spec wins.

---

## Crate Structure

```
src-rust/crates/
├── cli/           Main binary, REPL loop, command dispatch, terminal rendering
├── core/          Agent logic, session state, message history, system prompts
├── providers/     AI provider adapters — the most important extension point
├── tools/         Built-in tools the agent can use (bash, file I/O, etc.)
├── api/           HTTP client, request/response types, retry logic
└── config/        User configuration, API key storage, settings
```

Each crate has a single clear responsibility. Avoid adding things to `core/` unless they genuinely belong there.

---

## The Provider System

The heart of JET's multi-provider support is the `Provider` trait in `src-rust/crates/providers/src/lib.rs`.

```
┌─────────────────────────────────┐
│          User's terminal        │
└──────────────┬──────────────────┘
               │
┌──────────────▼──────────────────┐
│         JET Core Agent          │
│  (doesn't care which provider)  │
└──────────────┬──────────────────┘
               │  calls Provider trait
       ┌───────┴────────┐
       │                │
┌──────▼─────┐   ┌──────▼──────┐   ┌──────────────┐
│ Anthropic  │   │   OpenAI    │   │  (your new   │
│  Provider  │   │  Provider   │   │   provider)  │
└──────┬─────┘   └──────┬──────┘   └──────┬───────┘
       │                │                 │
       ▼                ▼                 ▼
  api.anthropic    api.openai      api.whatever.com
```

The core agent never imports a specific provider directly. It holds a `Box<dyn Provider>`. This means adding a new provider requires zero changes to core logic.

### The Provider trait

```rust
pub trait Provider: Send + Sync {
    fn name(&self) -> &str;
    fn models(&self) -> Vec<Model>;
    async fn complete(&self, request: CompletionRequest) -> Result<CompletionResponse>;
    async fn stream(&self, request: CompletionRequest) -> Result<ResponseStream>;
    fn is_configured(&self) -> bool;
}
```

See [CONTRIBUTING.md](./CONTRIBUTING.md#adding-a-new-provider) for a full walkthrough of implementing this.

---

## The Tool System

JET has a set of built-in tools that the AI agent can call during a session. Tools are also trait-based:

```rust
pub trait Tool: Send + Sync {
    fn name(&self) -> &str;
    fn description(&self) -> &str;
    fn schema(&self) -> serde_json::Value;  // JSON Schema for the tool's parameters
    async fn execute(&self, input: serde_json::Value) -> Result<ToolOutput>;
}
```

Built-in tools live in `src-rust/crates/tools/src/`. Current tools:

| Tool | What it does |
|---|---|
| `BashTool` | Run shell commands |
| `FileReadTool` | Read file contents |
| `FileWriteTool` | Write to files |
| `FileEditTool` | Make targeted edits to files |
| `GlobTool` | Find files matching a pattern |
| `GrepTool` | Search file contents |

Tools are registered in `tools/src/registry.rs` and sent to the provider as part of the system prompt / tool-use schema.

---

## Session Flow

Here's what happens when a user types a message in JET:

```
User input
    │
    ▼
Command check (/connect, /model, /cost, etc.)
    │ not a command
    ▼
Build message history (user + assistant turns)
    │
    ▼
Call provider.stream(request) ← this hits the AI API
    │
    ▼
Stream tokens to terminal (print as they arrive)
    │
    ▼
Check for tool calls in the response
    │ tool call found
    ▼
Execute tool (BashTool, FileWriteTool, etc.)
    │
    ▼
Add tool result to message history
    │
    ▼
Call provider again with tool result ← loop until no more tool calls
    │
    ▼
Final response printed to terminal
    │
    ▼
Update session state (cost tracking, history, etc.)
```

---

## Configuration

JET stores its config in `~/.config/jet/config.toml`. The format:

```toml
[default]
provider = "anthropic"
model = "claude-sonnet-4-6"

[providers.anthropic]
api_key = "sk-ant-..."

[providers.openai]
api_key = "sk-..."

[providers.ollama]
base_url = "http://localhost:11434"
# no api_key needed for local Ollama
```

API keys are stored in the OS keychain when available (macOS Keychain, Linux libsecret, Windows Credential Manager) and fall back to the config file. Keys are never logged or sent anywhere except the provider's own API.

---

## The Messaging Bridge (Proposed)

> [!NOTE]
> This section describes the **proposed** architecture for the messaging bridge (v0.4+). This system is not yet implemented.

The messaging bridge (WhatsApp, Telegram, Discord) is planned as a separate optional process that communicates with the JET agent over a local Unix socket. It is NOT part of the core CLI — it's an add-on module.

```
┌──────────────────┐        Unix socket        ┌─────────────────────┐
│  JET CLI agent   │ ◄────────────────────────► │  Messaging bridge   │
│ (runs on machine)│                            │ (optional process)  │
└──────────────────┘                            └─────────┬───────────┘
                                                          │
                                                    ┌─────▼──────┐
                                                    │  WhatsApp  │
                                                    │  Telegram  │
                                                    │  Discord   │
                                                    └────────────┘
```

This design means:
- The CLI works perfectly without the bridge
- The bridge can't break the CLI if it crashes
- The bridge is community-contributed (different people can build different bridges)

Architecture details for the bridge will be in a separate `BRIDGE.md` once v0.4 development starts.

---

## Adding a Feature — Decision Guide

Before adding anything, ask these questions:

**Does it belong in `core/`?**  
Only if it's fundamental to the agent loop (session state, message history, prompt building).

**Does it belong in `providers/`?**  
Only if it's specific to how a provider formats requests/responses.

**Does it belong in `tools/`?**  
If it's a capability the AI agent can invoke (file operations, web search, etc.).

**Does it belong in `cli/`?**  
If it's a user-facing command or terminal rendering concern.

**Should it be a separate crate?**  
If it's optional, has its own dependencies, or could be used independently — yes.

When in doubt, open an issue and ask.