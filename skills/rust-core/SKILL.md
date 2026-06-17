---
name: rust-core
description: Rust 编程语言核心技能 — 涵盖 Rust 1.93 语法、标准库、所有权模型、trait 系统、泛型、生命周期、错误处理、迭代器、模式匹配、模块系统等。从第一性原理出发，基于官方 rust-lang/rust 源码分析。当用户需要进行 Rust 编程（包括语法、标准库使用、代码组织、类型系统）时激活。
---

# Rust 核心语言技能

> 基于 Rust 1.93 官方源码（rust-lang/rust）分析，覆盖语法、标准库与最佳实践。

## Capability Boundaries

### ✅ 强项
1. Rust 语法与类型系统（枚举、结构体、trait、泛型、生命周期）
2. 所有权模型（所有权、借用、生命周期注解、内部可变性）
3. 标准库核心类型（Option、Result、Iterator、Cell、Rc、Arc）
4. 错误处理（? 运算符、Result 组合器、自定义错误类型）
5. 模块与包管理（crate、模块、use、pub、可见性）
6. 集合类型（Vec、HashMap、HashSet、String、BTreeMap）
7. 常用 trait（Clone、Copy、Debug、Display、PartialEq、Eq、Hash、Ord）
8. 智能指针（Box、Rc、Arc、Cell、RefCell、OnceCell、LazyCell）
9. 函数式编程（闭包、迭代器适配器、方法链）
10. 测试基础（#[test]、#[should_panic]、#[cfg(test)]）
11. 条件编译（#[cfg]、#[cfg_attr]）
12. 属性与派生宏（#[derive]、#[repr]）

### ⚠️ 前置要求
1. 确认 Rust 版本（`rustc --version`），本技能基于 Rust 1.93
2. 确认 Cargo 可用（`cargo --version`）

### ❌ 不适用范围
1. Cargo 构建系统高级配置 → 使用 `rust-cargo` 技能
2. 异步编程（async/await）→ 使用 `rust-concurrency` 技能
3. 不安全 Rust 与 FFI → 使用 `rust-unsafe` 技能
4. 宏系统（声明宏、过程宏）→ 使用 `rust-macros` 技能
5. Web 框架开发 → 使用 `rust-web` 技能
6. 嵌入式与 no_std 开发 → 使用 `rust-embedded` 技能
7. 代码审查 → 使用 `rust-code-review` 技能

## 何时使用

- "用 Rust 写一个..."
- "Rust 的所有权/生命周期/借用规则是什么"
- "在 Rust 中如何实现 X 特性"
- 需要使用标准库核心类型（Vec、String、HashMap 等）
- 需要理解 Rust 的类型系统与 trait 设计模式

## Data Privacy

本技能不收集、存储或传输任何用户数据。

---

# Rust 1.93 核心语言参考

基于官方 `rust-lang/rust` 源码（`library/core/src/` 6.5MB、`library/std/src/` 6.9MB、`library/alloc/src/` 2.2MB）的系统性分析。

## 一、枚举与代数数据类型（ADT）

Rust 的枚举是带数据的代数类型，这是最核心的设计模式。

```rust
// Option — 表示可选值（无 null）
pub enum Option<T> {
    None,
    Some(T),
}

// Result — 错误处理（#[must_use]）
pub enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

**关键特性：**
- `Option` 的**空指针优化**：`Option<&T>` 与裸指针大小相同
- `Result` 标注 `#[must_use]`，忽略会触发编译器警告
- 通过 `match`、`if let`、`let else` 进行模式匹配

```rust
// match 表达式（穷尽匹配）
match result {
    Ok(v) => println!("value: {v}"),
    Err(e) => eprintln!("error: {e}"),
}

// if let（单分支匹配）
if let Some(x) = optional { println!("{x}"); }

// let-else（需要在 else 分支返回/退出）
let Some(n) = NonZero::new(n) else {
    return Ok(());
};

// matches! 宏
let has_value = matches!(result, Ok(_));
```

## 二、结构体与方法

