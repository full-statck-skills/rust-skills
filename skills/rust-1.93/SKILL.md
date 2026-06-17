---
name: rust-1.93
description: Rust 1.93 编程语言聚合主技能 — 涵盖全部语法（变量、类型、函数、控制流、模式匹配）、所有权与生命周期、结构体与枚举、泛型与 trait 系统、集合与迭代器、错误处理、智能指针、闭包、模块系统、测试基础、以及标准库核心类型（Vec、String、HashMap、Option、Result、Box、Rc、Arc、Cell、RefCell、Path）。从第一性原理出发，基于官方 rust-lang/rust 源码分析。当用户需要进行任何 Rust 编程时自动激活，覆盖语言基础到标准库的全部核心知识。
---

# Rust 1.93 核心语言参考

> Rust 编程语言聚合主技能。基于 Rust 1.93.1 官方源码（rust-lang/rust 仓库：`library/` 与 `compiler/`）的系统性分析。

## Capability Boundaries

### ✅ 强项
1. 变量与数据类型（标量/复合类型、类型推断、类型转换、const/static、Shadowing）
2. 控制流（if/else、loop/while/for、break/continue/label、match/if let/while let）
3. 函数与闭包（定义、参数、返回值、闭包捕获 Fn/FnMut/FnOnce）
4. 所有权、借用、生命周期（ownership rules、&T/&mut T、slice、'a lifetime、elision、'static）
5. 结构体、枚举、模式匹配（struct/tuple struct/unit struct、enum with data、深度 match 解构）
6. 泛型与 trait（generic types/functions、trait definition、trait bound、impl/dyn Trait、associated types、super trait、derive、orphan rule）
7. 集合与迭代器（Vec、String、HashMap、HashSet、Iterator trait、适配器 map/filter/fold/collect）
8. 错误处理（panic、Result/Option、? 运算符、combinator、自定义错误、anyhow/thiserror）
9. 智能指针（Box、Rc、Arc、Cell、RefCell、Deref/Drop、OnceCell）
10. 模块与使用（mod、pub、use、路径 crate::/self::/super::、re-export、外部 crate）
11. 测试基础（#[test]、assert!/assert_eq!、#[should_panic]、#[cfg(test)] 模式）
12. 标准库常用模块（std::io、std::fs、std::path、std::time、std::env、std::process、std::net）
13. 属性（#[derive]、#[cfg]、#[repr]、#[must_use]、#[allow/deny]）

### ⚠️ 前置要求
1. 确认 Rust 工具链（`rustc --version` ≥ 1.93.0）
2. 确认 Cargo 可用（`cargo --version`）

### ❌ 不适用范围（按领域分流）
1. 项目脚手架与模块深度 → 使用 `rust-project-structure` 技能
2. Cargo 构建配置与依赖管理 → 使用 `rust-cargo-build` 技能
3. 并发/异步编程深度 → 使用 `rust-concurrency` 技能
4. 测试与基准测试深度 → 使用 `rust-testing` 技能
5. Unsafe Rust 与 FFI → 使用 `rust-unsafe-ffi` 技能
6. 宏系统 → 使用 `rust-macros` 技能
7. CLI 应用开发 → 使用 `rust-cli` 技能
8. Web 开发 → 使用 `rust-web` 技能
9. 嵌入式开发 → 使用 `rust-embedded` 技能

## 何时使用

- "用 Rust 写一个..."
- "Rust 的 X 语法是什么"
- "这个 Rust 代码怎么写"
- "标准库的 Y 怎么用"

## Data Privacy

本技能不收集、存储或传输任何用户数据。

---

## 一、变量与数据类型

### 1.1 变量绑定

```rust
let x = 5;                // 不可变绑定
let mut y = 10;           // 可变绑定
y += 5;

const MAX_POINTS: u32 = 100_000;  // 编译期常量
static APP_NAME: &str = "myapp";  // 静态变量

// 类型推断
let guess: u32 = "42".parse().expect("Not a number!");

// Shadowing（遮蔽）
let x = 5;
let x = x + 1;            // 新的 x 遮蔽旧的

// 类型别名
type Kilometers = i32;
```

