# Unsafe & FFI References

## Key types
| Type | Module | Use |
|------|--------|-----|
| `*const T` / `*mut T` | built-in | Raw pointers |
| `NonNull<T>` | std::ptr | Non-null pointer |
| `MaybeUninit<T>` | std::mem | Uninitialized memory |
| `ManuallyDrop<T>` | std::mem | Suppress drop |
| `CStr` / `CString` | std::ffi | C string interop |

## Further reading
- Rustonomicon: https://doc.rust-lang.org/nomicon/
- std::ptr docs: https://doc.rust-lang.org/std/ptr/