```rust
// 具名结构体
pub struct Vec<T, A: Allocator = Global> {
    buf: RawVec<T, A>,
    len: usize,
}

// 实现块
impl Path {
    pub fn from_ident(ident: Ident) -> Path { /* ... */ }
    pub fn is_global(&self) -> bool { /* ... */ }
}

// 元组结构体
pub struct Label(pub Ident);

// 单元结构体（零大小类型）
pub struct PhantomData<T: ?Sized>;
```

## 三、Trait 系统

Traits 是 Rust 唯一的接口抽象。

```rust
// Iterator trait — 最核心的 trait 之一
pub trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
    
    // 带默认实现的方法（40+ 个）
    fn count(self) -> usize where Self: Sized { /* ... */ }
    fn nth(&mut self, n: usize) -> Option<Self::Item> { /* ... */ }
    fn map<B, F>(self, f: F) -> Map<Self, F> where ... { /* ... */ }
    fn filter<P>(self, predicate: P) -> Filter<Self, P> where ... { /* ... */ }
    fn fold<B, F>(self, init: B, f: F) -> B where ... { /* ... */ }
    fn collect<B>(self) -> B where B: FromIterator<Self::Item> { /* ... */ }
}

// 操作符重载
pub trait PartialEq<Rhs: ?Sized = Self> {
    fn eq(&self, other: &Rhs) -> bool;
}

// 排序
pub trait Ord: Eq + PartialOrd {
    fn cmp(&self, other: &Self) -> Ordering;
}

// 借用（HashMap 的 get 方法）
pub trait Borrow<Borrowed: ?Sized> {
    fn borrow(&self) -> &Borrowed;
}
```

**常用派生宏：**
```rust
#[derive(Clone, Copy, Debug, PartialEq, Eq, Hash)]
pub struct MyStruct { /* ... */ }
```

## 四、所有权与借用

```rust
// 所有权转移（move）
let s = String::from("hello");
let t = s;  // s 被移动，不再可用

// 不可变借用（&T）
fn len(s: &String) -> usize { s.len() }

// 可变借用（&mut T）
fn push_char(s: &mut String, c: char) { s.push(c); }

// 借用规则：
// 1. 任意多个 &T，或唯一一个 &mut T
// 2. 引用不能比被引用的值活得更长

// Clone — 深拷贝
let a = vec![1, 2, 3];
let b = a.clone();

// Copy — 按位拷贝（栈上类型）
let x: i32 = 42;
let y = x;  // x 仍可用

// 智能指针
let boxed: Box<i32> = Box::new(42);      // 堆分配
let rc: Rc<i32> = Rc::new(42);           // 单线程引用计数
let arc: Arc<i32> = Arc::new(42);        // 多线程引用计数

// 内部可变性
let cell = Cell::new(42);                 // 用于 Copy 类型
let refcell = RefCell::new(String::new()); // 运行时借用检查
*refcell.borrow_mut() = "hello".to_string();
```

## 五、泛型与生命周期

```rust
// 泛型结构体
pub struct Vec<T, A: Allocator = Global> { /* ... */ }

// 泛型方法
pub fn get<Q>(&self, k: &Q) -> Option<&V>
where
    K: Borrow<Q>,
    Q: Hash + Eq + ?Sized,
{ /* ... */ }

// 生命周期
struct Lifetime<'a> {
    name: &'a str,
}

// 生命周期省略规则（常见情形自动推断）
// &T       →   &'a T
// &mut T   →   &'a mut T
// fn f(&self) → 输出生命周期 = &self 的生命周期

// 'static  — 整个程序运行期间存活
let s: &'static str = "hello";

// 泛型常量参数
fn next_chunk<const N: usize>(&mut self) -> Result<[Self::Item; N], IntoIter> { /* ... */ }
```

## 六、错误处理

