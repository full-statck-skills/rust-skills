# Cargo Build Examples

## Cargo.toml with features
```toml
[package]
name = "myapp"
version = "0.1.0"
edition = "2024"

[dependencies]
serde = { version = "1", features = ["derive"], optional = true }

[features]
default = []
json = ["dep:serde"]
```

## build.rs
```rust
// build.rs
fn main() {
    let out = std::env::var("OUT_DIR").unwrap();
    std::fs::write(format!("{out}/generated.rs"),
        "pub fn answer() -> i32 { 42 }").unwrap();
    println!("cargo::rerun-if-changed=build.rs");
}
```
