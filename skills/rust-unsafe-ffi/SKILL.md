---
name: rust-unsafe-ffi
description: Rust 不安全代码与 FFI 技能 — unsafe 五种超能力（裸指针、unsafe fn、可变静态变量、unsafe trait、union）、裸指针操作与 NonNull、FFI 外部函数接口（extern "C"、#[link]、#[no_mangle]）、内存操作（MaybeUninit、ManuallyDrop、transmute、offset_of!）、类型布局控制（#[repr]）、Pin、全局分配器。基于 The Book ch 19 与 The Rustonomicon。当用户需要与 C 交互、编写 unsafe 代码或操作裸内存时激活。
---

# Rust 不安全代码与 FFI

> 基于 The Rust Programming Language ch 19.1-19.2 与 [The Rustonomicon](https://doc.rust-lang.org/nomicon/)。

## Capability Boundaries

### ✅ 强项
1. unsafe 五种超能力（裸指针解引用、unsafe fn/方法、可变静态变量、unsafe trait、union 字段）
2. 裸指针（*const T、*mut T、NonNull<T>、偏移运算 add/offset、地址运算）
3. FFI（extern "C" ABI、#[link]、#[no_mangle]、C 类型映射、回调函数）
4. 内存操作（MaybeUninit<T>、ManuallyDrop<T>、transmute、offset_of!、size_of/align_of）
5. 类型布局控制（#[repr(C)]、#[repr(transparent)]、#[repr(align)]、#[repr(packed)]、#[repr(i32)]）
6. Pin<T>（自引用类型安全约束、Pin<Box<T>>、Pin<&mut T>）
7. 全局分配器自定义（#[global_allocator] + GlobalAlloc trait）
8. 不安全 trait 手动实现（Send、Sync）

### ⚠️ 前置要求
1. 深入理解 Rust 所有权与借用规则（rust-1.93）

### ❌ 不适用范围
1. 常规 Rust 编程 → 使用 `rust-1.93` 技能
2. 并发 unsafe 使用 → 使用 `rust-concurrency` 技能

## 何时使用

- "与 C 库交互"
- "unsafe 代码怎么写才安全"
- "transmute / MaybeUninit 使用"
- "类型布局控制"
- "Pin / 自引用结构"

## Data Privacy

本技能不收集、存储或传输任何用户数据。

---

## 一、unsafe 五种能力

```rust
// 1. 解引用裸指针
let mut x = 10;
let ptr: *mut i32 = &mut x;
unsafe { *ptr = 20; }

// 2. 调用 unsafe 函数
unsafe fn dangerous() {}
unsafe { dangerous(); }

// 3. 访问/修改可变静态变量
static mut COUNTER: u32 = 0;
unsafe { COUNTER += 1; }

// 4. 实现 unsafe trait
unsafe trait Foo {}
unsafe impl Foo for MyType {}

// 5. 访问 union 字段
union IntOrFloat { i: i32, f: f32 }
let u = IntOrFloat { f: 1.0 };
unsafe { let i = u.i; }
```

## 二、裸指针

```rust
// 创建
let mut x = 10;
let ptr: *const i32 = &x as *const i32;
let ptr_mut: *mut i32 = &mut x as *mut i32;

// 安全抽象
use std::ptr::NonNull;
let n: NonNull<i32> = NonNull::new(&mut x as *mut i32).unwrap();

// 偏移运算
let arr = [1, 2, 3];
let ptr = arr.as_ptr();
unsafe {
    assert_eq!(*ptr, 1);
    assert_eq!(*ptr.add(1), 2);
    assert_eq!(*ptr.offset(2), 3);
}

// 常用函数
std::ptr::read(src);      // 读指针处的值
std::ptr::write(dst, v);  // 写值到指针处
std::ptr::swap(a, b);     // 交换两个指针的值
std::ptr::drop_in_place(p);  // 原地 drop
```

## 三、FFI

```rust
use std::ffi::{CStr, CString};
use std::os::raw::{c_char, c_int, c_void};

// 调用 C 函数
#[link(name = "c")]
extern "C" {
    fn strlen(s: *const c_char) -> usize;
    fn malloc(size: usize) -> *mut c_void;
}

// 导出函数给 C
#[no_mangle]
pub extern "C" fn add(a: i32, b: i32) -> i32 { a + b }

// C 字符串安全封装
fn safe_strlen(s: &str) -> usize {
    let c_str = CString::new(s).expect("CString::new failed");
    unsafe { strlen(c_str.as_ptr()) }
}

// 回调函数
extern "C" fn callback(data: *mut c_void) {
    println!("callback called");
}
```

## 四、内存操作

```rust
use std::mem::{MaybeUninit, ManuallyDrop, transmute, offset_of, size_of};

// MaybeUninit — 未初始化内存
let mut uninit: MaybeUninit<String> = MaybeUninit::uninit();
uninit.write("hello".to_string());
let s = unsafe { uninit.assume_init() };

// 数组初始化
let mut arr: MaybeUninit<[u8; 1024]> = MaybeUninit::uninit();
let arr = unsafe { arr.assume_init() };

// ManuallyDrop — 防止 Drop
let md = ManuallyDrop::new(Box::new(42));
let ptr = &**md as *const i32;  // Box 不会被释放

// transmute — 类型转换
let bits: u32 = unsafe { transmute::<f32, u32>(1.0f32) };
```

## 五、类型布局控制

```rust
// C 兼容布局
#[repr(C)]
struct Point { x: f64, y: f64 }

// 透明包装
#[repr(transparent)]
struct Wrapper(Point);  // 内存布局与 Point 完全相同

// 对齐
#[repr(C, align(16))]
struct AlignedData([u8; 16]);

// 紧凑
#[repr(C, packed)]
struct Packed { x: u8, y: u32 }

// 枚举大小
#[repr(i32)]
enum MyEnum { A = 0, B = 1, C = 2 }

// offset_of! (1.77+)
struct S { a: u8, b: u32 }
assert_eq!(offset_of!(S, b), 4);  // 考虑对齐
```

## 六、Pin

```rust
use std::pin::Pin;

// Pin<Box<T>> — 防止值在内存中移动
let pinned = Pin::new(Box::new(42));
// let moved = *pinned;  // 编译错误

// 自引用结构安全模式
struct SelfRef {
    data: String,
    ptr: *const String,
}

impl SelfRef {
    fn new(data: String) -> Pin<Box<Self>> {
        let mut b = Box::new(SelfRef {
            ptr: std::ptr::null(),
            data,
        });
        b.ptr = &b.data;
        Pin::new(b)
    }
}
```

## 七、全局分配器

```rust
use std::alloc::{GlobalAlloc, Layout, System};

struct MyAllocator;

unsafe impl GlobalAlloc for MyAllocator {
    unsafe fn alloc(&self, layout: Layout) -> *mut u8 {
        System.alloc(layout)
    }
    unsafe fn dealloc(&self, ptr: *mut u8, layout: Layout) {
        System.dealloc(ptr, layout)
    }
}

#[global_allocator]
static ALLOCATOR: MyAllocator = MyAllocator;
```

## 官方参考

- [The Rustonomicon](https://doc.rust-lang.org/nomicon/)
- [std::ptr](https://doc.rust-lang.org/std/ptr/)
- [std::mem](https://doc.rust-lang.org/std/mem/)
- [std::ffi](https://doc.rust-lang.org/std/ffi/)
