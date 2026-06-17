---
name: rust-cargo-build
description: Rust Cargo 构建系统技能 — Cargo.toml 配置、依赖管理（crates.io/git/path/workspace）、特性（features）与条件编译、profile 配置（opt-level/lto/debug/strip/panic）、构建脚本（build.rs）、工作空间、交叉编译、cargo 常用命令、发布与打包。基于官方 Cargo Book。当用户需要配置项目构建、管理依赖、优化编译产物时激活。
---

# Rust Cargo 构建系统

> 基于官方 [Cargo Book](https://doc.rust-lang.org/cargo/index.html) 与 The Rust Programming Language ch 14。

## Capability Boundaries

### ✅ 强项
1. Cargo.toml 完整配置（package、dependencies、lib/bin 目标）
2. 依赖管理（版本号/crates.io、git、path、workspace 引用）
3. 特性系统（[features]、dep: 前缀、默认特性、隐式特性、传递特性、条件编译）
4. Profile 配置（opt-level、lto、debug、strip、panic、codegen-units、incremental）
5. 构建脚本（build.rs：rerun-if-changed、生成代码、链接 native lib、环境变量、cfg）
6. 工作空间（workspace 定义、依赖继承、成员配置）
7. 交叉编译（target 配置、linker、CFLAGS、工具链覆盖）
8. cargo 全部常用命令（build/check/run/test/bench/doc/clippy/fmt/fix/publish/add/remove/upgrade/tree/audit）
9. 发布（publish 准备、manifest 字段、打包检查、cargo package）
10. 条件编译（#[cfg()]、#[cfg_attr()]、cfg! 宏、#[cfg_accessible()]、#[cfg(sanitize)]）

### ⚠️ 前置要求
1. Rust 工具链安装完成（`rustc --version`）
2. 理解 Cargo 工作目录结构

### ❌ 不适用范围
1. 模块与可见性 → 使用 `rust-project-structure` 技能
2. Rust 语法基础 → 使用 `rust-1.93` 技能
3. 测试编写 → 使用 `rust-testing` 技能

## 何时使用

- "配置 Cargo.toml"
- "添加/管理依赖"
- "特性门控配置"
- "工作空间设置"
- "交叉编译"
- "优化构建产物体积/性能"
- "写 build.rs 构建脚本"
- "发布 crate 到 crates.io"

## Data Privacy

本技能不收集、存储或传输任何用户数据。

## Workflow

步骤 1. **确认版本** — `cargo --version` 确认 ≥ 1.93.0
步骤 2. **明确目标** — 二进制 crate / 库 crate / 工作空间 / 多目标？
步骤 3. **配置 Cargo.toml** — 包元数据 + 依赖 + 特性 + profile
步骤 4. **配置构建脚本（可选）** — 必要时编写 build.rs
步骤 5. **构建与测试** — `cargo check` / `cargo build` / `cargo test`
步骤 6. **优化** — profile 调优、特性剪裁、交叉编译配置
步骤 7. **发布** — `cargo package` + `cargo publish`

---

## 一、Cargo.toml 完整配置

```toml
[package]
name = "my-crate"                # crates.io 上显示的名称
version = "0.1.0"                # 语义化版本
edition = "2024"                 # 2015 | 2018 | 2021 | 2024
authors = ["You <you@example.com>"]
description = "A Rust crate"     # 用于 crates.io 搜索
license = "MIT OR Apache-2.0"    # SPDX 表达式
license-file = "LICENSE"         # 自定义许可文件
repository = "https://github.com/user/my-crate"
homepage = "https://my-crate.dev"
documentation = "https://docs.rs/my-crate"
readme = "README.md"
keywords = ["cli", "tool"]       # 最多 5 个
categories = ["command-line-utilities"]  # 来自 crates.io 分类
rust-version = "1.80"            # MSRV（最小支持版本）
exclude = ["ci/", "benches/"]    # 发布时排除的文件
include = ["src/", "README.md"]  # 仅包含这些文件（与 exclude 互斥）

[lib]
name = "my_crate"                # 库名（默认 = package.name.replace('-', '_')）
path = "src/lib.rs"              # 库入口（默认）
crate-type = ["lib", "cdylib", "staticlib", "dylib", "proc-macro"]

[[bin]]
name = "my-app"                  # 二进制名
path = "src/main.rs"             # 入口文件
required-features = ["cli"]      # 需要启用 cli 特性才编译

[[example]]
name = "quickstart"
path = "examples/quickstart.rs"
required-features = ["demo"]

[[bench]]
name = "my-bench"
harness = false                  # 使用自定义基准框架（如 criterion）

[[test]]
name = "integration"
path = "tests/integration.rs"

[package.metadata]
# 自定义元数据（不会被 crates.io 验证）
docs-rs = { features = ["full"] }
```

## 二、依赖管理

### 版本语法

```toml
[dependencies]
# 宽松版本（推荐）
serde = "1"                   # >=1.0.0, <2.0.0
tokio = "1.35"                # >=1.35.0, <2.0.0
uuid = "=1.0.0"               # 精确版本（谨慎使用）

# 范围版本
image = ">=0.23, <0.25"      # 范围
regex = "~1.5"                # ~1.5 → >=1.5.0, <1.6.0
clap = "^3.2"                 # ^3.2 → >=3.2.0, <4.0.0（默认行为）
```

### 依赖来源

```toml
[dependencies]
# crates.io（默认注册表）
serde = { version = "1", features = ["derive"] }

# Git
my-dep = { git = "https://github.com/user/repo.git", branch = "main" }
my-dep = { git = "https://github.com/user/repo.git", tag = "v1.0" }
my-dep = { git = "https://github.com/user/repo.git", rev = "abc1234" }

# 本地路径
my-dep = { path = "../my-dep" }

# 工作空间继承
my-dep = { workspace = true }

# 可选依赖
compression = { version = "0.4", optional = true }

# 平台特定
[target.'cfg(target_os = "linux")'.dependencies]
inotify = "0.10"

[target.'cfg(windows)'.dependencies]
winapi = { version = "0.3", features = ["fileapi"] }

[target.x86_64-unknown-linux-gnu.dependencies]
native-tls = "0.2"

# 依赖分类
[build-dependencies]      # 仅 build.rs 可用
cc = "1"
bindgen = "0.70"

[dev-dependencies]        # 仅测试/示例/基准可用
tempfile = "3"
criterion = { version = "0.5", features = ["html_reports"] }
```

## 三、特性系统

```toml
[features]
default = ["std", "serde"]           # 默认启用的特性
std = []                             # 启用标准库
serde = ["dep:serde", "dep:serde_json"]  # 启用 serde 派生
unstable = ["tokio/unstable"]        # 传递依赖的特性
full = ["serde", "compression", "net"]  # 组合特性

# dep: 前缀（Cargo 1.60+）
# dep:serde 表示自动将 serde 可选依赖作为特性公开
# 使用前需要先将 serde 声明为 optional

# 隐式特性
# 对每个 `optional = true` 依赖，Cargo 自动生成同名特性
# [dependencies]
# serde = { version = "1", optional = true }
# 自动生成 `serde` 特性

# 特性解析器（Cargo 1.51+）
[package]
resolver = "3"     # 新版特性解析器（工作空间默认）
# resolver = "2"   # 旧版（兼容）
```

### 条件编译配合特性

```rust
// #[cfg(feature = "serde")]
#[cfg(feature = "serde")]
impl Serialize for MyType { /* ... */ }

// 多个特性条件
#[cfg(all(feature = "serde", feature = "std"))]
fn with_serde_std() { /* ... */ }

// cfg! 宏（运行时检查）
if cfg!(feature = "unstable") {
    experimental_api();
}

// cfg_attr
#[cfg_attr(feature = "serde", derive(Serialize, Deserialize))]
struct Config;
```

## 四、Profile 配置

```toml
# Cargo.toml

[profile.dev]                      # 默认开发构建
opt-level = 0                      # 0-3, s=size, z=more size
debug = true                       # true=完整, 1=line tables, 0=none, false=none
debug-assertions = true            # 运行时断言
overflow-checks = true             # 整数溢出检查
lto = false                        # 链接时优化
codegen-units = 256                # 并行代码生成单元（越多越慢但越快编译）
incremental = true                 # 增量编译
strip = "none"                     # none|debuginfo|symbols
panic = "unwind"                   # unwind|abort
rpath = false                      # 运行时搜索路径

[profile.release]                  # 发布构建
opt-level = 3                      # 最大优化
debug = false
debug-assertions = false
overflow-checks = false
lto = true                         # thin|fat|off
codegen-units = 1                  # 单单元（最大优化）
incremental = false
strip = "symbols"                  # 剔除符号（减小二进制）
panic = "abort"                    # abort 更小更快
rpath = false

# 自定义 profile
[profile.release-lite]             # 继承 release
inherits = "release"
opt-level = 2
lto = false

[profile.bench]                    # 基准测试默认
inherits = "release"
lto = false
```

### 使用自定义 profile

```bash
cargo build --profile release-lite
```

## 五、构建脚本（build.rs）

```rust
// build.rs — 在 src/ 编译前运行

use std::env;
use std::path::Path;

fn main() {
    // === 告诉 Cargo 何时重新运行 ===
    // 文件变化时重跑
    println!("cargo::rerun-if-changed=src/proto/");
    println!("cargo::rerun-if-changed=wrapper.h");

    // 环境变量变化时重跑
    println!("cargo::rerun-if-env-changed=MY_APP_VERSION");

    // === 设置编译器 cfg ===
    // 在 Rust 代码中可使用 #[cfg(feature = "custom")]
    println!("cargo::rustc-cfg=feature=\"custom\"");

    // === 传递链接参数 ===
    // 链接 native 库
    println!("cargo::rustc-link-lib=png");       // -lpng
    println!("cargo::rustc-link-search=/usr/local/lib");  // -L path

    // === 生成代码 ===
    let out_dir = env::var("OUT_DIR").unwrap();
    let dest_path = Path::new(&out_dir).join("generated.rs");
    std::fs::write(
        &dest_path,
        "pub fn generated_fn() -> i32 { 42 }\n"
    ).unwrap();

    // === 添加到模块 ===
    // 在 src/lib.rs: include!(concat!(env!("OUT_DIR"), "/generated.rs"));

    // === 编译 C 代码 ===
    // [build-dependencies] cc = "1"
    cc::Build::new()
        .file("src/native/helper.c")
        .include("src/native")
        .compile("helper");

    // === 生成 bindings ===
    // [build-dependencies] bindgen = "0.70"
    // let bindings = bindgen::Builder::default()
    //     .header("wrapper.h")
    //     .generate()
    //     .unwrap();
    // bindings.write_to_file(dest_path.join("bindings.rs")).unwrap();
}
```

### build.rs 可用环境变量

```rust
// 编译时可读取的环境变量
let out_dir = env::var("OUT_DIR").unwrap();           // 输出目录
let cargo_manifest_dir = env::var("CARGO_MANIFEST_DIR").unwrap();  // 包目录
let target = env::var("TARGET").unwrap();              // 目标 triple
let host = env::var("HOST").unwrap();                 // 主机 triple
let profile = env::var("PROFILE").unwrap();            // dev/release
let num_jobs = env::var("CARGO_BUILD_JOBS").unwrap();  // 并行任务数

// 更多 cfg 条件
println!("cargo::rustc-cfg=feature=\"{}\"", target);
```

## 六、工作空间（Workspace）

```toml
# 根目录 Cargo.toml
[workspace]
resolver = "3"

# 成员（路径列表）
members = [
    "crates/core",
    "crates/utils",
    # glob 模式
    "apps/*",
]

# 排除成员
exclude = [
    "crates/experimental",
]

# 依赖继承（Cargo 1.74+）
[workspace.dependencies]
serde = "1"
tokio = { version = "1", features = ["full"] }
thiserror = "2"

[workspace.package]
version = "0.1.0"
edition = "2024"
authors = ["You <you@example.com>"]
license = "MIT"

# 工作空间级 profile（覆盖所有成员）
[workspace.profile.release]
opt-level = 3
lto = true
```

```toml
# crates/core/Cargo.toml — 使用继承
[package]
name = "core"
version.workspace = true           # 继承 workspace.package.version
edition.workspace = true
authors.workspace = true

[dependencies]
serde.workspace = true             # 继承 workspace.dependencies.serde
tokio.workspace = true
```

```bash
# 工作空间命令
cargo build --workspace            # 构建所有成员
cargo test -p core                 # 仅测试 core 包
cargo publish -p core              # 仅发布 core
cargo run -p my-app                # 运行 my-app
```

## 七、交叉编译

```bash
# 1. 安装目标
rustup target add aarch64-unknown-linux-gnu
rustup target add x86_64-pc-windows-gnu
rustup target add wasm32-unknown-unknown

# 2. 构建
cargo build --target aarch64-unknown-linux-gnu

# 3. 使用 .cargo/config.toml 配置
```

```toml
# .cargo/config.toml
[target.aarch64-unknown-linux-gnu]
linker = "aarch64-linux-gnu-gcc"
rustflags = ["-C", "target-feature=+crt-static"]

[target.x86_64-pc-windows-gnu]
linker = "x86_64-w64-mingw32-gcc"

[target.wasm32-unknown-unknown]
runner = "wasmtime"                # 运行命令
rustflags = ["-C", "target-feature=+atomics,+bulk-memory"]

# 全局配置
[build]
rustflags = ["-C", "target-cpu=native"]
jobs = 8                            # 并行任务数
target-dir = "target"               # 产物目录
```

### .cargo/config.toml 关键配置

```toml
[alias]
b = "build"
t = "test"
r = "run"

[env]
# 传递给所有 cargo 命令的环境变量
RUST_LOG = "info"

[registries]
my-registry = { index = "https://my-registry.com/git-index" }

[net]
retry = 3
git-fetch-with-cli = true

[http]
proxy = "http://proxy.example.com:8080"
timeout = 30
```

## 八、cargo 命令速查

```bash
# 构建
cargo build                    # debug 构建
cargo build --release          # release 构建
cargo build --profile custom   # 自定义 profile
cargo build --target triple    # 交叉编译
cargo build --features "serde" # 启用特性
cargo build --no-default-features
cargo build --all-features     # 启用所有特性
cargo check                    # 只检查，不生成二进制（比 build 快）

# 运行
cargo run                      # 构建并运行
cargo run -- arg1 arg2         # 传递参数
cargo run --example quickstart # 运行示例

# 测试
cargo test                     # 运行测试
cargo test -- --test-threads=1 # 单线程
cargo test -- --nocapture      # 显示 stdout
cargo test --doc               # 文档测试

# 代码质量
cargo fmt                      # 格式化
cargo fmt --check              # CI 检查格式
cargo clippy                   # 静态分析
cargo clippy --fix             # 自动修复
cargo fix --edition            # edition 迁移

# 文档
cargo doc                      # 生成文档
cargo doc --open               # 打开浏览器
cargo doc --document-private-items

# 依赖管理
cargo add serde                # 添加依赖
cargo add serde --features derive
cargo add log --optional
cargo remove log               # 移除依赖
cargo upgrade                  # 升级依赖（需 cargo-edit）
cargo tree                     # 依赖树
cargo tree -d                  # 重复依赖检测
cargo tree -i serde            # 反向依赖

# 发布
cargo publish --dry-run        # 发布前检查
cargo package                  # 打包检查
cargo publish                  # 发布到 crates.io
cargo owner --add user         # 添加所有者

# 工具
cargo update                   # 更新 lock 文件
cargo clean                    # 清理产物
cargo locate-project           # 显示项目路径
cargo metadata                 # 项目元数据 JSON
cargo vendor                   # vendor 依赖到本地
cargo audit                    # 安全检查（需 cargo-audit）
cargo deny check               # 许可/安全/审计检查（需 cargo-deny）
cargo outdated                 # 检查过时依赖（需 cargo-outdated）
cargo bench                    # 运行基准测试
```

## 九、发布与打包

```bash
# 1. 检查包
cargo package                  # 创建 .crate 文件并验证
cargo package --list           # 列出将包含的文件

# 2. API 指南检查
cargo doc --no-deps            # 检查文档完整性
cargo test                     # 确保测试通过

# 3. 发布
cargo publish --dry-run        # 模拟发布
cargo publish                  # 发布到 crates.io

# 4. 版本管理
cargo login                    # 设置 API token
cargo yank --vers 0.1.0        # 弃用版本
cargo owner --list             # 查看所有者
```

**发布清单：**
```
□ Cargo.toml 包含 description + license + repository
□ README.md 齐全
□ 文档注释完整（cargo doc --no-deps 无警告）
□ 所有测试通过
□ semver 版本号正确
□ exclude/include 正确（无多余文件）
□ 依赖版本合理（不依赖过宽 / 过窄）
□ MSRV（rust-version）正确声明
```

## 十、常见问题（Gotchas）

1. **`resolver = "2" 与 "3" 的区别** — resolver "2" 合并特性 across 工作空间成员；resolver "3" 隔离每个 crate 的特性解析。在 edition 2021+ 中默认使用 "3"
2. **特性是附加的** — 同一 crate 被依赖多次时，特性是合并的，不能通过某个 usage 关闭
3. **`optional = true` 依赖自动创建特性** — 在 Cargo 1.60 之前自动创建；1.60+ 改用 `dep:` 前缀显式控制
4. **`cargo:rerun-if-changed` 必须用对路径** — 如果写入的文件在 `OUT_DIR` 之外，必须显式声明，否则 build.rs 可能不重跑
5. **`[patch]` 覆盖工作空间所有成员** — 在根 Cargo.toml 或工作空间目录 Cargo.toml 中使用 `[patch.crates-io]` 可以临时替换依赖
6. **SemVer 破坏性变更的检测** — `cargo-semver-checks` 可以自动化检测破坏性变更
7. **Workspace 依赖版本的锁定** — 工作空间共享一个 `Cargo.lock`，使用 `cargo update -p package` 更新单个包
8. **`cfg!(feature = "x")` 在 build.rs 中不可用** — build.rs 不参与 crate 特性系统，需用 `cargo::rustc-cfg` 传递


## Gotchas

1. cargo add 和手动编辑 Cargo.toml 可能冲突 - cargo add 重写 [dependencies] 时会丢弃注释
2. resolver 2 与 3 的特性合并行为不同 - resolver 3 隔离工作空间各 crate 特性
3. build.rs 未声明 rerun-if-changed - Cargo 只会在 crate 源文件变化时重跑 build.rs
4. optional=true 依赖自动创建特性 - 1.60+ 用 dep: 前缀显式控制
5. [patch] 只对当前工作空间有效 - 不能 patch build 依赖
6. cargo publish 后不可删除 - 只能 yank（弃用）不能移除

## FAQ

**Q：cargo check 和 cargo build 有什么区别？**
A：`cargo check` 只检查类型正确性，不生成可执行文件，比 `build` 快 2-5 倍，适合开发循环。

**Q：`features` 和 `cfg` 的关系是什么？**
A：`[features]` 是 Cargo.toml 中定义的编译时选项，其状态通过 `--cfg feature="xxx"` 传入编译器。Rust 代码中用 `#[cfg(feature = "xxx")]` 检查。

**Q：为什么需要 resolver = "3"？**
A：resolver "3" 在 edition 2021+ 中默认启用。它避免了工作空间中因特性合并产生的编译不一致问题，特别适合大型工作空间。

**Q：如何优化二进制大小？**
A：release profile 中设置 `opt-level = "z"`, `lto = true`, `codegen-units = 1`, `strip = "symbols"`, `panic = "abort"`；此外可使用 `cargo-bloat` 分析二进制各部分大小。

**Q：build.rs 有什么常见的陷阱？**
A：最常见的陷阱是忘记声明 `cargo:rerun-if-changed`，导致 build.rs 在源文件变更时不重跑。所有 build.rs 读取的文件都应声明。

**Q：如何在同一项目中使用多个 Rust edition？**
A：工作空间中每个成员可以有自己的 edition。根 Cargo.toml 的 `[workspace.package]` 中的 `edition` 是默认值。

**Q：`cargo add` 和手动编辑 Cargo.toml 哪个好？**
A：`cargo add`（Cargo 1.62+）保证版本号兼容、不破坏格式。推荐使用。`cargo remove` 同样可用。

## 官方参考

- [Cargo Book](https://doc.rust-lang.org/cargo/index.html)
- [Cargo Reference — Manifest](https://doc.rust-lang.org/cargo/reference/manifest.html)
- [Cargo Reference — Features](https://doc.rust-lang.org/cargo/reference/features.html)
- [Cargo Reference — Profiles](https://doc.rust-lang.org/cargo/reference/profiles.html)
- [Cargo Reference — Build Scripts](https://doc.rust-lang.org/cargo/reference/build-scripts.html)
- [Cargo Reference — Workspaces](https://doc.rust-lang.org/cargo/reference/workspaces.html)
- [Cargo Reference — Cross Compilation](https://doc.rust-lang.org/cargo/reference/config.html)
- [Cargo Commands](https://doc.rust-lang.org/cargo/commands/)
