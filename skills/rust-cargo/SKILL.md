---
name: rust-cargo
description: Rust Cargo 构建系统技能 — 涵盖 Cargo.toml 配置、包管理、依赖管理、工作空间、构建脚本、交叉编译、cargo 子命令等。基于官方 Cargo Book。当用户需要配置项目、管理依赖、构建发布时激活。
---

# Rust Cargo 构建系统

> 基于官方 [Cargo Book](https://doc.rust-lang.org/cargo/index.html)（源码 `library/` 中的 Cargo.lock/Cargo.toml 参考）。

## Capability Boundaries

### ✅ 强项
1. Cargo.toml 配置（依赖、特性、profile、元数据）
2. 依赖管理（crates.io、git、path、workspace）
3. 工作空间（workspace）多包项目
4. 特性门控（[features] 表、条件编译）
5. 构建脚本（build.rs）
6. Profile 配置（dev、release、自定义）
7. 交叉编译配置
8. cargo 子命令（test、bench、clippy、fmt、doc）
9. Cargo 环境变量与配置

### ⚠️ 前置要求
1. 确认 Cargo 可用（`cargo --version`）

### ❌ 不适用范围
1. Rust 语言语法 → 使用 `rust-core` 技能
2. 测试编写 → 使用 `rust-testing` 技能

## 何时使用

- "帮我创建 Rust 项目"
- "配置 Cargo.toml"
- "添加依赖/管理特性"
- "设置工作空间"
- "交叉编译配置"

## Data Privacy

本技能不收集、存储或传输任何用户数据。

---

# Cargo 参考（Rust 1.93）

## 项目创建与结构

```bash
cargo new my-project           # 创建二进制项目
cargo new my-lib --lib         # 创建库项目
cargo init                     # 在现有目录初始化
```

## Cargo.toml 配置

```toml
[package]
name = "my-project"
version = "0.1.0"
edition = "2024"           # 可用: 2015, 2018, 2021, 2024
authors = ["you@example.com"]
description = "A great project"
license = "MIT"
repository = "https://github.com/user/repo"

[dependencies]
serde = { version = "1", features = ["derive"] }
tokio = { version = "1", features = ["full"] }
reqwest = { version = "0.12", default-features = false, features = ["json"] }

[dev-dependencies]
tempfile = "3"

[build-dependencies]
cc = "1"

[features]
default = ["std"]
std = []
serde-support = ["dep:serde"]

[profile.release]
opt-level = 3              # 优化级别
lto = true                 # 链接时优化
codegen-units = 1          # 单代码生成单元
strip = "symbols"          # 剥离符号
panic = "abort"            # panic=abort

[workspace]
members = ["crates/*", "app"]
resolver = "3"            # 工作空间解析器
```

## 关键命令

```bash
cargo build                    # 构建（debug）
cargo build --release          # 发布构建
cargo check                    # 仅检查（不生成二进制）
cargo test                     # 运行测试
cargo bench                    # 运行基准测试
cargo run                      # 运行
cargo clippy                   # 静态分析
cargo fmt                      # 格式化
cargo doc --open               # 生成文档
cargo update                   # 更新依赖
cargo publish                  # 发布到 crates.io
cargo add serde                # 添加依赖（Cargo 1.62+）
cargo remove serde             # 移除依赖
cargo tree                     # 查看依赖树
cargo audit                    # 安全检查（需要 cargo-audit）
```

## 构建脚本（build.rs）

```rust
// build.rs — 编译时执行
fn main() {
    println!("cargo::rerun-if-changed=src/proto/*");
    println!("cargo::rustc-cfg=feature=\"custom\"");
    
    // 生成代码
    std::fs::write(
        std::path::Path::new("src/generated.rs"),
        "pub fn generated() -> i32 { 42 }",
    ).unwrap();
}
```

## 环境变量

```rust
// 在 src 中可用
env!("CARGO_PKG_VERSION")      // 包版本
env!("CARGO_MANIFEST_DIR")     // 包目录
option_env!("CARGO_PKG_NAME")  // 可选环境变量
```
