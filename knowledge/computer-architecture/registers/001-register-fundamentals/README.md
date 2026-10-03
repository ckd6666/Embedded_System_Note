# Register Fundamentals

---
## 1. Overview
---

Register（寄存器）是 processor core 或 peripheral 内部、由 hardware 定义并可被读取或写入的状态/数据接口。

有些 Register 会实际保存 bit 状态；有些 Register 的 read / write 会直接反映或触发 hardware behavior，因此不能把所有 Register 都简单理解成普通 memory。

在嵌入式系统里，Register 主要出现在两类位置：

- **CPU registers**：CPU 执行指令时直接使用，例如保存运算数据、地址、Stack Pointer、Program Counter；
- **Peripheral registers**：GPIO、USART、Timer、ADC 等 peripheral 用来表示配置、数据和状态，或触发硬件操作。

两者都叫 Register，但用途和访问方式不同。

---
## 2. Register Stores Bits
---

Register 由若干 bit 组成。

例如一个 32-bit register 可以包含 32 个 bit：

```text
bit31 ... bit2 bit1 bit0
```

这些 bit 可以整体表示一个数值，也可以被划分成多个 **bit field**，每个 field 表示不同含义。

例如一个 32-bit peripheral register 可能把：

- bit 0 用作 enable；
- bits 2:1 用作 mode；
- bit 8 用作 status flag。

因此，理解 Register 时不能只看整个数值，还要看每个 bit / field 的硬件语义。

---
## 3. Register Width
---

Register width 表示 Register 一共有多少 bit，例如：

- 8-bit；
- 16-bit；
- 32-bit；
- 64-bit。

在 Cortex-M4 中，常见 core registers 是 32-bit。STM32F446 的很多 peripheral registers 也使用 32-bit register layout，但具体宽度和有效 bit 仍应以对应 reference manual 为准。

Register width 不等于所有 bit 都有定义。一个 32-bit register 中可能只有部分 bit 有实际功能，其余 bit 可能是 reserved。

---
## 4. Register Value and Hardware State
---

Register 的值通常不是孤立存在的。

对于 CPU register，它可能直接参与指令执行。

对于 peripheral register，它常常和真实硬件状态连接。例如：

- control register 中某个 bit 置 1，可能 enable 一个 peripheral；
- status register 中某个 bit 由 hardware 置 1，表示某个 event 已经发生；
- data register 中的值可能成为 peripheral 要发送的数据；
- input register 中的 bit 可能反映 pin 当前的数字电平。

因此，在嵌入式系统中，Register 是 software 和 hardware 之间的重要接口。

---
## 5. Reading and Writing
---

Register 是否可以 read 或 write 取决于它的设计。

常见情况包括：

- **read-only**：software 只能读取；
- **write-only**：software 只能写入；
- **read-write**：software 既能读也能写。

即使两个 Register 都是 32-bit，也不能因为宽度相同就假设访问语义相同。

Register 的具体 access type、reset value 和 field 含义必须查对应 architecture manual 或 reference manual。

---
## 6. Reset Value
---

很多 Register 在 reset 后会进入规定的初始状态，这个初始值通常称为 **reset value**。

Reset value 说明 software 尚未主动配置时 hardware 从什么状态开始。

例如某个 GPIO configuration register 在 reset 后可能表示 input mode；某个 control register 的 enable bit 在 reset 后可能为 0。

具体 reset value 必须以目标 device 的官方文档为准。

---
## 7. Related Knowledge
---

- [CPU Registers](../002-cpu-registers/README.md)
- [Peripheral Registers](../003-peripheral-registers/README.md)
- [Memory-Mapped I/O](../004-memory-mapped-io/README.md)
- [Register Fields and Access Semantics](../005-register-fields-and-access-semantics/README.md)
- [Reading Register Documentation](../006-reading-register-documentation/README.md)

---
## 8. References
---

- STMicroelectronics, [PM0214 — STM32 Cortex-M4 MCUs and MPUs programming manual](https://www.st.com/resource/en/programming_manual/dm00046982-stm32-cortex-m4-mcus-and-mpus-programming-manual-stmicroelectronics.pdf)
- Arm, [CMSIS-SVD register properties](https://arm-software.github.io/CMSIS_5/SVD/html/elem_special.html)
- STMicroelectronics, [RM0390 — STM32F446xx reference manual](https://www.st.com/resource/en/reference_manual/rm0390-stm32f446xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
