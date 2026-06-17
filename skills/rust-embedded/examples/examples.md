# Embedded Examples

## Minimal no_std
```rust
#![no_std]
#![no_main]

use core::panic::PanicInfo;

#[panic_handler]
fn panic(_info: &PanicInfo) -> ! { loop {} }

#[cortex_m_rt::entry]
fn main() -> ! { loop {} }
```

## GPIO blink (cortex-m)
```rust
let mut peripherals = rp2040_hal::Peripherals::take().unwrap();
let pins = rp2040_hal::gpio::Pins::new(peripherals.IO_BANK0, ...);
let mut led = pins.gpio25.into_push_pull_output();
led.set_high().ok();
```

## RTIC task
```rust
#[rtic::app(device = rp2040_pac)]
mod app {
    #[shared] struct Shared { count: u32 }
    #[local] struct Local { }

    #[init]
    fn init(_: init::Context) -> (Shared, Local) { (Shared { count: 0 }, Local {}) }

    #[task(shared = [count])]
    fn increment(mut cx: increment::Context) {
        cx.shared.count.lock(|c| *c += 1);
    }
}
```
