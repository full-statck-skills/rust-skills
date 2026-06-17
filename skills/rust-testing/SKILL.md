---
name: rust-testing
description: Rust 测试与质量保障技能 — 涵盖 unit tests、integration tests、doc tests、benchmarks、coverage、proptest、mock 等。基于官方 library/ 中 test 子系统与 community best practices。当用户需要编写测试、进行基准测试或保证代码质量时激活。
---

# Rust 测试与质量保障

> 基于 Rust 标准库测试框架与社区最佳实践。

## Capability Boundaries

### ✅ 强项
1. 单元测试（#[test]、子模块测试）
2. 文档测试（/// 中的代码块）
3. 集成测试（tests/ 目录）
4. 基准测试（#[bench]、cargo bench）
5. 测试属性（#[should_panic]、#[ignore]）
6. 测试组织（测试模块、测试辅助函数）
7. 代码覆盖率（cargo-llvm-cov、tarpaulin）
8. 属性测试（proptest、quickcheck）
9. mock 对象（mockall、trait mocking）

### ⚠️ 前置要求
1. 理解 Rust 模块系统

### ❌ 不适用范围
1. Rust 基础语法 → 使用 `rust-core` 技能

## 何时使用

- "帮我写单元测试"
- "运行测试/基准测试"
- "检查代码覆盖率"
- "编写文档中的示例代码"

## Data Privacy

本技能不收集、存储或传输任何用户数据。

---

# Rust 测试参考

## 单元测试

```rust
// 在 src/lib.rs 或 src/foo.rs 文件中
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_addition() {
        assert_eq!(add(2, 2), 4);
    }

    #[test]
    fn test_failure() {
        assert!(true, "this should pass");
    }

    #[test]
    #[should_panic(expected = "divide by zero")]
    fn test_panic() {
        divide(1, 0);
    }

    #[test]
    #[ignore = "not implemented yet"]
    fn test_future() { /* TODO */ }
}
```

## 文档测试（doctest）

```rust
/// 将两个数相加。
///
/// ```
/// let result = my_crate::add(2, 3);
/// assert_eq!(result, 5);
/// ```
///
/// ```rust,should_panic
/// my_crate::divide(1, 0);
/// ```
pub fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

## 集成测试

```rust
// tests/my_test.rs — 放在项目根目录的 tests/ 文件夹
use my_project::add;

#[test]
fn integration_test() {
    assert_eq!(add(1, 2), 3);
}
```

## 测试命令

```bash
cargo test                    # 运行所有测试
cargo test test_name          # 按名称过滤
cargo test -- --nocapture     # 显示 println 输出
cargo test -- --test-threads=1 # 单线程运行
cargo test -- --ignored       # 运行被忽略的测试
cargo test --doc              # 只运行文档测试
cargo bench                   # 运行基准测试
```

## 基准测试

```rust
#![feature(test)]
extern crate test;

#[cfg(test)]
mod benches {
    use test::Bencher;

    #[bench]
    fn bench_add(b: &mut Bencher) {
        b.iter(|| add(1, 2));
    }
}
```

## 代码覆盖率

```bash
# 使用 cargo-llvm-cov
cargo install cargo-llvm-cov
cargo llvm-cov               # 运行并报告
cargo llvm-cov --open        # 生成 HTML 报告
```
