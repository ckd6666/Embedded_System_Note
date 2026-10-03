# CPU Registers

---
## 1. Overview
---

CPU registers 是 processor core 内部、供 CPU 执行指令时直接使用的 Register。

它们和 peripheral registers 不同：CPU registers 属于 processor core 的执行状态，而 peripheral registers 属于 GPIO、USART、Timer 等外设。

在 Cortex-M4 中，常见 core registers 包括：

- `R0-R12`：general-purpose registers；
- `R13`：Stack Pointer（SP）；
- `R14`：Link Register（LR）；
- `R15`：Program Counter（PC）；
- Program Status Register（PSR）及其他 special registers。

---
## 2. General-Purpose Registers
---

`R0-R12` 是 32-bit general-purpose registers。

它们主要用于保存：

- 运算数据；
- 临时结果；
- 地址；
- function call 过程中需要传递或保存的值。

CPU 执行机器指令时，会频繁在这些 registers 和 memory 之间移动数据。

例如一条加法指令可能读取两个 register 中的值，把结果写回另一个 register。

这些具体分工由 instruction set 和 calling convention 决定，不需要把每个 register 永久绑定成某一种用途。

---
## 3. Stack Pointer
---

`R13` 是 Stack Pointer（SP）。

Stack Pointer 保存当前 stack 位置，用于 function call、local state 保存、exception entry 等过程。

Cortex-M4 支持两个 stack pointer：

- **MSP** — Main Stack Pointer；
- **PSP** — Process Stack Pointer。

具体使用哪一个取决于 processor mode 和配置。

对初学者最重要的是理解：

> Stack Pointer 是 CPU 用来定位当前 stack 的 register。

---
## 4. Link Register
---

`R14` 是 Link Register（LR）。

它主要保存 subroutine、function call 或 exception 返回时需要的 return information。

普通 function call 执行时，CPU 需要知道 function 结束后回到哪里继续执行。LR 就参与保存这类返回信息。

因此 LR 和“函数为什么执行完还能回到调用位置”有直接关系。

---
## 5. Program Counter
---

`R15` 是 Program Counter（PC）。

PC 保存当前程序执行位置相关的地址。

CPU 正常执行代码时，PC 会随着 instruction flow 更新。

当发生：

- branch；
- function call；
- function return；
- interrupt / exception；

CPU 的执行位置会改变，PC 也会随之改变。

这就是为什么 CPU 可以从 `main()` 跳到其他 function，也可以在 interrupt 发生时进入 ISR。

---
## 6. Program Status Register
---

Cortex-M4 的 Program Status Register（PSR）包含 processor 当前执行状态相关的信息。

它组合了：

- **APSR** — Application Program Status Register；
- **IPSR** — Interrupt Program Status Register；
- **EPSR** — Execution Program Status Register。

这些状态包括 arithmetic condition flags、当前 exception number、instruction execution state 等。

它们属于 CPU 执行模型，而不是 peripheral configuration。

---
## 7. CPU Register and C Code
---

C source code 通常不会直接指定：

> 这个变量必须一直放在 R3。

Compiler 会根据 instruction set、optimization 和 calling convention 决定怎样使用 CPU registers。

因此：

```c
int a = 10;
int b = 20;
int c = a + b;
```

在机器代码层面可能会使用 CPU registers 完成运算，但 C source 本身描述的是语言语义，不是具体 register 分配。

这和 peripheral register 不同。Peripheral register 往往代表明确的 hardware control/status state，软件访问它是为了直接影响某个外设。

---
## 8. Course Resources
---

CPU registers 和 instruction execution、function call、stack、control flow 连在一起，第一次学习时更适合配合动态讲解。

- **MIT 6.004 Computation Structures — L02–L04**  
  Bilibili：https://www.bilibili.com/video/BV197411s736/  
  中英 CC 字幕，中文为机翻。L02 介绍 RISC-V registers 与 assembly，L03 讲 procedure 和 stack，L04 继续连接 procedure、stack 与 MMIO。

- **Embedded Systems Bare-Metal Programming Ground Up™ (STM32) — 23.5 ARM Cortex-M Registers**  
  Bilibili：https://www.bilibili.com/video/BV1VwgYz4Etn/  
  大模型机翻双语字幕。用于把通用 CPU-register 概念对应到 ARM Cortex-M。

## 9. Related Knowledge
---

- [Register Fundamentals](../001-register-fundamentals/README.md)
- [Peripheral Registers](../003-peripheral-registers/README.md)
- [Memory-Mapped I/O](../004-memory-mapped-io/README.md)

---
## 10. References
---

- STMicroelectronics, [PM0214 — STM32 Cortex-M4 MCUs and MPUs programming manual](https://www.st.com/resource/en/programming_manual/dm00046982-stm32-cortex-m4-mcus-and-mpus-programming-manual-stmicroelectronics.pdf)
- Arm, [Cortex-M processor documentation resources](https://developer.arm.com/community/arm-community-blogs/b/architectures-and-processors-blog/posts/cortex-m-resources)
