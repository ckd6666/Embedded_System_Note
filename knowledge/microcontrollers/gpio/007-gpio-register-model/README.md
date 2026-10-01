# GPIO Register Model

---
## 1. Overview
---

MCU 的 GPIO 通过 hardware registers 与软件交互。

在 STM32 中，GPIO registers 属于 **memory-mapped I/O**：每个 register 都映射到 processor address space 中的某个地址。C library 或 driver 最终通过这些地址访问 GPIO peripheral，GPIO hardware 再根据 register 中的状态工作。

核心桥接关系是：

`C / library call -> memory-mapped GPIO register -> GPIO hardware behavior`

---
## 2. Memory-Mapped I/O
---

Memory-mapped I/O 的意思是：peripheral register 可以使用 CPU 普通的 memory load / store 指令访问。

这些地址虽然位于 processor address space 中，但并不代表普通 RAM，而是映射到 peripheral 内部的 hardware state。

因此软件可以通过访问 GPIO register 来：

- 写入 configuration bits；
- 写入 output state；
- 读取 input state。

从 CPU 角度看，这是一次 memory access；从 peripheral 角度看，这是一次 hardware control 或 status access。

---
## 3. One Port, Several Registers
---

一个 GPIO Port 会包含多个 registers，因为 pin 的不同属性需要独立配置。

以 STM32F446 为例，主要 GPIO registers 包括：

| Register | Main role |
| --- | --- |
| `GPIOx_MODER` | 选择 input、output、alternate-function 或 analog mode |
| `GPIOx_OTYPER` | 选择 push-pull 或 open-drain output type |
| `GPIOx_OSPEEDR` | 选择 output speed |
| `GPIOx_PUPDR` | 选择 no pull、pull-up 或 pull-down |
| `GPIOx_IDR` | 提供当前 digital input state |
| `GPIOx_ODR` | 保存 GPIO output data |
| `GPIOx_BSRR` | set/reset output bits |
| `GPIOx_AFRL/H` | 选择 pin 的 alternate function |

不同 MCU 的 register 名称和布局可能不同，但把 configuration、input state、output state 和 peripheral routing 分开管理，是 GPIO peripheral 中常见的设计。

---
## 4. Configuration Registers
---

Configuration registers 决定 pin 之后会怎样工作。

以 STM32F446 为例：

- `MODER`：选择 pin 的基本 mode；
- `OTYPER`：选择 push-pull 或 open-drain；
- `OSPEEDR`：选择 output speed；
- `PUPDR`：配置 internal pull-up / pull-down；
- `AFRL` / `AFRH`：选择 alternate-function routing。

修改这些 registers，就是在修改 GPIO hardware configuration。

---
## 5. Input Data Register
---

`GPIOx_IDR` 暴露 GPIO input path 当前观察到的数字状态。

其中每个相关 bit 对应 Port 中的一个 pin。

软件读取这个 bit 并不会产生 input state。

Pin 上的实际电压首先被 GPIO input circuitry 解释为 digital state，然后这个状态才反映到 `GPIOx_IDR` 中。

见 [GPIO Input](../002-gpio-input/README.md)。

---
## 6. Output Data Register
---

`GPIOx_ODR` 保存 GPIO output data。

当某个 pin 被配置为 output 时，对应 bit 表示 GPIO output path 希望输出的状态。

例如改变 GPIOA output data 的 bit 5，会改变 PA5 对应的 output state；最终 pin 怎样被驱动，还取决于 mode 和 output type 等配置。

见 [GPIO Output](../003-gpio-output/README.md)。

---
## 7. Bit Set/Reset Register
---

STM32 提供 `GPIOx_BSRR` 来 set 或 reset 指定 output bits。

这样软件可以直接请求修改某些 bit，而不必先读取整个 output data register，再修改后写回。

这种方式可以避免典型 read-modify-write sequence 带来的不必要干扰。

具体 set/reset bit layout 由 reference manual 定义。

---
## 8. Alternate-Function Registers
---

STM32 GPIO Port 使用 alternate-function registers 选择哪个 peripheral signal 连接到某个 pin。

在 STM32F446 中：

- `AFRL` 管理 Port 中较低编号的 pin；
- `AFRH` 管理较高编号的 pin。

实际选择的 AF value 必须是 datasheet 中为该 pin 定义的有效 mapping。

见 [GPIO Alternate Functions](../006-alternate-functions/README.md)。

---
## 9. From API to Register
---

Library API 会隐藏 register 访问细节，但不会绕过 register model。

例如：

- 配置 GPIO pin 为 output 的 API，最终会修改 GPIO mode 对应的 register bits；
- 读取 GPIO input 的 API，最终会读取 peripheral input state；
- 改变 GPIO output 的 API，最终会通过相应 register interface 更新 output state。

因此，理解 register model 可以解释 GPIO API 实际控制了什么，而应用代码本身不必直接操作 raw address。

---
## 10. Related Knowledge
---

- [GPIO Fundamentals](../001-gpio-fundamentals/README.md)
- [GPIO Input](../002-gpio-input/README.md)
- [GPIO Output](../003-gpio-output/README.md)
- [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md)
- [Output Types](../005-output-types/README.md)
- [Alternate Functions](../006-alternate-functions/README.md)

---
## 11. References
---

- STMicroelectronics, [RM0390 — STM32F446xx reference manual](https://www.st.com/resource/en/reference_manual/rm0390-stm32f446xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
- STMicroelectronics, [DS10693 — STM32F446xC/E datasheet](https://www.st.com/resource/en/datasheet/stm32f446re.pdf)
- STMicroelectronics, [AN4899 — Guidelines for GPIO hardware settings and low-power consumption on STM32 MCUs](https://www.st.com/resource/en/application_note/an4899-guidelines-for-gpio-hardware-settings-and-lowpower-consumption-on-stm32-mcus-stmicroelectronics.pdf)
