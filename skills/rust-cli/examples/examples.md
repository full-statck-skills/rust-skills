# CLI Examples

## Basic clap CLI
```rust
use clap::Parser;

#[derive(Parser)]
#[command(name = "myapp", version, about = "A CLI tool")]
struct Cli {
    #[arg(short, long, default_value = "-")]
    input: String,
    #[arg(short, long)]
    verbose: bool,
}

fn main() {
    let cli = Cli::parse();
    println!("input: {}, verbose: {}", cli.input, cli.verbose);
}
```

## File reader
```rust
use std::fs;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let contents = fs::read_to_string("data.txt")?;
    for line in contents.lines() {
        println!("{line}");
    }
    Ok(())
}
```

## Cat clone (stdin)
```rust
use std::io::{self, BufRead};

fn main() {
    for line in io::stdin().lock().lines() {
        println!("{}", line.unwrap());
    }
}
```
