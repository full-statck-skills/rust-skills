# Concurrency References

## Key std types
| Type | Module | Use |
|------|--------|-----|
| `thread::spawn` | std::thread | OS threads |
| `Mutex<T>` | std::sync | Mutual exclusion |
| `Arc<T>` | std::sync | Atomic ref counting |
| `mpsc` | std::sync::mpsc | Multi-producer channel |
| `AtomicU64` | std::sync::atomic | Lock-free atomics |
| `tokio` | tokio crate | Async runtime |

## Further reading
- std::thread docs: https://doc.rust-lang.org/std/thread/
- std::sync docs: https://doc.rust-lang.org/std/sync/
- Tokio tutorial: https://tokio.rs/tokio/tutorial