### 1.2 标量类型

```rust
// 整数：i8, i16, i32, i64, i128, u8, u16, u32, u64, u128, isize, usize
let a: i8 = -128;
let b: u32 = 42;
// 字面量：0x1A（十六进制）、0o27（八进制）、0b1100（二进制）、b'A'（字节）

// 浮点：f32, f64
let pi = 3.14159;  // 默认 f64

// 布尔
let t: bool = true;

// 字符（4 字节 Unicode）
let c: char = '🦀';
```

### 1.3 复合类型

```rust
// 元组（Tuple）
let tup: (i32, f64, u8) = (500, 6.4, 1);
let (x, y, z) = tup;   // 解构
let first = tup.0;      // 索引访问

// 数组（Array — 固定长度，栈上）
let arr: [i32; 3] = [1, 2, 3];
let first = arr[0];

// 切片（Slice — 动态大小视图）
let slice: &[i32] = &arr[1..3];
let s: &str = "hello";  // 字符串切片
```

## 二、函数与控制流

### 2.1 函数定义

```rust
fn add(x: i32, y: i32) -> i32 {
    x + y                // 表达式（无分号）作为返回值
}

fn greet(name: &str) -> String {
    format!("Hello, {name}!")  // 或 return format!(...)
}

// 发散函数（never type）
fn never_returns() -> ! {
    panic!("this function never returns");
}
```

### 2.2 控制流

```rust
// if/else（表达式）
let number = if condition { 5 } else { 6 };

// 循环
loop {
    break;                // break 可带值：break 42;
}

let result = loop { break 42; };  // result = 42

while condition { /* ... */ }
for element in array.iter() { /* ... */ }
for (index, value) in (0..10).enumerate() { /* ... */ }

// 循环标签
'outer: loop {
    loop { break 'outer; }
}
```

### 2.3 模式匹配

```rust
match value {
    1 => println!("one"),
    2 | 3 => println!("two or three"),      // 或模式
    5..=10 => println!("range 5..=10"),     // 范围模式
    n @ 11..=20 => println!("between 11-20: {n}"),  // @ 绑定
    _ => println!("anything"),              // 通配符
}

// if let — 单分支匹配
if let Some(x) = optional { println!("{x}"); }

// while let
while let Some(top) = stack.pop() { /* ... */ }

// let-else（需要退出路径）
let Ok(value) = result else { return; };
```

## 三、所有权、借用与生命周期

### 3.1 所有权规则

```rust
// 1. 每个值有唯一所有者
// 2. 同一时间只有一个可变引用或任意多个不可变引用
// 3. 引用必须始终有效

let s = String::from("hello");
let t = s;               // s 被移动到 t，s 不再有效
// println!("{s}");      // 编译错误

let a = [1, 2, 3];
let b = a;               // i32 实现 Copy，a 仍有效（栈上数据）

// Clone（显式深拷贝）
let s1 = String::from("hello");
let s2 = s1.clone();
println!("{s1}, {s2}");  // 均有效
```

### 3.2 引用与借用

```rust
fn calculate_length(s: &String) -> usize { s.len() }   // 不可变借用
fn change(s: &mut String) { s.push_str(" world"); }    // 可变借用

let s = String::from("hello");
let r1 = &s;   // OK
let r2 = &s;   // OK — 多个不可变引用
// let r3 = &mut s;  // 错误！不可同时借用可变和不可变

let mut s = String::from("hello");
let r1 = &mut s;   // OK
// let r2 = &mut s; // 错误！只能有一个可变引用
```

### 3.3 切片

```rust
let s = String::from("hello world");
let hello = &s[0..5];       // 字符串切片 &str
let world = &s[6..11];

let a = [1, 2, 3, 4, 5];
let slice = &a[1..3];       // 数组切片 &[i32]
```

### 3.4 生命周期

