# Concurrency Examples

## Thread pool pattern
```rust
use std::sync::{Arc, Mutex};
use std::thread;

let counter = Arc::new(Mutex::new(0));
let mut handles = vec![];

for _ in 0..10 {
    let c = Arc::clone(&counter);
    handles.push(thread::spawn(move || {
        *c.lock().unwrap() += 1;
    }));
}
for h in handles { h.join().unwrap(); }
println!("{}", counter.lock().unwrap());
```

## async/await with Tokio
```rust
use tokio::time;

async fn task(n: u32) -> u32 {
    time::sleep(time::Duration::from_millis(n as u64)).await;
    n
}

#[tokio::main]
async fn main() {
    let (a, b) = tokio::join!(task(100), task(200));
    println!("{a}, {b}");
}
```

## mpsc channel
```rust
use std::sync::mpsc;
use std::thread;

let (tx, rx) = mpsc::channel();
for i in 0..5 {
    let tx = tx.clone();
    thread::spawn(move || { tx.send(i).unwrap(); });
}
for received in rx { println!("Got: {received}"); }
```
