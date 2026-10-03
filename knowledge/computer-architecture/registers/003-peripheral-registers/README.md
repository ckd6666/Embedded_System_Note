# Peripheral Registers

---
## 1. Overview
---

Peripheral registers 是 GPIO、USART、Timer、ADC、DMA 等 peripheral 内部暴露给软件的控制、状态和数据接口。

软件通过读写这些 registers 来：

- 配置 peripheral；
- 启动或停止某个功能；
- 写入要处理或发送的数据；
- 读取 hardware 当前状态；
- 清除或确认某些 event。

它们是 embedded software 与 MCU peripheral 之间最重要的接口之一。

---
## 2. Common Register Roles
---

Peripheral registers 常见可以按用途分成几类。

### 2.1 Configuration / Control Register

用于配置 peripheral 怎样工作。

例如可能包含：

- enable bit；
- mode selection；
- clock division；
- interrupt enable；
- channel selection。

软件修改这些 bit 后，peripheral 的工作方式随之改变。

---

### 2.2 Status Register

用于表示 peripheral 当前状态或已经发生的 event。

例如可能表示：

- data ready；
- transmission complete；
- interrupt pending；
- overflow；
- error condition。

有些 status bit 由 hardware 自动设置，软件只负责读取或清除。

---

### 2.3 Data Register

用于在 software 和 peripheral 之间传递数据。

例如：

- USART transmit data；
- ADC conversion result；
- SPI receive data；
- GPIO input/output data。

Data register 的具体读写行为取决于对应 peripheral。

---
## 3. Register Bits Have Hardware Meaning
---

Peripheral register 中的 bit 不是普通软件变量。

例如某个 control register 的 bit 0 可能定义为：

```text
0 = peripheral disabled
1 = peripheral enabled
```

软件把这个 bit 写成 1 后，hardware control logic 会按照这个定义改变 peripheral 状态。

因此：

> 写 register = 改变 peripheral hardware state

但前提是必须按照 reference manual 中定义的语义访问。

---
## 4. Hardware Can Update Registers
---

Peripheral register 不一定只由 software 修改。

Hardware 也可能自动更新 register 或 field。

例如：

- ADC 完成 conversion 后设置 status flag；
- USART 收到数据后更新 receive data / status；
- Timer counter 随 clock 自动变化；
- GPIO input register 反映 pin 当前电平。

所以 peripheral register 是 software 与 hardware 共享的接口。

这也是为什么不能简单把它们当成普通 RAM variable 来理解。

---
## 5. Different Registers Have Different Access Rules
---

Peripheral registers 可能具有不同 access type：

- read-only；
- write-only；
- read-write。

有些 field 还具有特殊 write semantics，例如：

- write 1 to clear；
- write 0 to clear；
- read 后自动清除；
- 写入触发一次 hardware action。

因此，访问一个 peripheral register 前，必须先理解它的具体 access semantics。

详见 [Register Fields and Access Semantics](../005-register-fields-and-access-semantics/README.md)。

---
## 6. Peripheral Register vs CPU Register
---

两者都叫 Register，但属于不同层次。

| 类型 | 所属位置 | 主要用途 |
| --- | --- | --- |
| CPU register | processor core | 执行指令、运算、地址、control flow |
| Peripheral register | peripheral hardware | 配置、控制、交换数据、报告状态 |

例如：

- `PC` 是 CPU register；
- `GPIOA_ODR` 是 GPIO peripheral register；
- `USART2_SR` / 对应 USART status register 是 peripheral register。

CPU 会通过 memory-mapped I/O 访问 peripheral registers。

---
## 7. Example: GPIO
---

以 GPIO output 为例：

软件调用 GPIO API 后，library 最终会修改 GPIO peripheral 中对应的 register state。

例如：

- mode register 决定 pin 是 input 还是 output；
- output type register 决定 push-pull 或 open-drain；
- output data register 保存 output state。

Peripheral register 把“软件配置”转换成 GPIO hardware 可以直接使用的状态。

---
## 8. Related Knowledge
---

- [Register Fundamentals](../001-register-fundamentals/README.md)
- [CPU Registers](../002-cpu-registers/README.md)
- [Memory-Mapped I/O](../004-memory-mapped-io/README.md)
- [Register Fields and Access Semantics](../005-register-fields-and-access-semantics/README.md)
- [GPIO Register Model](../../../microcontrollers/gpio/007-gpio-register-model/README.md)

---
## 9. References
---

- Arm, [CMSIS-Core Peripheral Access](https://arm-software.github.io/CMSIS_6/latest/Core/group__peripheral__gr.html)
- Arm, [CMSIS-SVD register properties](https://arm-software.github.io/CMSIS_5/SVD/html/elem_special.html)
- STMicroelectronics, [RM0390 — STM32F446xx reference manual](https://www.st.com/resource/en/reference_manual/rm0390-stm32f446xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
