# Register Fields and Access Semantics

---
## 1. Overview
---

一个 Register 往往不会把所有 bit 当成一个整体使用，而是划分成多个 **bit field**。

每个 field 都有自己的位置、宽度和含义。

例如一个 32-bit register 可能包含：

- bit 0：enable；
- bits 2:1：mode；
- bit 8：status flag；
- 其余 bit：reserved。

理解 peripheral register 时，重点不是只看整个 hexadecimal value，而是理解每个 field 的定义和访问语义。

---
## 2. Bit and Bit Field
---

单个 bit 只能表示 0 或 1。

多个连续 bit 可以组成一个 field。

例如：

```text
bits 2:1 = MODE
```

表示 MODE field 占用 bit 2 和 bit 1，因此一共可以表示 4 种 bit pattern：

```text
00
01
10
11
```

这些 bit pattern 分别代表什么，必须由 reference manual 定义。

不能只根据数值自行猜测。

---
## 3. Access Type
---

Register 或 field 通常会标明 access type。

常见类型包括：

- **read-only (RO)**：软件可以 read，不能按普通方式 write；
- **write-only (WO)**：软件可以 write，read 不具有普通读取语义；
- **read-write (RW)**：软件可以 read，也可以 write。

有些设备还定义更特殊的 access rule。

Access type 是 register 语义的一部分，不能只根据 C type 判断。

---
## 4. Reset Value
---

**Reset value** 表示 device reset 后 Register 或 field 的规定初始状态。

例如：

```text
Reset value: 0x00000000
```

表示在对应 reset 条件后，文档定义的 register bits 初始为 0。

Reset value 可以帮助判断：

- software 尚未配置时 hardware 的初始状态；
- 某个 enable bit 默认是否关闭；
- 某个 mode field 默认选择哪种模式。

并不是所有 bit 都一定具有定义明确的 reset value，具体要看官方文档。

---
## 5. Hardware-Updated Fields
---

某些 field 主要由 hardware 更新。

例如 status flag 可能在 event 发生时自动从 0 变成 1。

软件可能只负责：

- read 这个 flag；
- 按文档规定的方法 clear；
- 根据 flag 做下一步处理。

因此不能假设：

> Register 里的所有 bit 都只由 software write 决定。

Peripheral register 经常是 software 和 hardware 共同作用的状态接口。

---
## 6. Special Write Semantics
---

有些 field 的 write 行为不是普通“把写入值保存进去”。

常见例子包括：

- **write-one-to-clear (W1C)**：向某个 bit 写 1 表示清除该 flag；
- write 后触发一次 action；
- 只有第一次 write 有效；
- 某些 value 被禁止或保留。

例如 W1C field 当前为 1 时，software 写入 1 的含义可能是：

> clear this flag

而不是：

> 把这个 bit 设成 1

所以访问 peripheral register 时，必须先看 field description。

---
## 7. Reserved Bits
---

Register 中没有公开定义用途的 bit 常标记为 **reserved**。

Reserved bit 不应该被当成普通可用 field。

软件应按照 reference manual 对该 register 的要求处理 reserved bits，不应自行赋予意义或依赖其读出值。

这也是为什么修改某个 field 时，不能在不了解 register semantics 的情况下随意改写整个 Register。

---
## 8. Read-Modify-Write
---

当 software 只想修改 Register 中的一部分 field 时，常见思路是：

1. read 当前 register value；
2. 修改目标 bits；
3. write 新 value。

这个过程称为 **read-modify-write**。

但它并不适合所有 peripheral registers。

如果同一个 Register 中存在：

- hardware-updated bits；
- W1C bits；
- write-only bits；
- special write semantics；

普通 read-modify-write 可能产生错误结果。

因此是否适合使用 read-modify-write，要由该 Register 的官方语义决定。

C 中 bit mask 和 read-modify-write 的具体写法见 [C Hardware Access](../../../C/10-hardware-access/)。

---
## 9. Related Knowledge
---

- [Register Fundamentals](../001-register-fundamentals/README.md)
- [Peripheral Registers](../003-peripheral-registers/README.md)
- [Memory-Mapped I/O](../004-memory-mapped-io/README.md)
- [Reading Register Documentation](../006-reading-register-documentation/README.md)
- [C Hardware Access](../../../C/10-hardware-access/)

---
## 10. References
---

- Arm, [CMSIS-SVD register properties](https://arm-software.github.io/CMSIS_5/SVD/html/elem_special.html)
- Arm, [CMSIS-SVD schema access and modified-write semantics](https://arm-software.github.io/CMSIS_5/SVD/html/schema_1_2_gr.html)
- STMicroelectronics, [RM0390 — STM32F446xx reference manual](https://www.st.com/resource/en/reference_manual/rm0390-stm32f446xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
