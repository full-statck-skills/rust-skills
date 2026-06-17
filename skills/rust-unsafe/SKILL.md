---
name: rust-unsafe
description: Rust 不安全代码与 FFI 技能 — 涵盖 unsafe 关键字、裸指针、union、FFI 外部函数接口、ABI、#[no_mangle]、bindgen、Pin、MaybeUninit、不安全 trait 实现等。基于 The Rustonomicon 与编译器源码。当用户需要编写 unsafe Rust、与 C 语言交互、操作裸内存时激活。
---

# Rust 不安全代码与 FFI

> 基于 [The Rustonomicon](https://doc.rust-lang.org/nomicon/) 与编译器源码（`compiler/` 中的 unsafety 检查模式）。

## Capability Boundaries

### ✅ 强项
1. unsafe 关键字（裸指针解引用、可变静态变量、union、FFI、不安全 trait/fn）
2. 裸指针（*const T、*mut T、NonNull<T>）
3. FFI 外部函数接口（extern "C"、#[link]、#[no_mangle]）
4. 内存操作（MaybeUninit、ManuallyDrop、Pin、transmute）
5. 不安全 trait 实现（Send、Sync 手动实现）
6. ABI 与类型布局（#[repr(C)]、#[repr(transparent)]、offset_of!）
7. 侵入式数据结构

### ⚠️ 前置要求
1. 深入理解 Rust 所有权与借用规则

### ❌ 不适用范围
1. 普通 Rust 编程 → 使用 `rust-core` 技能
2. 并发编程 → 使用 `rust-concurrency` 技能

## 何时使用

- "与 C 库交互"
- "实现不安全的数据结构"
- "操作裸指针"
- "优化热点路径（需要 unsafe）"

## Data Privacy

本技能不收集、存储或传输任何用户数据。

---

# Unsafe Rust 参考

## unsafe 的五种能力

```rust
// 1. 解引用裸指针
let mut x = 10;
let ptr: *mut i32 = &mut x;
unsafe { *ptr = 20; }

// 2. 调用 unsafe 函数
unsafe fn dangerous() {}
unsafe { dangerous(); }

// 3. 访问或修改可变静态变量
static mut COUNTER: u32 = 0;
unsafe { COUNTER += 1; }

// 4. 实现 unsafe trait
unsafe trait Foo {}
unsafe impl Foo for MyType {}

// 5. 访问 union 字段
union MyUnion { i: i32, f: f32 }
```

## FFI 外部函数接口

```rust
// 链接 C 库
#[link(name = "c")]
extern "C" {
    fn strlen(s: *const c_char) -> usize;
    static errno: i32;
}

// 导出 Rust 函数给 C
#[no_mangle]
pub extern "C" fn my_function(arg: i32) -> i32 {
    arg * 2
}

// ABI 约定
extern "C"      // C ABI
extern "stdcall" // Windows stdcall
extern "rust"    // Rust ABI（默认）

// 不安全 API 的安全封装
pub fn safe_strlen(s: &str) -> usize {
    use std::ffi::CString;
    let c_str = CString::new(s).expect("CString::new failed");
    unsafe { strlen(c_str.as_ptr()) }
}
```

## 类型布局控制

```rust
// C 兼容布局
#[repr(C)]
struct Point {
    x: f64,
    y: f64,
}

// 透明包装（字段类型和包装类型布局相同）
#[repr(transparent)]
struct Wrapper(Point);

// 指定对齐
#[repr(C, align(16))]
struct Aligned([u8; 16]);

// 固定大小
#[repr(i32)]
enum MyEnum {
    A = 0,
    B = 1,
    C = 2,
}
```

## 低级内存操作

```rust
use std::mem::{MaybeUninit, ManuallyDrop, transmute};

// MaybeUninit — 未初始化内存
let mut uninit: MaybeUninit<String> = MaybeUninit::uninit();
uninit.write("hello".to_string());
let initialized = unsafe { uninit.assume_init() };

// ManuallyDrop — 阻止 Drop
let md = ManuallyDrop::new(Box::new(42));
let ptr: *mut i32 = &mut **md;  // 防止 Box 被释放

// transmute — 类型转换
let bytes: [u8; 4] = unsafe { transmute::<f32, [u8; 4]>(1.0f32) };
```

## 不安全 trait 实现

```rust
// 手动实现 Send/Sync
struct MyType(*const u8);

// 编译器无法自动推导，但可以安全地断言
unsafe impl Send for MyType {}
unsafe impl Sync for MyType {}
```

## 官方参考

- [The Rustonomicon](https://doc.rust-lang.org/nomicon/)
- [std::mem](https://doc.rust-lang.org/std/mem/)
- [std::ptr](https://doc.rust-lang.org/std/ptr/)
