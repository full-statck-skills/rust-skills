# Code Review Examples

## Review checklist
- [ ] Naming conventions followed (PascalCase/snake_case)
- [ ] All Result values handled (no bare unwrap)
- [ ] Unsafe blocks have safety docs
- [ ] Clippy warnings resolved
- [ ] Dead code removed

## Anti-pattern detection
```rust
// BAD
let cloned = data.clone();
for item in cloned { process(item); }

// GOOD
for item in data { process(item); }
```
