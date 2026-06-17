---
name: rust-concurrency
description: Rust 并发编程技能 — 涵盖 async/await、std::thread、std::sync（Mutex、RwLock、Barrier、Condvar、OnceLock、LazyLock）、std::sync::atomic、mpsc/mpmc 通道、Send/Sync、Tokio 生态。基于官方 std::sync 与 thread 模块源码分析。当用户需要编写多线程、异步 I/O 或并发代码时激活。
---

# Rust 并发编程

> 基于官方 `library/std/src/sync/` 与 `library/std/src/thread/` 模块源码分析。

## Capability Boundaries

### ✅ 强项
1. OS 线程（std::thread::spawn、Builder、JoinHandle）
2. 同步原语（Mutex、RwLock、Barrier、Condvar、OnceLock、LazyLock）
3. 原子操作（std::sync::atomic 所有原子类型）
4. 通道通信（mpsc 多生产者单消费者、mpmc 多生产者多消费者）
5. Send/Sync trait 系统
6. async/await 基础
7. 线程局部存储（thread_local!）

### ⚠️ 前置要求
1. 理解 Rust 所有权模型

### ❌ 不适用范围
1. Rust 基础语法 → 使用 `rust-core` 技能

## 何时使用

- "用多线程处理数据"
- "实现生产者-消费者模式"
- "使用 async/await"
- "线程间共享数据"
- "避免数据竞争"

## Data Privacy

本技能不收集、存储或传输任何用户数据。

---

# Rust 并发参考

## 线程（std::thread）

```rust
use std::thread;

// 基础线程
let handle = thread::spawn(move || {
    println!("Hello from thread!");
});

// 等待线程完成
handle.join().unwrap();

// 配置线程
let builder = thread::Builder::new()
    .name("worker".into())
    .stack_size(1024 * 1024);

let handle = builder.spawn(move || { /* ... */ }).unwrap();

// 获取当前线程
let current = thread::current();
println!("Thread name: {:?}", current.name());
```

## 同步原语（std::sync）

```rust
use std::sync::{Arc, Mutex, RwLock, Barrier, OnceLock, LazyLock};

// Mutex — 互斥锁
let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];

for _ in 0..10 {
    let counter = Arc::clone(&counter);
    handles.push(thread::spawn(move || {
        let mut num = counter.lock().unwrap();
        *num += 1;
    }));
}

// RwLock — 读写锁（多读单写）
let data = Arc::new(RwLock::new(vec![1, 2, 3]));
{
    let read = data.read().unwrap();
    println!("len: {}", read.len());
}
{
    let mut write = data.write().unwrap();
    write.push(4);
}

// Barrier — 屏障（同步多个线程）
let barrier = Arc::new(Barrier::new(5));

// OnceLock — 懒初始化（线程安全）
static CONFIG: OnceLock<String> = OnceLock::new();
let config = CONFIG.get_or_init(|| load_config());

// LazyLock — 惰性初始化
static CACHE: LazyLock<HashMap<String, Data>> = LazyLock::new(|| {
    HashMap::new()
});
```

## 通道（mpsc、mpmc）

```rust
use std::sync::mpsc;

// 多生产者，单消费者
let (tx, rx) = mpsc::channel();

thread::spawn(move || {
    tx.send(42).unwrap();
});

assert_eq!(rx.recv().unwrap(), 42);
```

## 原子操作

```rust
use std::sync::atomic::{
    AtomicU64, AtomicBool, Ordering
};

static COUNTER: AtomicU64 = AtomicU64::new(0);

// 原子递增
let prev = COUNTER.fetch_add(1, Ordering::SeqCst);

// 原子交换（可见于其他线程的标志位）
static READY: AtomicBool = AtomicBool::new(false);
READY.store(true, Ordering::Release);
let is_ready = READY.load(Ordering::Acquire);
```

## Send / Sync

```rust
// Send: 类型所有权可跨线程转移
// Sync: 类型引用 &T 可跨线程共享

// 编译器自动推导，但可手动实现（需要 unsafe）
unsafe impl Send for MyType {}
unsafe impl Sync for MyType {}
```

## 官方参考

- [std::thread](https://doc.rust-lang.org/std/thread/)
- [std::sync](https://doc.rust-lang.org/std/sync/)
- [std::sync::atomic](https://doc.rust-lang.org/std/sync/atomic/)