```rust
// 生命周期注解
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}

// 省略规则：每个引用参数有自己的生命周期
// fn first_word(s: &str) -> &str  // 等价于 fn first_word<'a>(s: &'a str) -> &'a str

// 'static — 整个程序生命周期
let s: &'static str = "I live forever";

// 生命周期约束
struct Book<'a> {
    title: &'a str,
}

impl<'a> Book<'a> {
    fn read(&self) -> &str { self.title }  // 输出 = 输入的生命周期
}
```

## 四、结构体、枚举与模式匹配

### 4.1 结构体

```rust
// 具名结构体
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,       // 字段初始化简写
        email,
        sign_in_count: 1,
    }
}

// 结构体更新语法
let user2 = User {
    email: String::from("another@example.com"),
    ..user1             // 其余字段从 user1 移动
};

// 元组结构体
struct Color(i32, i32, i32);
let black = Color(0, 0, 0);

// 单元结构体
struct AlwaysEqual;
```

### 4.2 方法

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // 方法：&self 不可变借用
    fn area(&self) -> u32 {
        self.width * self.height
    }

    // 可变方法
    fn set_width(&mut self, w: u32) {
        self.width = w;
    }

    // 关联函数（无 self）
    fn square(size: u32) -> Self {
        Self { width: size, height: size }
    }
}
```

### 4.3 枚举

```rust
enum IpAddr {
    V4(u8, u8, u8, u8),
    V6(String),
}

enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(Color),
}

impl Message {
    fn call(&self) { /* ... */ }
}

// Option<T> — 无 null
enum Option<T> {
    None,
    Some(T),
}
```

### 4.4 深度模式匹配

```rust
// 解构结构体
let User { username, email, .. } = user;

// 解构枚举
match msg {
    Message::Quit => (),
    Message::Move { x, y } => println!("{x}, {y}"),
    Message::Write(text) => println!("{text}"),
    _ => (),
}

// @ 绑定
match num {
    n @ 1..=10 => println!("small: {n}"),
    n @ 11..=100 => println!("medium: {n}"),
    _ => println!("large"),
}

// 匹配守卫
match pair {
    (x, y) if x == y => println!("equal"),
    (x, y) if x + y == 0 => println!("opposites"),
    _ => (),
}
```

## 五、泛型与 Trait

### 5.1 泛型

```rust
// 泛型函数
fn largest<T: PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];
    for item in list {
        if item > largest { largest = item; }
    }
    largest
}

// 泛型结构体
struct Point<T> {
    x: T,
    y: T,
}

impl<T> Point<T> {
    fn x(&self) -> &T { &self.x }
}

// 泛型常量参数
fn nth_element<const N: usize>(arr: &[i32]) -> i32 {
    arr[N]
}
```

### 5.2 Trait

```rust
// 定义 trait
pub trait Summary {
    fn summarize(&self) -> String;

    // 默认实现
    fn summarize_default(&self) -> String {
        String::from("(Read more...)")
    }
}

// 实现 trait
pub struct Article {
    pub headline: String,
    pub location: String,
}

impl Summary for Article {
    fn summarize(&self) -> String {
        format!("{}, by {}", self.headline, self.location)
    }
}

// Trait bound
fn notify<T: Summary>(item: &T) {
    println!("{}", item.summarize());
}

// where 子句
fn notify2<T, U>(t: &T, u: &U)
where
    T: Summary + Clone,
    U: Summary + Debug,
{ /* ... */ }

// impl Trait 语法
fn notify3(item: &impl Summary) { /* ... */ }

// Trait 对象（动态分发）
let article: Box<dyn Summary> = Box::new(Article { /* ... */ });

// 关联类型
pub trait Iterator {
    type Item;
    fn next(&mut self) -> Option<Self::Item>;
}

// derive 宏
#[derive(Clone, Copy, Debug, PartialEq, Eq, Hash)]
struct MyStruct { /* ... */ }
```

## 六、集合与迭代器

### 6.1 Vec

```rust
let mut v: Vec<i32> = Vec::new();       // 空向量
let v = vec![1, 2, 3];                  // 宏

v.push(4);
let last = v.pop();                     // Option<T>
let third = &v[2];                      // 索引（越界 panic）
let third = v.get(2);                   // Option<&T>
```

### 6.2 String

```rust
let mut s = String::new();
let s = "hello".to_string();
let s = String::from("hello");
s.push_str(" world");
s.push('!');

