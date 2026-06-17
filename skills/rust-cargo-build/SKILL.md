---
name: rust-cargo-build
description: Rust Cargo 构建系统技能 — Cargo.toml 配置、依赖管理（crates.io/git/path/workspace）、特性（features）条件编译、profiles（opt-level/lto/debug）、构建脚本（build.rs）、交叉编译配置、cargo 命令大全。基于官方 Cargo Book。当用户需要配置项目构建、管理依赖、优化编译产时激活。
---

# Rust Cargo 构建系统

> 基于官方 [Cargo Book](https://doc.rust-lang.org/cargo/index.html) 与 The Book ch 14。

## Capability Boundaries

### ✅ 强项
1. Cargo.toml 完整配置（package、dependencies、lib/bin 目标）
2. 依赖声明（版本号/crates.io、git、path、workspace）
3. 特性系统（[features]、dep: 前缀、默认特性、条件编译）
4. profile 配置（opt-level、lto、debug、strip、panic）
5. 工作空间（workspace 定义、依赖继承）
6. 构建脚本（build.rs 常用模式、rerun-if-changed、生成代码）
7. 交叉编译（target 配置、linker、工具链）
8. cargo 全部常用命令与参数
9. 发布（publish 准备、package 字段、发布检查）

### ⚠️ 前置要求
1. Rust 工具链安装完成

### ❌ 不适用范围
1. 模块与可见性 → 使用 `rust-project-structure` 技能
2. Rust 语法基础 → 使用 `rust-1.93` 技能

## 何时使用

- "配置 Cargo.toml"
- "添加/管理依赖"
- "特性门控配置"
- "工作空间设置"
- "交叉编译"
- "优化构建产物体积/性能"

## Data Privacy

本技能不收集、存储或传输任何用户数据。

---

## 一、Cargo.toml 配置

```toml
[package]
name = "my-crate"
version = "0.1.0"
edition = "2024"           # 2015 | 2018 | 2021 | 2024
authors = ["You <you@example.com>"]
description = "A Rust crate"
license = "MIT OR Apache-2.0"
repository = "https://github.com/user/my-crate"
homepage = "https://my-crate.dev"
documentation = "https://docs.rs/my-crate"
keywords = ["cli", "tool"]
categories = ["command-line-utilities"]
readme = "README.md"
rust-version = "1.80"      # MSRV

[lib]
name = "my_crate"
path = "src/lib.rs"
crate-type = ["lib", "cdylib"]  # 库类型

[[bin]]
name = "my-app"
path = "src/main.rs"
```

## 二、依赖管理

```toml
[dependencies]
serde = "1"                        # crates.io ≥1.0.0
tokio = { version = "1", features = ["full"] }
regex = "=1.5.0"                   # 精确版本
uuid = { version = "1", optional = true }  # 可选依赖

# Git 依赖
my-dep = { git = "https://github.com/user/repo.git", branch = "main" }

# Path 依赖（本地开发）
my-dep = { path = "../my-dep" }

# Workspace 依赖
my-dep = { workspace = true }

[dev-dependencies]
tempfile = "3"                     # 仅测试使用
criterion = { version = "0.5", features = ["html_reports"] }

[build-dependencies]
cc = "1"                           # 编译 C 代码

[features]
default = ["std"]
std = []
serde = ["dep:serde", "dep:serde_json"]  # 显式特性依赖
unstable = ["tokio/unstable"]           # 传递 crate 特性
