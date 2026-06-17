---
name: rust-embedded
description: Rust 嵌入式开发技能 — 涵盖 no_std、嵌入式 HAL、RTIC、Cortex-M、RISC-V、外设访问、中断处理、DMA、实时约束、调试（probe-rs、OpenOCD）、链接脚本配置等。基于 The Embedded Rust Book 与社区实践。当用户需要为微控制器编写 Rust 代码时激活。
---

# Rust 嵌入式开发

> 基于 [The Embedded Rust Book](https://doc.rust-lang.org/embedded-book) 与社区最佳实践。

## Capability Boundaries

### ✅ 强项
1. no_std 环境、核心 crate 依赖
2. 嵌入式 HAL（embedded-hal）、PAC（外设访问 crate）
3. RTIC 实时框架（实时中断驱动的并发）
4. GPIO、UART、I2C、SPI、ADC、PWM 外设驱动
5. 中断处理（中断表、NVIC、异常处理）
6. 内存映射寄存器访问（mmio、VolatileCell）
7. 静态分配（固定大小缓冲区、静态数组）
8. 调试（probe-rs、itmdump、semihosting、RTT）

### ⚠️ 前置要求
1. 理解 Rust 所有权与生命周期
2. 了解目标微控制器架构（Cortex-M、RISC-V 等）

### ❌ 不适用范围
1. 标准库功能 → 使用 `rust-core` 技能
2. 桌面端 Web 开发 → 使用 `rust-web` 技能

## 何时使用

- "为 STM32/ESP32/RP2040 编写 Rust"
- "裸机开发"
- "实时嵌入式系统"
- "无操作系统环境"

## Data Privacy

本技能不收集、存储或传输任何用户数据。

---

# Rust 嵌入式开发参考

## no_std 环境

```rust
// 禁用标准库
#![no_std]
#![no_main]

use core::panic::PanicInfo;

#[panic_handler]
fn panic(_info: &PanicInfo) -> ! {
    loop {}
}
```

## 入口与启动

```rust
// 使用 cortex-m-rt
#![no_std]
#![no_main]

use cortex_m_rt::entry;

#[entry]
fn main() -> ! {
    loop {}
}
```

## 外设访问模式

```rust
use embedded_hal::digital::OutputPin;
use rp2040_hal::gpio::Pins;

// 初始化外设
let mut peripherals = rp2040_hal::Peripherals::take().unwrap();

// 配置 GPIO
let pins = Pins::new(peripherals.IO_BANK0, peripherals.PADS_BANK0, sio.gpio_bank0, &mut resets);
let mut led_pin = pins.gpio25.into_push_pull_output();

// 操作外设
led_pin.set_high().unwrap();
```

## 常用 Crate

| 用途 | crate | 说明 |
|------|-------|------|
| 运行时 | cortex-m-rt | Cortex-M 启动代码 |
| HAL | embedded-hal | 硬件抽象层 trait |
| MCU HAL | rp2040-hal | RP2040 |
| MCU HAL | stm32f4xx-hal | STM32F4 |
| MCU HAL | esp32c3-hal | ESP32-C3 |
| 实时框架 | rtic | RTIC 并发框架 |
| 调试 | probe-rs | 调试工具 |
| 串口 | defmt | 高效日志格式化 |