// 拼接
let s = s1 + &s2;               // s1 被移动
let s = format!("{s1}-{s2}");   // 不移动所有权

// 字符串切片
let hello = &s[0..5];           // 注意 Unicode 边界
```

### 6.3 HashMap

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Yellow"), 50);

// Entry API
scores.entry(String::from("Blue")).or_insert(50);
```

### 6.4 迭代器

```rust
let v = vec![1, 2, 3];
let iter = v.iter();

// 适配器（惰性）
let result: Vec<_> = v.iter()
    .map(|x| x + 1)
    .filter(|x| x > &1)
    .collect();

// 消费者
let sum: i32 = v.iter().sum();
let count = v.iter().count();
let any_gt_2 = v.iter().any(|&x| x > 2);
```

## 七、错误处理

### 7.1 panic 与 Result

```rust
// 不可恢复错误
panic!("crash and burn");
let v = vec![1];
v[99];    // 运行时 panic

// 可恢复错误
use std::fs::File;
let f = File::open("hello.txt");
let f = match f {
    Ok(file) => file,
    Err(error) => panic!("Problem: {error:?}"),
};

// ? 运算符（推荐）
fn read_username() -> Result<String, io::Error> {
    let mut f = File::open("username.txt")?;
    let mut s = String::new();
    f.read_to_string(&mut s)?;
    Ok(s)
}
```

### 7.2 组合器

```rust
result.map(|v| v + 1)
      .map_err(|e| format!("error: {e}"))
      .and_then(|v| Ok(v * 2))
      .unwrap_or(0)
      .unwrap_or_else(|_| 42);

let x = optional.unwrap_or(0);
let y = optional.expect("value expected");
```

## 八、智能指针

```rust
let b = Box::new(5);                    // 堆分配
let rc = Rc::new(5);                    // 引用计数（单线程）
let arc = Arc::new(5);                  // 原子引用计数（多线程）

let cell = Cell::new(42);               // 内部可变性（Copy 类型）
let refcell = RefCell::new(5);          // 运行时借用检查

// Rc<RefCell<T>> 组合
let shared = Rc::new(RefCell::new(42));
*shared.borrow_mut() += 1;
```

## 九、闭包

```rust
let add_one = |x| x + 1;                       // 自动推断
let add = |a: i32, b: i32| -> i32 { a + b };   // 显式类型

// 环境捕获
let x = vec![1, 2, 3];
let equal_to_x = move |z| z == x;              // move 所有权
```

## 十、模块与可见性

```rust
// src/lib.rs — crate 根
pub mod front_of_house;

// src/front_of_house.rs
pub mod hosting;

// src/front_of_house/hosting.rs
pub fn add_to_waitlist() {}

// 路径
use crate::front_of_house::hosting;
use super::serve_order;           // 父模块
use std::collections::HashMap;    // 绝对路径

// 嵌套路径
use std::{cmp::Ordering, io};

// 重新导出
pub use crate::front_of_house::hosting;
```

## 十一、标准库常用模块速查

| 模块 | 关键类型/函数 | 用途 |
|------|-------------|------|
| `std::io` | `Read`, `Write`, `BufRead`, `stdin()`, `stdout()` | I/O |
| `std::fs` | `File`, `read_to_string`, `write`, `create_dir` | 文件系统 |
| `std::path` | `Path`, `PathBuf` | 路径操作 |
| `std::time` | `Duration`, `Instant`, `SystemTime` | 时间 |
| `std::env` | `args()`, `var()`, `current_dir()` | 环境 |
| `std::process` | `Command`, `ExitCode` | 进程 |
| `std::net` | `TcpListener`, `TcpStream`, `UdpSocket` | 网络 |

## 官方参考

- [The Rust Programming Language (The Book)](https://doc.rust-lang.org/book/)
- [Rust Standard Library API](https://doc.rust-lang.org/std/index.html)
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/)
- [Rust Reference](https://doc.rust-lang.org/reference/index.html)
