<div align="center">

# rust-skills

**Rust 语言技能 — 标准库、Cargo、异步、测试、不安全代码、Web、嵌入式、宏**

[![GitHub](https://img.shields.io/badge/github-full--statck--skills%2Frust-skills-green.svg)](https://github.com/full-statck-skills/rust-skills)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent%20Skills-兼容-purple.svg)](https://agentskills.io)

[English](./README.md) | 简体中文

[简介](#-简介) · [安装](#-安装) · [技能列表](#-技能列表-9) · [仓库布局](#-仓库布局) · [官方来源](#-官方来源) · [支持的智能体](#-支持的智能体) · [生态](#-生态)

</div>

---

## 📖 简介

**Rust 技能** 是一组面向 Rust 编程语言及其生态的 AI 编码智能体技能，属于 [Full Stack Skills](https://github.com/partme-ai/full-stack-skills) 生态，由 [PartMe.AI](https://github.com/partme-ai) 维护。

本包包含 **9 个技能**，按三个层级组织：**核心语言与工具**（语言、Cargo、测试）、**领域库**（并发、不安全代码、宏）、**生态**（Web、嵌入式、代码审查）。每个技能是独立的 `SKILL.md` 文件，AI 智能体按需加载。

## 📦 安装

```bash
npx skills add full-statck-skills/rust-skills
```

或安装特定技能：`npx skills add full-statck-skills/rust-skills --skill <skill-name>`

## 🎯 技能列表 (9) 按层级

```
┌──────────────────────────────────────────────────────────────┐
│              Layer 1: 核心语言与工具                          │
│                                                              │
│  rust-core ←─────── 主技能，语言 + 全标准库综合                │
│  rust-cargo ←────── Cargo 构建系统、包管理、工作空间            │
│  rust-testing ←──── 测试、基准测试、文档测试、覆盖率             │
├──────────────────────────────────────────────────────────────┤
│              Layer 2: 领域库                                  │
│                                                              │
│  rust-concurrency ← async/await、线程、同步原语                 │
│  rust-unsafe ←───── 不安全 Rust、FFI、裸指针                   │
│  rust-macros ←───── 声明宏 (macro_rules!) 与过程宏             │
├──────────────────────────────────────────────────────────────┤
│              Layer 3: 生态与平台                               │
│                                                              │
│  rust-web ←──────── Web 框架（axum、actix、rocket）            │
│  rust-embedded ←─── 嵌入式 Rust、no_std、HAL、RTIC            │
│  rust-code-review ← Rust 代码审查（风格与正确性）               │
└──────────────────────────────────────────────────────────────┘
```

## 📚 仓库布局

```text
rust-skills/
├── skills/
│   ├── rust-core/              # 主技能（语言 + 标准库）
│   │   ├── examples/           # 离线快速入门示例
│   │   └── references/         # 语言与标准库参考
│   ├── rust-cargo/             # Cargo 构建系统
│   ├── rust-code-review/       # 代码审查
│   ├── rust-concurrency/       # 异步与线程
│   ├── rust-testing/           # 测试与基准测试
│   ├── rust-unsafe/            # 不安全 Rust 与 FFI
│   ├── rust-web/               # Web 框架
│   ├── rust-embedded/          # 嵌入式与 no_std
│   └── rust-macros/            # 宏系统
├── scripts/
├── README.md
└── README.zh-CN.md
```

## 📖 官方来源

`rust-core` 主技能基于以下官方文档：

- [Rust 程序设计语言（The Book）](https://doc.rust-lang.org/book/)
- [Rust 标准库 API](https://doc.rust-lang.org/std/index.html)
- [Rust 例程](https://doc.rust-lang.org/rust-by-example/)
- [Cargo 手册](https://doc.rust-lang.org/cargo/index.html)
- [Rust 参考手册](https://doc.rust-lang.org/reference/index.html)
- [Rustonomicon（不安全 Rust）](https://doc.rust-lang.org/nomicon/index.html)
- [Rust 版本指南](https://doc.rust-lang.org/edition-guide/index.html)
- [rustc 手册](https://doc.rust-lang.org/rustc/index.html)
- [rustdoc 手册](https://doc.rust-lang.org/rustdoc/index.html)
- [Clippy 手册](https://doc.rust-lang.org/clippy/index.html)
- [嵌入式 Rust 手册](https://doc.rust-lang.org/embedded-book)
- [命令行应用手册](https://rust-cli.github.io/book/index.html)

## 🤖 支持的智能体

兼容 [Claude Code](https://code.claude.com)、[Codex](https://developers.openai.com/codex)、[Cursor](https://cursor.com)、[OpenCode](https://opencode.ai)、[Gemini CLI](https://geminicli.com)、[GitHub Copilot](https://github.com/features/copilot)、[Windsurf](https://codeium.com/windsurf) 及 [70+ 其他](https://agentskills.io/clients)。

### Claude Code 安装

**方式 1：npx skills CLI（推荐）**

```bash
npx skills add full-statck-skills/rust-skills
```

**方式 2：手动安装**

```bash
git clone https://github.com/full-statck-skills/rust-skills.git
cp -r rust-skills/skills/* .claude/skills/
```

更多详情请参考 [Claude Code 技能指南](https://code.claude.com/docs/en/skills) 和 [Agent Skills 规范](https://agentskills.io/)。

## 🌐 生态

| 资源 | 链接 |
|----------|------|
| **Full Stack Skills** | [github.com/partme-ai/full-stack-skills](https://github.com/partme-ai/full-stack-skills) |
| **所有技能组** | [github.com/full-statck-skills](https://github.com/full-statck-skills) |
| **Agent Skills 规范** | [agentskills.io](https://agentskills.io) |
| **Skills CLI** | [github.com/vercel-labs/skills](https://github.com/vercel-labs/skills) |

## 📄 许可证

Apache 2.0 — 参见 [LICENSE](LICENSE).
