---
name: rust-1.93
description: Up-to-date Rust 1.93.1 language and standard library skill. Use when writing, reviewing, debugging, or migrating Rust code, working with std modules, ownership/lifetimes, traits/generics, collections, error handling, smart pointers, closures, iterators, modules, testing, and the standard library (Vec, String, HashMap, Option, Result, Box, Rc, Arc, Cell, RefCell, Path, I/O). Based on official rust-lang/rust source analysis.
---

# Rust Language Reference (v1.93.1)

> Rust programming language aggregate primary skill. Based on the official `rust-lang/rust` source code (`library/` and `compiler/`) — 11,516 `.rs` files across the standard library and compiler.

## Capability Boundaries

### ✅ Strong Suits
1. Variables & data types — scalars, compounds, type inference, const/static, shadowing
2. Control flow — if/else, loop/while/for, break/continue/label, match/if let/while let/let else
3. Functions & closures — definitions, parameters, return values, closure captures (Fn/FnMut/FnOnce)
4. Ownership, borrowing, lifetimes — ownership rules, &T/&mut T, slices, 'a lifetime elision, 'static
5. Structs, enums, pattern matching — struct/tuple struct/unit struct, enum with data, deep match destructuring
6. Generics & traits — generic types/functions, trait definition/bounds, impl/dyn Trait, associated types, supertrait, derive, orphan rule
7. Collections & iterators — Vec, String, HashMap, HashSet, Iterator trait, adapters (map/filter/fold/collect)
8. Error handling — panic, Result/Option, ? operator, combinators, custom errors, anyhow/thiserror
9. Smart pointers — Box, Rc, Arc, Cell, RefCell, Deref/Drop, OnceCell, LazyCell
10. Modules & visibility — mod, pub, use, crate::/self::/super:: paths, re-export, external crates
11. Testing basics — #[test], assert!/assert_eq!, #[should_panic], #[cfg(test)]
12. Common std library modules — std::io, std::fs, std::path, std::time, std::env, std::process, std::net
13. Attributes — #[derive], #[cfg], #[repr], #[must_use], #[allow/deny]
14. Formatting — format string syntax, Display/Debug, pretty-printing {:#?}

### ⚠️ Requirements
1. Verify Rust toolchain (`rustc --version` ≥ 1.93.0)
2. Verify Cargo available (`cargo --version`)

### ❌ Out of Scope (with alternatives)
1. Project scaffolding & modules depth → use `rust-project-structure`
2. Cargo build configuration & dependency management → use `rust-cargo-build`
3. Concurrency & async programming depth → use `rust-concurrency`
4. Testing & benchmarking depth → use `rust-testing`
5. Unsafe Rust & FFI → use `rust-unsafe-ffi`
6. Macro system → use `rust-macros`
7. CLI application development → use `rust-cli`
8. Web development (axum, serde, databases) → use `rust-web`
9. Embedded & no_std development → use `rust-embedded`
10. Code review & style → use `rust-code-review` / `rust-style-clippy`

## When to use this skill

Use this skill when the user needs to write, review, debug, or migrate Rust 1.93 code, or when working with standard library types, ownership/lifetimes, trait systems, collections, error handling, smart pointers, closures, iterators, module systems, testing, or common std lib modules.

## Data Privacy

This skill does not collect, store, or transmit any user data. All code examples are for local development reference only.

## Official Positioning

From the official Rust docs, Rust is a systems programming language focused on **safety, speed, and concurrency**:

- **Memory safety without GC** — ownership and borrowing enforced at compile time
- **Zero-cost abstractions** — high-level constructs → efficient machine code
- **Fearless concurrency** — type system prevents data races at compile time
- **Practical tooling** — Cargo, rustfmt, Clippy, rustdoc, rust-analyzer
- **Expressive type system** — algebraic data types, pattern matching, generics, traits

### Primary official sources
- [The Rust Programming Language (The Book)](https://doc.rust-lang.org/book/)
- [Rust Standard Library API](https://doc.rust-lang.org/std/index.html)
- [Rust Reference](https://doc.rust-lang.org/reference/index.html)
- [Rust by Example](https://doc.rust-lang.org/rust-by-example/)
- [Cargo Book](https://doc.rust-lang.org/cargo/index.html)
- [Rust RFCs](https://rust-lang.github.io/rfcs/)
- [Rust Edition Guide](https://doc.rust-lang.org/edition-guide/)

## Quick Start

**Example invocations:**
```
Write a Rust function that reads a CSV file and returns Vec<Record>
Review this Rust code for lifetime issues
Use HashMap with a custom key type
Implement the Iterator trait for a custom collection
Handle errors properly with ? and custom error types
Format this struct with Display and Debug traits
```

## Critical: How to use this skill

1. **Confirm the version first** — Run `rustc --version`; this skill targets Rust 1.93.1
2. **Start here** for language fundamentals, std library types, basic I/O, testing, error handling
3. **Load references/files** for detailed module guidance (see Standard Library References section)
4. **Load examples/files** for copyable snippets (see Offline Examples section)
5. **Switch to domain skills** for specialized work (concurrency, unsafe, macros, CLI, Web, embedded)

## Critical: What's New in Rust 1.93

Rust 1.93 (February 2026) stabilises these notable features:

### New stable features
- **`offset_of!`** — Compute struct field offsets at compile time: `offset_of!(MyStruct, field)`
- **`LazyCell`** and **`LazyLock`** — Lazy initialization wrappers (previously unstable)
- **`derive_const`** — Support for `const` implementations via derive
- **`iter_next_chunk`** — `Iterator::next_chunk::<N>()` gets fixed-size arrays
- **`associated_type_defaults`** — Default types for associated types
- **`const_precise_live_drops`** — More precise const fn drop tracking

### Breaking changes
- **`unsafe_op_in_unsafe_fn` is now a hard error** — All unsafe operations in `unsafe fn` must be in explicit `unsafe { }` blocks
- **Edition 2024 is now stable** — Default for new projects; includes improved `impl Trait` capture rules

## Critical: The Rust Book Coverage Map

| Book Chapter | Coverage Here | Domain Skill |
|-------------|--------------|-------------|
| ch 1-2: Getting Started / Guessing Game | ✅ Quick Start | - |
| ch 3: Common Programming Concepts | ✅ Variables, types, functions, control flow | - |
| ch 4: Ownership | ✅ Ownership, borrowing, slices | - |
| ch 5: Structs | ✅ Structs, methods, associated functions | - |
| ch 6: Enums & Pattern Matching | ✅ Enum, match, if let, while let | - |
| ch 7: Modules | ✅ Basics | `rust-project-structure` |
| ch 8: Common Collections | ✅ Vec, String, HashMap | - |
| ch 9: Error Handling | ✅ Result, panic, ?, custom errors | - |
| ch 10: Generics, Traits, Lifetimes | ✅ Generics, traits, lifetimes | - |
| ch 11: Testing | ✅ Basics | `rust-testing` |
| ch 12: I/O Project | ✅ File I/O basics | `rust-cli` |
| ch 13: Functional Features | ✅ Closures, iterators | - |
| ch 14: Cargo & Crates.io | ✅ Basics | `rust-cargo-build` |
| ch 15: Smart Pointers | ✅ Box, Rc, Arc, Cell, RefCell | - |
| ch 16: Concurrency | ❌ Basics only | `rust-concurrency` |
| ch 17: OOP Features | ✅ Trait objects | - |
| ch 18: Patterns & Matching | ✅ Pattern syntax | - |
| ch 19: Advanced Features | ❌ | `rust-unsafe-ffi`, `rust-macros` |
| ch 20: Web Server | ❌ | `rust-web`, `rust-concurrency` |

## Critical: Standard Library Coverage Map

### Core (no_std, always available)
| Module | Key Types / Traits |
|--------|-------------------|
| `core::option` | `Option<T>` |
| `core::result` | `Result<T, E>` |
| `core::iter` | `Iterator`, `IntoIterator`, `FromIterator` |
| `core::clone` | `Clone` |
| `core::cmp` | `PartialEq`, `Eq`, `PartialOrd`, `Ord`, `Ordering` |
| `core::ops` | `Deref`, `Drop`, `Add`, `Index`, `Range`, `ControlFlow`, `Try` |
| `core::fmt` | `Display`, `Debug`, `Formatter`, `write!` |
| `core::convert` | `From`, `Into`, `TryFrom`, `TryInto`, `AsRef`, `AsRef` |
| `core::mem` | `size_of`, `align_of`, `replace`, `swap`, `MaybeUninit`, `ManuallyDrop` |
| `core::cell` | `Cell`, `RefCell`, `OnceCell`, `LazyCell` |
| `core::marker` | `Copy`, `Send`, `Sync`, `Sized`, `PhantomData` |
| `core::ptr` | `NonNull`, `null`, `addr_of!`, `addr_of_mut!` |
| `core::slice` | `[T]` methods: `sort`, `binary_search`, `split`, `windows` |
| `core::str` | `str` methods: `trim`, `contains`, `replace`, `split` |
| `core::hash` | `Hash`, `Hasher`, `BuildHasher` |
| `core::future` | `Future`, `IntoFuture` |
| `core::pin` | `Pin`, `Pin<&mut T>` |

### Alloc (requires allocator)
| Module | Key Types / Traits |
|--------|-------------------|
| `alloc::boxed` | `Box<T>`, `Box<[T]>` |
| `alloc::rc` | `Rc<T>`, `Weak<T>` |
| `alloc::sync` | `Arc<T>`, `Weak<T>` |
| `alloc::vec` | `Vec<T>` |
| `alloc::string` | `String` |
| `alloc::collections` | `VecDeque`, `LinkedList`, `BinaryHeap`, `BTreeMap`, `BTreeSet` |

### Std (full, platform-dependent)
| Module | Key Types / Functions |
|--------|----------------------|
| `std::io` | `Read`, `Write`, `BufRead`, `BufReader`, `BufWriter`, `Cursor`, `stdin`, `stdout`, `stderr` |
| `std::fs` | `File`, `read_to_string`, `read_dir`, `metadata`, `create_dir`, `remove_dir_all` |
| `std::path` | `Path`, `PathBuf` |
| `std::net` | `TcpListener`, `TcpStream`, `UdpSocket`, `IpAddr`, `SocketAddr` |
| `std::time` | `Duration`, `Instant`, `SystemTime` |
| `std::env` | `args`, `var`, `current_dir`, `temp_dir`, `home_dir` |
| `std::process` | `Command`, `ExitCode`, `Stdio` |
| `std::thread` | `spawn`, `Builder`, `JoinHandle`, `scope`, `sleep` |
| `std::sync` | `Mutex`, `RwLock`, `Barrier`, `Condvar`, `OnceLock`, `LazyLock`, `mpsc` |
| `std::sync::atomic` | `AtomicBool`, `AtomicI32`, `AtomicUsize`, `Ordering` |
| `std::ffi` | `OsStr`, `OsString`, `CStr`, `CString` |
| `std::error` | `Error` trait |
| `std::collections` | `HashMap`, `HashSet` (re-exported from alloc) |

## Language Reference (Essential Patterns)

### Variables & Data Types

```rust
let x = 5;                          // immutable
let mut y = 10;                     // mutable
const MAX: u32 = 100_000;           // compile-time constant
static APP: &str = "myapp";         // static variable

// Scalar types — i8-u128, f32-f64, bool, char
let i: i32 = -42;
let pi: f64 = std::f64::consts::PI;
let flag: bool = true;
let c: char = '🦀';                 // 4-byte Unicode

// Compound types — tuple, array, slice
let tup: (i32, f64, u8) = (500, 6.4, 1);
let (x, _, z) = tup;                // destructure, ignore middle

let arr: [i32; 3] = [1, 2, 3];
let first: &i32 = &arr[0];          // index (panics if OOB)
let first_safe: Option<&i32> = arr.get(0);  // safe index

let slice: &[i32] = &arr[0..2];     // view into array
let s: &str = "hello world";        // string slice
let hello = &s[0..5];               // caution: Unicode boundaries
```

### Functions & Control Flow

```rust
fn add(x: i32, y: i32) -> i32 { x + y }   // last expression = return
fn greet(name: &str) -> String {
    format!("Hello, {name}!")
}
fn never_returns() -> ! { panic!("never"); }

// if is an expression
let number = if condition { 5 } else { 6 };

// loop with break value
let result = loop { break 42; };
'outer: for i in 0..10 {
    for j in 0..10 {
        if i + j == 15 { break 'outer; }
    }
}

// match — exhaustive
match value {
    1 => println!("one"),
    2 | 3 => println!("two or three"),
    5..=10 => println!("range inclusive"),
    n @ 11..=20 => println!("bound: {n}"),
    _ => println!("catch-all"),
}

// Pattern matching
if let Some(x) = optional { println!("{x}"); }
while let Some(top) = stack.pop() { /* ... */ }
let Ok(v) = risky() else { return Err("fail"); };
```

### Ownership & Borrowing

```rust
// Ownership rules:
// 1. Each value has one owner at a time
// 2. Either multiple &T or one &mut T
// 3. References must always be valid

let s = String::from("hello");
let t = s;                           // moved — s no longer valid

let a = [1, 2, 3];                   // i32: Copy — a still valid
let b = a;

let s1 = String::from("hello");
let s2 = s1.clone();                 // explicit deep copy

// Borrowing (&T — immutable, &mut T — mutable, exclusive)
fn len(s: &String) -> usize { s.len() }
fn push(s: &mut String, c: char) { s.push(c); }

// Lifetimes
fn longest<'a>(x: &'a str, y: &'a str) -> &'a str {
    if x.len() > y.len() { x } else { y }
}
// Elision rule: fn first_word(s: &str) -> &str  →  fn first_word<'a>(s: &'a str) -> &'a str

// 'static — lives for entire program
let s: &'static str = "hello";

// Lifetime bounds on struct
struct Book<'a> { title: &'a str }
impl<'a> Book<'a> { fn read(&self) -> &str { self.title } }
```

### Structs, Enums & Pattern Matching

```rust
struct User { active: bool, username: String, email: String, sign_in_count: u64 }

// Field init shorthand + update syntax
fn new_user(email: String) -> User {
    User { email, username: email, active: true, sign_in_count: 1 }
}
let u2 = User { email: String::from("b@c"), ..u1 };  // partial move

// Tuple struct / Unit struct
struct Color(i32, i32, i32);
struct AlwaysEqual;

// Methods — impl blocks
impl Rectangle {
    fn area(&self) -> u32 { self.width * self.height }           // &self
    fn set_width(&mut self, w: u32) { self.width = w; }          // &mut self
    fn square(size: u32) -> Self { Self { width: size, height: size } }  // associated fn
}

// Enum with data
enum Message {
    Quit, Move { x: i32, y: i32 }, Write(String), ChangeColor(i32, i32, i32),
}

// Deep pattern matching
let User { username, email, .. } = user;
match msg {
    Message::Quit => (),
    Message::Move { x, y } => println!("{x}, {y}"),
    Message::Write(text) => println!("{text}"),
    Message::ChangeColor(r, g, b) => (),
}
```

### Generics & Traits

```rust
pub trait Summary {
    fn summarize(&self) -> String;
    fn default_summary(&self) -> String { String::from("(Read more...)") }  // default impl
}

pub struct Article { pub headline: String, pub location: String }
impl Summary for Article {
    fn summarize(&self) -> String { format!("{}, by {}", self.headline, self.location) }
}

// Trait bounds — 3 syntaxes
fn notify1<T: Summary>(item: &T) { item.summarize(); }
fn notify2(item: &impl Summary) { item.summarize(); }                    // sugar
fn notify3<T, U>(t: &T, u: &U) where T: Summary + Clone, U: Debug, {}

// Trait objects (dynamic dispatch)
let trait_obj: Box<dyn Summary> = Box::new(Article { /* ... */ });

// Associated types
pub trait Iterator { type Item; fn next(&mut self) -> Option<Self::Item>; }

// Derive macros
#[derive(Clone, Copy, Debug, PartialEq, Eq, Hash)]
struct Point { x: i32, y: i32 }

// Generic const parameters
fn first_n<const N: usize>(arr: &[i32]) -> [i32; N] {
    let mut result = [0; N];
    for (i, v) in arr.iter().take(N).enumerate() { result[i] = *v; }
    result
}
```

### Collections

```rust
let mut v = vec![1, 2, 3];
v.push(4);
let last = v.pop();              // Option<i32>
let third = v.get(2);            // Option<&i32>
v.sort();
v.dedup();
v.retain(|x| *x > 0);

let mut s = String::from("hello");
s.push_str(" world");
let combined = format!("{}-{}", a, b);  // no move like + does

let mut map = HashMap::new();
map.insert("key".to_string(), 42);
map.entry("key".to_string()).or_insert(0);    // Entry API
for (k, v) in &map { println!("{k}: {v}"); }
```

### Iterators

```rust
let v = vec![1, 2, 3, 4, 5];

// Adapters (lazy)
let evens_squared: Vec<_> = v.iter()
    .filter(|x| *x % 2 == 0)
    .map(|x| x * x)
    .collect();

// Consumers
let sum: i32 = v.iter().sum();
let product: i32 = v.iter().product();
let count = v.iter().count();
let any = v.iter().any(|x| x > &3);
let all = v.iter().all(|x| x > &0);
let max = v.iter().max();
let folded = v.iter().fold(0, |acc, x| acc + x);

// next_chunk (1.93+)
let [a, b]: [i32; 2] = v.iter().copied().next_chunk().unwrap();
```

### Error Handling

```rust
// ? operator — propagate errors
fn read_file(path: &str) -> io::Result<String> {
    let mut f = File::open(path)?;
    let mut contents = String::new();
    f.read_to_string(&mut contents)?;
    Ok(contents)
}

// Combinators
result.map(|v| v + 1).map_err(|e| format!("{e}")).and_then(|v| Ok(v * 2));
let x = optional.unwrap_or(0);
let x = optional.unwrap_or_else(|| expensive_default());
let x = result.expect("must succeed");
let x = result.unwrap_or_default();

// Custom error types with thiserror
use thiserror::Error;
#[derive(Error, Debug)]
pub enum MyError {
    #[error("network error: {0}")]
    Network(#[from] io::Error),
    #[error("invalid data: {msg}")]
    Parse { msg: String },
}

// anyhow for application code
use anyhow::{Result, Context};
fn do_work() -> Result<()> {
    let data = read_file("config.toml").context("failed to read config")?;
    Ok(())
}
```

### Smart Pointers

```rust
let b = Box::new(5);               // heap allocated
let rc = Rc::new(5);               // reference counted (single-threaded)
let arc = Arc::new(5);             // atomic reference counted (multi-threaded)

// Interior mutability
let cell = Cell::new(42);          // for Copy types: get, set, replace
let val = cell.get();
cell.set(100);

let refcell = RefCell::new(vec![1, 2, 3]);
refcell.borrow_mut().push(4);      // runtime borrow checking — panics on violation

// Rc<RefCell<T>> — shared mutable state (single-threaded)
let shared = Rc::new(RefCell::new(42));
*shared.borrow_mut() += 1;
```

### Modules & Visibility

```rust
// lib.rs — crate root
pub mod front_of_house;        // loads front_of_house.rs or front_of_house/mod.rs
mod back_of_house;             // private to crate

// Paths
use crate::front_of_house::hosting;
use super::serve_order;
use std::collections::HashMap;

// Nested / glob / alias
use std::{cmp::Ordering, io::{self, Write}};
use std::collections::*;       // glob — use sparingly
use std::result::Result as StdResult;

// Re-export
pub use crate::front_of_house::hosting;
```

### Common Attributes

```rust
#[derive(Debug, Clone, Copy, PartialEq, Eq, Hash)]
#[repr(C)]                          // C-compatible layout
#[repr(transparent)]                // newtype wrapper, same layout as inner type
#[repr(align(16))]                  // 16-byte alignment
#[cfg(target_os = "linux")]
#[must_use = "this result must be used"]
#[allow(clippy::needless_return)]
#[deny(unsafe_op_in_unsafe_fn)]     // hard error in 1.93
```

## Quick Fixes

| Error Code / Pattern | Likely Cause | Fix |
|---------------------|-------------|-----|
| `E0308: mismatched types` | Type mismatch | Check expected vs actual type; add explicit annotation |
| `E0502: cannot borrow as mutable` | Simultaneous &T + &mut T | Restructure borrow scope; clone if needed |
| `E0597: borrowed value does not live long enough` | Reference outlives source | Extend source lifetime, return owned value |
| `E0277: trait bound not satisfied` | Missing trait impl | Add `#[derive(...)]` or `impl Trait for Type` |
| `E0432: unresolved import` | Wrong module path | Check path; use `crate::`, `super::`, absolute path |
| `E0382: use of moved value` | Value moved, then reused | Clone before move, or restructure code |
| `E0499: cannot borrow as mutable more than once` | Multiple &mut refs | Use single &mut, split with scopes |
| `E0106: missing lifetime specifier` | Lifetime needed | Add `<'a>` to fn signature or struct |
| `E0716: temporary value dropped` | Reference to temporary | Store temporary in a let binding first |
| `E0061: wrong number of arguments` | Wrong call | Check function signature |
| `E0133: use of unsafe` | Unsafe call without block | Wrap in `unsafe { ... }` |
| `cannot use `self` in associated function` | Used `&self` on non-method | Requires a first parameter of `&self`, `&mut self`, or `self` |
| `format argument must be captured` | Edition mismatch | Use `"{var}"` style (2021+) or `"{}", var` |

## Common Anti-Patterns

| Anti-Pattern | Problem | Better Approach |
|-------------|---------|----------------|
| `value.clone()` before reading | Unnecessary allocation | Use `&value` |
| `.unwrap()` everywhere | Panics on error | Use `?` or `.ok_or(...)?` |
| `String` + `&str` in hot loop | Repeated allocations | Use `format!` or `join` |
| `Box::new(42)` for simple values | Unnecessary heap allocation | Stack-allocate directly |
| Large `enum` with big variants | Stack copy overhead | Box the large variant(s) |
| `std::mem::forget()` to avoid drop | Unsound | Use `ManuallyDrop` instead |
| Long `Mutex` lock scope | Lock contention | `{ let guard = lock.lock(); ... }` |
| Runtime `RefCell` panic | Unexpected borrow | Check with `try_borrow_mut()` |

## Gotchas (Rust 1.93)

1. **`unsafe_op_in_unsafe_fn` is now a hard error** — All unsafe operations must be in explicit `unsafe { }`, even inside `unsafe fn`
2. **`let else` requires a diverging else branch** — The `else` must be `return`, `break`, `panic!`, etc.
3. **`impl Trait` in argument position is sugar** — `fn f(x: impl T)` = `fn f<X: T>(x: X)`
4. **`dyn Trait` is not `Sized`** — Must be behind pointer (`&dyn`, `Box<dyn>`, etc.)
5. **`self` vs `Self`** — `self` is the method receiver; `Self` is the implementing type alias
6. **`..` vs `..=`** — `0..5` yields 0,1,2,3,4; `0..=5` yields 0,1,2,3,4,5
7. **String indexing by byte is error-prone** — `&s[i..j]` panics if not on char boundary; prefer `.chars()`
8. **`?` requires matching return type** — Only works inside functions returning `Result` or `Option`
9. **`#[derive(Debug)]` requires all fields are `Debug`** — Private fields of non-Debug types cause errors
10. **Edition 2021+ closure capture** — Closures capture only the fields they use, not the whole struct

## FAQ

**Q: What Rust edition should I use?**
A: The latest stable edition is **2024** (set `edition = "2024"` in Cargo.toml). See `rust-style-clippy` for migration guidance from 2015/2018/2021.

**Q: How does this skill differ from domain skills?**
A: This skill covers language fundamentals and common std types. For deep dives into specific areas — concurrency, unsafe, macros, CLI, Web, embedded — use the corresponding domain skill.

**Q: What if example code fails to compile?**
A: Verify `rustc --version` ≥ 1.93.0. If older, some APIs/behaviour may differ. Use the Quick Fixes table above.

**Q: Can I use this offline?**
A: Yes. All `references/` and `examples/` files are local copies and work without internet access.

**Q: Does this skill collect my data?**
A: No. This skill is a pure documentation reference and does not collect any user data.

## Audience

| User Type | Usage |
|-----------|-------|
| **Rust beginners** | Write basic code and learn Rust idioms from Language Reference |
| **Migration users** | Migrate from older editions using the Quick Fixes table |
| **Experienced developers** | Verify API patterns, look up syntax edge cases, load references/ for depth |

## Offline Examples (examples/)

- `examples/quickstart-workflows.md` — Basic project, data structs, I/O, error handling
- `examples/collections-patterns.md` — Vec, String, HashMap, iterator chains, sorting
- `examples/trait-design-patterns.md` — Trait definition, default methods, trait objects, blanket impls
- `examples/error-handling-patterns.md` — Custom errors, anyhow, thiserror, combinators
- `examples/ownership-patterns.md` — Borrow checker patterns, lifetime tricks, NLL patterns

## Language References (references/)

- **[Patterns & Idioms](references/patterns.md)** — Builder pattern, newtype, RAII, interior mutability, typestate
- **[Style Guide](references/style-guide.md)** — Naming conventions, file organization, doc comments

## Standard Library References (references/)

### Collections
- **[Vec](references/vec.md)** — Creation, mutation, capacity, drain, retain, sort
- **[String & str](references/string.md)** — UTF-8 strings, manipulation, searching, conversion
- **[HashMap](references/hashmap.md)** — Entry API, custom keys / hashers, iteration
- **[Iterator patterns](references/iterators.md)** — Adapter combinations, custom iterators, performance notes

### I/O & Filesystem
- **[std::io](references/io.md)** — Read/Write/BufRead traits, BufReader/BufWriter, stdin/stdout/stderr
- **[std::fs](references/fs.md)** — File ops, directories, metadata, path patterns
- **[std::path](references/path.md)** — Path/PathBuf: join, canonicalize, components, glob

### Text & Formatting
- **[std::fmt](references/fmt.md)** — Format string syntax, custom Display/Debug, formatting traits

### Error Handling
- **[Error Patterns](references/errors.md)** — Custom error types, From conversions, error context propagation

### Smart Pointers
- **[Box, Rc, Arc](references/smart-pointers.md)** — Heap allocation, ref counting, Deref/Drop patterns
- **[Cell, RefCell](references/interior-mutability.md)** — Interior mutability, OnceCell, LazyCell
- **[Conversions](references/conversions.md)** — From/Into, TryFrom/TryInto, AsRef/AsRef patterns

## Workflow

Step 1. **Confirm version** — Run `rustc --version` (need ≥ 1.93.0)

Step 2. **Essential syntax** — Use Language Reference section above for fundamentals

Step 3. **Std library lookup** — Find the module in the Std Library Coverage Map; load `references/` file

Step 4. **Copyable code** — Use `examples/` for quick-start patterns

Step 5. **Handle errors** — Use Quick Fixes table for compilation errors

Step 6. **Domain switch** — For concurrency, unsafe, macros, CLI, Web, embedded → use relevant domain skill
