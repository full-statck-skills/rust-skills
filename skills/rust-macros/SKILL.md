---
name: rust-macros
description: Rust 宏系统技能 — 涵盖声明宏（macro_rules!）、过程宏（derive、attribute、function-like）、宏设计模式、调试与测试。基于官方 library/core/src/macros/ 与 compiler/ 中的宏处理源码。当用户需要编写宏来减少重复代码、创建 DSL 或自定义派生时激活。
---

# Rust 宏系统

> 基于官方 `library/core/src/macros/mod.rs` 与 `compiler/` 中宏处理源码分析。

## Capability Boundaries

### ✅ 强项
1. 声明宏（macro_rules!）— 模式匹配与代码替换
2. 过程宏（proc_macro）— derive、attribute、function-like 三类
3. 宏设计模式（递归、TT muncher、解析器组合）
4. 宏调试（log_syntax!、trace_macros!）
5. 辅助宏工具（paste、quote、syn）

### ⚠️ 前置要求
1. 深入理解 Rust 语法

### ❌ 不适用范围
1. Rust 基础语法 → 使用 `rust-core` 技能
2. 派生宏的简单使用（如 #[derive(Debug)]）→ 使用 `rust-core` 技能

## 何时使用

- "减少重复代码"
- "创建 DSL"
- "实现自定义 derive"
- "编写声明宏"

## Data Privacy

本技能不收集、存储或传输任何用户数据。

---

# Rust 宏系统参考

## 声明宏（macro_rules!）

```rust
// 基础模式
macro_rules! my_vec {
    // 匹配 empty
    () => {
        Vec::new()
    };
    // 匹配单个元素重复
    ($($x:expr),* $(,)?) => {{
        let mut v = Vec::new();
        $(v.push($x);)*
        v
    }};
    // 匹配重复带分号
    ($elem:expr; $n:expr) => {
        vec![$elem; $n]
    };
}

// 片段类型符
// expr — 表达式
// ident — 标识符
// ty — 类型
// pat — 模式
// stmt — 语句
// block — 代码块
// item — 项
// meta — 属性元数据
// tt — token 树（最通用）

// assert_eq!（来自标准库的模式）
macro_rules! assert_eq {
    ($left:expr, $right:expr $(,)?) => {
        match (&$left, &$right) {
            (left_val, right_val) => {
                if !(*left_val == *right_val) {
                    let kind = $crate::panicking::AssertKind::Eq;
                    $crate::panicking::assert_failed(
                        kind, &*left_val, &*right_val,
                        $crate::option::Option::None,
                    );
                }
            }
        }
    };
}
```

## 过程宏（proc_macro）

```rust
// 需要在单独的 proc-macro crate 中定义
// Cargo.toml:
// [lib]
// proc-macro = true

// derive 宏
use proc_macro::TokenStream;

#[proc_macro_derive(MyTrait)]
pub fn my_derive(input: TokenStream) -> TokenStream {
    // 使用 syn 解析输入
    // 使用 quote 生成代码
    input
}

// 属性宏
#[proc_macro_attribute]
pub fn route(attr: TokenStream, item: TokenStream) -> TokenStream {
    item
}

// 函数式宏
#[proc_macro]
pub fn sql(input: TokenStream) -> TokenStream {
    input
}
```

## 常用工具 Crate

```toml
[dependencies]
syn = { version = "2", features = ["full"] }   // 解析 Rust 语法
quote = "1"                                      // 生成 Rust 代码
proc-macro2 = "1"                                // proc_macro 替代品
```

```rust
use syn::{parse_macro_input, DeriveInput, ItemFn};
use quote::quote;

#[proc_macro_derive(MyDebug)]
pub fn my_debug(input: TokenStream) -> TokenStream {
    let input = parse_macro_input!(input as DeriveInput);
    let name = &input.ident;
    
    let expanded = quote! {
        impl std::fmt::Debug for #name {
            fn fmt(&self, f: &mut std::fmt::Formatter<'_>) -> std::fmt::Result {
                write!(f, stringify!(#name))
            }
        }
    };
    
    TokenStream::from(expanded)
}
```

## 宏调试

```rust
// 在 nightly 中启用
#![feature(trace_macros)]

macro_rules! add { ($a:expr, $b:expr) => { $a + $b }; }

fn main() {
    trace_macros!(true);
    let x = add!(1, 2);
    trace_macros!(false);
}
```
