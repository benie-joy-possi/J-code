# Contributing to JET ⚡

First — thank you for wanting to contribute. JET is built in public and we want it to be accessible to everyone, whether you're a Rust expert or someone who just started coding last month.

> **AI writes most of the code here.** What matters most is understanding what you're building and why. Don't be intimidated by Rust — use AI tools to write the implementation, and focus on understanding the logic.

---

## Table of Contents

- [How JET is structured](#how-jet-is-structured)
- [Ways to contribute](#ways-to-contribute)
- [Adding a new provider](#adding-a-new-provider) ← most popular contribution
- [Setting up your dev environment](#setting-up-your-dev-environment)
- [Code style and standards](#code-style-and-standards)
- [Submitting a pull request](#submitting-a-pull-request)
- [Reporting bugs](#reporting-bugs)
- [Community](#community)

---

## How JET is structured

```
J-code/
├── spec/              # Behavioral specs — what JET should do (no Rust required)
├── src-rust/          # The Rust implementation
│   └── crates/
│       ├── cli/       # Terminal UI, command handling, REPL loop
│       ├── core/      # Core agent logic, session management
│       ├── providers/ # AI provider adapters (THIS is where most contributions go)
│       ├── tools/     # Built-in tools (bash, file read/write, etc.)
│       └── api/       # HTTP client, request/response types
├── CONTRIBUTING.md    # This file
├── ROADMAP.md         # Where the project is going
└── README.md          # Project overview
```

**The key insight:** JET is split into two layers — **specs** and **implementation**. The `spec/` folder describes *what* JET should do in plain English. The `src-rust/` folder is *how* it does it. You can contribute to specs without writing a single line of Rust.

---

## Ways to contribute

### 🔌 Add a new AI provider (most wanted)
See [Adding a new provider](#adding-a-new-provider) below. This is the #1 thing contributors can do right now.

### 🐛 Fix a bug
Look for issues labeled [`bug`](https://github.com/benie-joy-possi/J-code/issues?q=label%3Abug) and pick one that interests you.

### 📝 Improve documentation
Unclear docs? Fix them. Missing examples? Add them. Every doc improvement helps the next contributor.

### 💡 Suggest a feature
Open an issue with the `enhancement` label. Describe what you want and why. We discuss before building.

### ✅ Good first issues
Look for [`good-first-issue`](https://github.com/benie-joy-possi/J-code/issues?q=label%3Agood-first-issue) — these are scoped, well-explained, and perfect for getting started.

---

## Adding a new provider

This is the most impactful thing you can contribute right now. Every new provider makes JET useful to a new group of developers.

> [!TIP]
> **Most new providers are OpenAI-compatible.** If the provider you want to add uses the OpenAI API format (like Groq, DeepSeek, or local servers), you can add it in just a few lines of code by adding a factory function to the `OpenAiCompatProvider`.

**Providers we still need:** Gemini, specialized custom models, and further enhancements to existing ones.

### Step 1: Open an issue first

Before writing code, open an issue saying "I want to add [Provider Name]". This prevents two people working on the same thing at the same time.

### Step 2: Understand the Provider trait

Every provider in JET implements the same Rust trait. A trait is like a contract — it says "every provider must have these methods."

```rust
// src-rust/crates/providers/src/lib.rs

pub trait Provider: Send + Sync {
    /// The display name shown to the user (e.g., "OpenAI", "Anthropic")
    fn name(&self) -> &str;

    /// The list of models this provider supports
    fn models(&self) -> Vec<Model>;

    /// Send messages and get a response back
    async fn complete(&self, request: CompletionRequest) -> Result<CompletionResponse>;

    /// Stream a response token by token (for real-time output)
    async fn stream(&self, request: CompletionRequest) -> Result<ResponseStream>;

    /// Check if the provider is properly configured (API key present, etc.)
    fn is_configured(&self) -> bool;
}
```

You implement this trait for your new provider, and JET will automatically support it everywhere — `/connect`, `/model`, `/providers`, cost tracking, everything.

### Step 3: Use the OpenAI-Compatible shortcut (Recommended)

If the provider is OpenAI-compatible, do NOT implement the trait from scratch. Instead, add a factory function to:
`src-rust/crates/api/src/providers/openai_compat_providers.rs`

Example for a new provider:
```rust
pub fn your_provider() -> OpenAiCompatProvider {
    let key = std::env::var("YOUR_PROVIDER_API_KEY").unwrap_or_default();
    OpenAiCompatProvider::new(
        ProviderId::YOUR_PROVIDER,
        "Your Provider Name",
        "https://api.yourprovider.com/v1",
    ).with_api_key(key)
}
```

### Step 4: Fallback: Create your provider file

If the provider is NOT OpenAI-compatible (like Anthropic or Gemini), you'll need to implement the trait.

Create a new file: `src-rust/crates/providers/src/providers/your_provider.rs`

Here's a template to get you started:

```rust
use crate::{Provider, Model, CompletionRequest, CompletionResponse, ResponseStream};
use anyhow::Result;

pub struct YourProvider {
    api_key: String,
    base_url: String,
}

impl YourProvider {
    pub fn new(api_key: String) -> Self {
        Self {
            api_key,
            base_url: "https://api.yourprovider.com/v1".to_string(),
        }
    }
}

impl Provider for YourProvider {
    fn name(&self) -> &str {
        "YourProvider"
    }

    fn models(&self) -> Vec<Model> {
        vec![
            Model { id: "model-name-1".to_string(), display_name: "Model Name 1".to_string() },
            Model { id: "model-name-2".to_string(), display_name: "Model Name 2".to_string() },
        ]
    }

    async fn complete(&self, request: CompletionRequest) -> Result<CompletionResponse> {
        // Make your HTTP call to the provider's API here
        // Most providers use an OpenAI-compatible format, so you can often copy from openai.rs
        todo!()
    }

    async fn stream(&self, request: CompletionRequest) -> Result<ResponseStream> {
        todo!()
    }

    fn is_configured(&self) -> bool {
        !self.api_key.is_empty()
    }
}
```

### Step 4: Register your provider

Add it to the provider registry in `src-rust/crates/providers/src/registry.rs`:

```rust
pub fn all_providers() -> Vec<Box<dyn Provider>> {
    vec![
        Box::new(AnthropicProvider::from_env()),
        Box::new(OpenAIProvider::from_env()),
        Box::new(YourProvider::from_env()), // ← add this line
    ]
}
```

### Step 5: Add to the docs

Add your provider to the table in `PROVIDERS.md` and update the status from 📋 Planned to ✅ Stable or 🔨 In Progress.

### Step 6: Write a test

```rust
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_provider_name() {
        let provider = YourProvider::new("test_key".to_string());
        assert_eq!(provider.name(), "YourProvider");
    }

    #[test]
    fn test_models_not_empty() {
        let provider = YourProvider::new("test_key".to_string());
        assert!(!provider.models().is_empty());
    }
}
```

### Step 7: Open a pull request

See [Submitting a pull request](#submitting-a-pull-request).

---

## Setting up your dev environment

### Requirements

- [Rust](https://rustup.rs/) (stable, 1.75+)
- Git
- An API key for the provider you're working with (or use Ollama for free local testing)

### Clone and build

```bash
git clone https://github.com/benie-joy-possi/J-code
cd J-code
cargo build
```

### Run JET locally

```bash
cargo run -- /connect
```

### Run tests

```bash
cargo test
```

### Lint and format

```bash
cargo clippy        # check for common mistakes
cargo fmt           # auto-format your code
```

Make sure both pass before opening a PR.

---

## Code style and standards

- **Format with `cargo fmt`** — no exceptions, it runs in CI
- **No clippy warnings** — fix them or explain why they should be suppressed
- **Keep functions small** — if it's more than ~50 lines, consider splitting it
- **Error handling** — use `anyhow::Result` for user-facing errors, don't `.unwrap()` in production paths
- **Comments** — explain *why*, not *what*. The code shows what; comments explain the reasoning.
- **No telemetry, ever** — no analytics calls, no tracking, no phoning home

---

## Submitting a pull request

1. Fork the repo
2. Create a branch: `git checkout -b add-gemini-provider`
3. Make your changes
4. Run `cargo test && cargo clippy && cargo fmt`
5. Commit with a clear message: `feat: add Gemini provider with gemini-1.5-pro support`
6. Push and open a PR against `main`

**PR title format:**
- `feat:` — new feature or provider
- `fix:` — bug fix
- `docs:` — documentation only
- `refactor:` — code change with no behavior change
- `test:` — adding or fixing tests

**In your PR description, include:**
- What does this PR do?
- How did you test it?
- Any known limitations?

---

## Reporting bugs

Open an issue using the **Bug Report** template. Include:
- JET version (`jet --version`)
- Your OS
- Provider you were using
- What you expected to happen
- What actually happened
- Any error output

---

## Community

Have questions? Open an issue — we respond to everything.

Want to discuss ideas before opening a PR? Open a [Discussion](https://github.com/benie-joy-possi/J-code/discussions).

---

Built with ❤️ by Ju-nine and Joy. Made better by everyone who contributes.