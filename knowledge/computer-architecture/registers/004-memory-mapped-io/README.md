# Memory-Mapped I/O

---
## 1. Overview
---

Memory-Mapped I/O（MMIO）是一种让 CPU 通过普通 address space 访问 hardware peripheral 的方式。

在这种设计里，peripheral registers 会被分配到 processor address space 中的固定地址。

因此，CPU 可以使用与普通 memory access 相同的 load / store 机制访问 peripheral register。

关键区别是：

> 某个 address 访问到的不是普通 RAM，而是 hardware register。

---
## 2. Address Space
---

Processor address space 是 CPU 可以产生并访问的地址范围。

不同地址区间可以连接到不同目标，例如：

- Flash；
- SRAM；
- peripheral registers；
- system control region；
- external memory。

因此，“CPU 访问一个地址”并不自动等于“访问 RAM”。

这个地址最终连接到什么，由具体 processor / MCU 的 memory map 决定。

---
## 3. Peripheral Base Address
---

一个 peripheral 通常会占用 address space 中的一段连续区域。

例如概念上：

```text
GPIOA base address
+ 0x00 -> MODER
+ 0x04 -> OTYPER
+ 0x08 -> OSPEEDR
...
```

这里的 **base address** 是 peripheral address block 的起点。

每个 register 相对 base address 还有自己的 **offset**。

因此：

```text
register address = peripheral base address + register offset
```

具体 base address 和 offset 必须查 MCU reference manual。

---
## 4. What Happens on a Register Access
---

当 CPU 对某个 peripheral register address 执行 read 时：

1. CPU 发起 memory read；
2. MCU interconnect 把这个 address 路由到对应 peripheral；
3. peripheral 返回 register 当前值；
4. CPU 得到读取结果。

当 CPU 执行 write 时：

1. CPU 发起 memory write；
2. address 被路由到对应 peripheral；
3. write data 到达目标 register；
4. peripheral 根据 register 定义改变 hardware state 或执行相应动作。

因此，MMIO 是这座桥的核心：

`CPU memory access -> peripheral register -> hardware behavior`

---
## 5. Why APIs Can Control Hardware
---

像：

```c
gpio_set(...);
```

这样的 library API 最终并不是“神奇地控制硬件”。

它会通过某种 C-level register access，最终让 CPU 对特定 memory-mapped register address 执行 read 或 write。

因此可以把常见路径理解成：

`C API -> register address access -> peripheral register -> hardware behavior`

C 语言里如何正确表达这种 fixed-address register access，属于 [C / Hardware Access](../../../C/10-hardware-access/)。

---
## 6. MMIO Is Not Ordinary RAM
---

Memory-mapped register 虽然出现在 address space 中，但其行为可能和 RAM 完全不同。

普通 RAM 通常满足：

- write 一个值；
- later read 时得到之前保存的值。

Peripheral register 可能不是这样。

例如某个 register 可能：

- read-only；
- write-only；
- read 时返回 live hardware state；
- write 1 清除某个 flag；
- write 后立即触发 hardware action；
- 某些 bit 由 hardware 自己更新。

所以“有一个地址”只说明 CPU 可以通过 address space 访问它，不代表它具有普通 memory 的语义。

---
## 7. MMIO and CPU Registers
---

Memory-Mapped I/O 访问的 peripheral registers 不等于 CPU registers。

CPU registers 位于 processor core 内部，并由 instruction set 直接操作。

Peripheral registers 位于 MCU peripheral hardware 中，CPU 通过 address space 访问它们。

例如：

- `R0`：CPU register；
- `PC`：CPU register；
- `GPIOA_ODR`：memory-mapped peripheral register。

---
---
## 8. Course Resources
---

Memory-Mapped I/O 涉及 CPU、address space、memory access 和 peripheral 多层连接，适合用课程先建立整体过程。

- **MIT 6.004 Computation Structures — L04 Procedures and MMIO**  
  Bilibili：https://www.bilibili.com/video/BV197411s736/  
  提供中英 CC 字幕，中文为机翻。L04 直接讲 MMIO，适合建立 CPU 通过 memory address 访问 I/O 的整体模型。

- **《从 CPU 架构到操作系统实现》— 外设章 5.1–5.3**  
  Bilibili：https://www.bilibili.com/video/BV1ErEaznEaB/  
  中文讲解。重点看存储器系统、读写存储器，以及用 ARM 汇编点灯，可把 MMIO 概念连接到 Cortex-M / STM32。
## 9. Related Knowledge
---

- [Register Fundamentals](../001-register-fundamentals/README.md)
- [CPU Registers](../002-cpu-registers/README.md)
- [Peripheral Registers](../003-peripheral-registers/README.md)
- [Register Fields and Access Semantics](../005-register-fields-and-access-semantics/README.md)
- [Reading Register Documentation](../006-reading-register-documentation/README.md)
- [C Hardware Access](../../../C/10-hardware-access/)

---
## 10. References
---

- STMicroelectronics, [PM0214 — STM32 Cortex-M4 MCUs and MPUs programming manual](https://www.st.com/resource/en/programming_manual/dm00046982-stm32-cortex-m4-mcus-and-mpus-programming-manual-stmicroelectronics.pdf)
- Arm, [CMSIS-SVD device and address-space description](https://arm-software.github.io/CMSIS_5/5.8.0/SVD/html/elem_device.html)
- STMicroelectronics, [RM0390 — STM32F446xx reference manual](https://www.st.com/resource/en/reference_manual/rm0390-stm32f446xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