```rust
// 方式1：? 运算符（最常用，自动传播错误）
fn read_file(path: &str) -> io::Result<String> {
    let mut file = File::open(path)?;
    let mut contents = String::new();
    file.read_to_string(&mut contents)?;
    Ok(contents)
}

// 方式2：模式匹配
let version = match header.get(0) {
    None => return Err("invalid header"),
    Some(&1) => Version::V1,
    Some(&2) => Version::V2,
    Some(_) => return Err("unknown version"),
};

// 方式3：组合器
result
    .map(|v| v + 1)
    .map_err(|e| format!("error: {e}"))
    .and_then(|v| Ok(v * 2))
    .unwrap_or_default();

// 方式4：unwrap / expect（简单场景）
let file = File::open("foo.txt").expect("failed to open");
let v = optional.unwrap_or(0);

// 关键原则：Result 必须被使用（#[must_use]）
```

## 七、闭包与函数式模式

```rust
// 闭包：|| { ... }
|accum, elem| accum + 1

// move 闭包（将外部变量所有权移入）
thread::spawn(move || {
    do_work(data)  // data 的所有权移入闭包
});

// 迭代器方法链（零成本抽象）
let nums: Vec<_> = values
    .iter()
    .map(|x| x * 2)
    .filter(|x| x > &10)
    .take(5)
    .collect();

// Fn / FnMut / FnOnce 三组 trait
fn call_fn(f: impl Fn(i32) -> i32) -> i32 { f(1) }
fn call_fn_mut(f: impl FnMut(i32) -> i32) -> i32 { f(1) }
fn call_fn_once(f: impl FnOnce(i32) -> i32) -> i32 { f(1) }
```

## 八、模块与可见性

```rust
// lib.rs
#![no_std]  // 无标准库
#![feature(core_intrinsics)]  // 启用 unstable 特性

// 模块声明
pub mod option;        // 公开模块
mod macros;            // 私有模块
pub(crate) mod util;   // 仅 crate 可见

// 重新导出
pub use self::option::Option;
pub use self::iter::Iterator;

// 路径
use std::collections::HashMap;
use std::sync::Arc;
```

## 九、属性与条件编译

```rust
// 稳定性属性
#[stable(feature = "core", since = "1.6.0")]
#[unstable(feature = "try_trait_v2", issue = "84277")]

// cfg 条件编译
#[cfg(target_os = "linux")]
fn only_linux() { /* ... */ }

#[cfg(not(no_global_oom_handling))]
fn alloc_fn() { /* ... */ }

// must_use
#[must_use = "iterators are lazy and do nothing unless consumed"]

// repr 控制内存布局
#[repr(C)]
#[repr(transparent)]
```

## 十、标准库核心类型速查

| 类型 | 说明 | 关键方法 |
|------|------|---------|
| `Vec<T>` | 动态数组 | `push`, `pop`, `insert`, `remove`, `len`, `iter` |
| `String` | UTF-8 字符串 | `push_str`, `push`, `as_str`, `len`, `trim` |
| `HashMap<K,V>` | 哈希表 | `insert`, `get`, `remove`, `contains_key`, `entry` |
| `HashSet<T>` | 哈希集合 | `insert`, `contains`, `remove`, `union`, `intersection` |
| `BTreeMap<K,V>` | 有序映射 | `insert`, `get`, `range`, `first`, `last` |
| `Box<T>` | 堆分配 | `new`, `deref`, `into_raw` |
| `Rc<T>` | 引用计数 | `new`, `clone`, `downgrade` |
| `Arc<T>` | 原子引用计数 | `new`, `clone`, `downgrade` |
| `Cell<T>` | 内部可变（Copy） | `get`, `set`, `replace`, `take` |
| `RefCell<T>` | 内部可变（运行时） | `borrow`, `borrow_mut` |

## 官方参考

- [The Rust Programming Language (The Book)](https://doc.rust-lang.org/book/)
- [Rust Standard Library API](https://doc.rust-lang.org/std/index.html)
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/)
- [Rust Reference](https://doc.rust-lang.org/reference/index.html)
- [Rust Edition Guide](https://doc.rust-lang.org/edition-guide/index.html)
