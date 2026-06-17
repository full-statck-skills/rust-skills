<div align="center">

# rust-skills

**Rust language skills — standard library, Cargo, async, testing, unsafe, web, embedded, macros**

[![GitHub](https://img.shields.io/badge/github-full--statck--skills%2Frust-skills-green.svg)](https://github.com/full-statck-skills/rust-skills)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-Compatible-purple.svg)](https://agentskills.io)

English | [简体中文](./README.zh-CN.md)

[Introduction](#-introduction) · [Install](#-install) · [Skills](#-skills) · [Repository Layout](#-repository-layout) · [Official Sources](#-official-sources) · [Supported Agents](#-supported-agents) · [Ecosystem](#-ecosystem)

</div>

---

## 📖 Introduction

**Rust Skills** is a curated collection of Agent Skills for AI coding agents focused on the [Rust programming language](https://rust-lang.org) and its ecosystem, part of the [Full Stack Skills](https://github.com/partme-ai/full-stack-skills) ecosystem maintained by [PartMe.AI](https://github.com/partme-ai).

This package includes **9 skills** organized into three layers: **Core Language & Tooling** (language, Cargo, testing), **Domain Libraries** (concurrency, unsafe, macros), and **Ecosystem** (web, embedded). Each skill is a self-contained `SKILL.md` file that AI agents load on-demand.

## 📦 Install

```bash
npx skills add full-statck-skills/rust-skills
```

Or install specific skills: `npx skills add full-statck-skills/rust-skills --skill <skill-name>`

## 🎯 Skills (9) by Layer

```
┌──────────────────────────────────────────────────────────────┐
│              Layer 1: Core Language & Tooling                │
│                                                              │
│  rust-core ──────── Primary skill, full language + std lib    │
│  rust-cargo ─────── Cargo build system, packages, workspaces  │
│  rust-testing ───── Testing, benches, doc-tests, coverage     │
├──────────────────────────────────────────────────────────────┤
│              Layer 2: Domain Libraries                       │
│                                                              │
│  rust-concurrency  async/await, threads, sync primitives      │
│  rust-unsafe ────── Unsafe Rust, FFI, raw pointers           │
│  rust-macros ────── Declarative (macro_rules!) & proc macros  │
├──────────────────────────────────────────────────────────────┤
│              Layer 3: Ecosystem & Platforms                   │
│                                                              │
│  rust-web ───────── Web frameworks (axum, actix, rocket)     │
│  rust-embedded ──── Embedded Rust, no_std, HAL, RTIC         │
│  rust-code-review ─ Code review for style & correctness       │
└──────────────────────────────────────────────────────────────┘
```

## 📚 Repository Layout

```text
rust-skills/
├── skills/
│   ├── rust-core/              # Primary aggregate skill (language + std lib)
│   │   ├── examples/           # Offline quick-start examples
│   │   └── references/         # Language & std lib references
│   ├── rust-cargo/             # Cargo build system specialization
│   ├── rust-code-review/       # Code review skill
│   ├── rust-concurrency/       # Async & threading
│   ├── rust-testing/           # Testing & benchmarking
│   ├── rust-unsafe/            # Unsafe Rust & FFI
│   ├── rust-web/               # Web frameworks
│   ├── rust-embedded/          # Embedded & no_std
│   └── rust-macros/            # Macro system
├── scripts/
├── README.md
└── README.zh-CN.md
```

## 📖 Official Sources

The `rust-core` main skill is grounded in these official documentation sources:

- [The Rust Programming Language (The Book)](https://doc.rust-lang.org/book/)
- [Rust Standard Library API](https://doc.rust-lang.org/std/index.html)
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/)
- [Cargo Book](https://doc.rust-lang.org/cargo/index.html)
- [Rust Reference](https://doc.rust-lang.org/reference/index.html)
- [Rustonomicon (Unsafe Rust)](https://doc.rust-lang.org/nomicon/index.html)
- [Rust Edition Guide](https://doc.rust-lang.org/edition-guide/index.html)
- [Rustc Book](https://doc.rust-lang.org/rustc/index.html)
- [Rustdoc Book](https://doc.rust-lang.org/rustdoc/index.html)
- [Clippy Book](https://doc.rust-lang.org/clippy/index.html)
- [Rust Embedded Book](https://doc.rust-lang.org/embedded-book)
- [Command Line Book](https://rust-cli.github.io/book/index.html)

## 🤖 Supported Agents

Works with [Claude Code](https://code.claude.com), [Codex](https://developers.openai.com/codex), [Cursor](https://cursor.com), [OpenCode](https://opencode.ai), [Gemini CLI](https://geminicli.com), [GitHub Copilot](https://github.com/features/copilot), [Windsurf](https://codeium.com/windsurf), and [70+ others](https://agentskills.io/clients).

### Claude Code Installation

**Option 1: npx skills CLI (Recommended)**

```bash
npx skills add full-statck-skills/rust-skills
```

**Option 2: Manual Installation**

```bash
git clone https://github.com/full-statck-skills/rust-skills.git
cp -r rust-skills/skills/* .claude/skills/
```

For more details, see the [Claude Code Skills Guide](https://code.claude.com/docs/en/skills) and [Agent Skills Spec](https://agentskills.io/).

## 🌐 Ecosystem

| Resource | Link |
|----------|------|
| **Full Stack Skills** | [github.com/partme-ai/full-stack-skills](https://github.com/partme-ai/full-stack-skills) |
| **All Skill Groups** | [github.com/full-statck-skills](https://github.com/full-statck-skills) |
| **Agent Skills Spec** | [agentskills.io](https://agentskills.io) |
| **Skills CLI** | [github.com/vercel-labs/skills](https://github.com/vercel-labs/skills) |

## 📄 License

Apache 2.0 — see [LICENSE](LICENSE).
