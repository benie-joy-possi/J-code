# CLAURST Repository Analysis and Documentation Generation Prompt

## Objective
You are an expert software engineering assistant tasked with thoroughly analyzing the CLAURST repository—a Rust-based reimplementation of Claude Code (Anthropic's AI coding assistant). Your goal is to index the repository's structure and contents, understand its purpose and components, and produce clear, actionable documentation on how to set it up, test it, and make it work. CLAURST is a clean-room Rust implementation created from behavioral specifications, featuring advanced systems like multi-agent orchestration, memory consolidation, and experimental features.

## Repository Context
- **Location**: The repository is located at [/home/benie-joy/My_Projects/CLAUDE/claurst/](cci:7://file:///home/benie-joy/My_Projects/CLAUDE/claurst:0:0-0:0)
- **Primary Language**: Rust with Cargo workspace structure
- **Key Directories**: 
  - [src-rust/](cci:7://file:///home/benie-joy/My_Projects/CLAUDE/claurst/src-rust:0:0-0:0): Main Rust implementation with 12 crates
  - [spec/](cci:7://file:///home/benie-joy/My_Projects/CLAUDE/claurst/spec:0:0-0:0): Behavioral specifications (13 markdown files detailing architecture)
  - [.github/](cci:7://file:///home/benie-joy/My_Projects/CLAUDE/claurst/.github:0:0-0:0): CI/CD workflows
  - [public/](cci:7://file:///home/benie-joy/My_Projects/CLAUDE/claurst/public:0:0-0:0): Assets (images, screenshots)
- **Current Status**: Functional codebase with build system but limited testing infrastructure

## Constraints and Guidelines
- **Input Handling**: Analyze the repository at the provided absolute path [/home/benie-joy/My_Projects/CLAUDE/claurst/](cci:7://file:///home/benie-joy/My_Projects/CLAUDE/claurst:0:0-0:0). Use directory listing, file reading, and code analysis tools to systematically explore the codebase.
- **Indexing Approach**: 
  - Start with high-level directory structure ([src-rust/crates/](cci:7://file:///home/benie-joy/My_Projects/CLAUDE/claurst/src-rust/crates:0:0-0:0), [spec/](cci:7://file:///home/benie-joy/My_Projects/CLAUDE/claurst/spec:0:0-0:0), etc.)
  - Deep-dive into key files: [Cargo.toml](cci:7://file:///home/benie-joy/My_Projects/CLAUDE/claurst/src-rust/Cargo.toml:0:0-0:0), [src-rust/Cargo.toml](cci:7://file:///home/benie-joy/My_Projects/CLAUDE/claurst/src-rust/Cargo.toml:0:0-0:0), main entry points, and spec documents
  - Analyze Rust crates individually, noting dependencies and feature flags
  - Extract build configurations, CI workflows, and environment requirements
- **Analysis Depth**: 
  - Identify core purpose: Rust reimplementation of Claude Code with advanced AI agent features
  - Map components: CLI, tools, agents, terminal UI, API integration, etc.
  - For testing: Examine any existing test files, CI configurations, and suggest comprehensive testing strategies
  - For setup/running: Parse Cargo.toml files, build scripts, and runtime requirements
- **Output Structure**: Produce a single, self-contained Markdown (.md) file named `claurst_documentation.md`. Use clear headings, subheadings, code blocks, tables, and bullet points for readability. Include a table of contents at the top. Ensure the documentation is "LLM-compatible" by being concise yet complete—avoid fluff, but provide enough detail for an AI or human to replicate setup/testing without external help.
- **Clarity and Completeness**: 
  - Use simple, precise language. Define technical terms on first use (e.g., "Cargo: Rust's package manager and build system").
  - Include code snippets, file paths, and command examples.
  - Add warnings for common pitfalls (e.g., "Ensure Rust 1.70+ is installed").
  - Note experimental features and feature gates clearly.
  - If components are incomplete (e.g., limited tests), provide guidance on improvements.
- **Accuracy and Safety**: Base all claims on the repository's actual contents. Do not invent features or instructions. Prioritize security best practices.
- **Scope Limitations**: Focus on core functionality, setup, testing, and components. Do not include unrelated details like contributor bios unless they impact setup/testing.
- **Length and Formatting**: Aim for 1500-4000 words, depending on repo complexity. Use Markdown syntax strictly (e.g., no HTML). End with a "Next Steps" section for improvements.

## Steps to Follow
1. **Repository Access and Initial Scan**:
   - List root directory contents and identify main directories (src-rust/, spec/, .github/, public/).
   - Read key configuration files: root Cargo.toml, src-rust/Cargo.toml, README.md, LICENSE.md.

2. **Detailed Indexing**:
   - Analyze each crate in [src-rust/crates/](cci:7://file:///home/benie-joy/My_Projects/CLAUDE/claurst/src-rust/crates:0:0-0:0): cli/, core/, tools/, api/, etc.
   - Review spec documents in [spec/](cci:7://file:///home/benie-joy/My_Projects/CLAUDE/claurst/spec:0:0-0:0) for architectural insights.
   - Extract dependencies, features, and build requirements from Cargo.toml files.
   - Examine CI workflows in [.github/](cci:7://file:///home/benie-joy/My_Projects/CLAUDE/claurst/.github:0:0-0:0) for testing and deployment processes.

3. **Purpose and Overview**:
   - Summarize CLAURST as a Rust reimplementation of Claude Code with advanced features.
   - List tech stack: Rust, tokio, ratatui, etc.
   - Highlight unique systems: multi-agent orchestration, Dream memory consolidation, Buddy companion system.

4. **Component Documentation**:
   - Create sections for each major component (crates, tools, systems).
   - For each: Describe purpose, key files, dependencies, and relationships.
   - Include tables for tool registry, feature flags, and crate purposes.

5. **Setup and Running Instructions**:
   - Provide step-by-step commands to install Rust, clone (if needed), build, and run.
   - Include prerequisites (OS, Rust version, dependencies).
   - Handle common issues (e.g., "If build fails, check Rust toolchain version").
   - Note experimental features and how to enable them.

6. **Testing Documentation**:
   - Describe existing testing infrastructure (CI, any test files).
   - Suggest comprehensive testing strategies: unit tests, integration tests, CLI testing.
   - Provide example test commands and frameworks to use.
   - Recommend test coverage goals and CI improvements.

7. **Compile and Output**:
   - Write the entire documentation as a Markdown file named `claurst_documentation.md`.
   - Ensure it's ready to be committed to the repo or shared.

## Example Output Structure