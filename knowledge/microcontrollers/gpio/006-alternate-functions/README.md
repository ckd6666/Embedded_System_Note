# GPIO Alternate Functions

---
## 1. Overview
---

一个 MCU pin 往往可以承担多种内部功能。

**Alternate Function（AF）** 表示把物理 pin 连接到某个 MCU peripheral，例如 timer、USART、SPI、I2C，而不是只把它当成普通 GPIO input 或 output 使用。

这种机制也叫 **pin multiplexing**。

---
## 2. Why Alternate Functions Exist
---

MCU 内部 peripheral signal 的数量往往多于芯片能够提供的专用 package pin 数量。

Pin multiplexing 让一个物理 pin 可以在多种内部功能之间复用。

最终使用哪一种功能，由软件配置决定。

可选功能取决于：

- MCU 型号；
- 具体 pin；
- package。

因此不能假设任意 pin 都能连接任意 peripheral。

---
## 3. GPIO Mode and Function Selection
---

在 STM32 中，要让某个 GPIO pin 承担 digital peripheral signal，通常需要完成两个相关配置：

1. 把 pin 配置成 **alternate-function mode**；
2. 选择这个 pin 对应的具体 alternate function。

这两个配置不是一回事。

Alternate-function mode 表示：

> 这个 pin 现在由 peripheral path 使用，而不是普通 GPIO output logic。

Alternate-function selection 表示：

> 具体是哪一个 peripheral signal 连接到这个 pin。

---
## 4. Alternate-Function Numbers
---

STM32 使用类似 `AF0`、`AF1`、`AF7` 这样的 alternate-function selection。

AF number 的含义取决于具体 device 和 pin。

例如在 STM32F446 上，`PA2` 可以通过 `AF7` 连接到 `USART2_TX`。

这并不表示 AF7 在所有 pin 上都等于 USART2_TX。

某个 pin 支持哪些 AF 映射，应以 datasheet 中的 alternate-function mapping table 为准。

---
## 5. Signal Direction
---

Alternate-function connection 可以承载不同方向的 signal：

- peripheral output 从 MCU 内部传向 pin；
- pin input 从外部传向 peripheral；
- bidirectional peripheral signal 双向使用同一个 pin。

例如：

- USART TX：USART peripheral 产生 signal，并通过 pin 向外发送；
- USART RX：signal 从 pin 进入 USART peripheral。

物理 pin 本身没有变，改变的是 MCU 内部哪个 hardware path 与它连接。

---
## 6. Electrical Configuration Still Matters
---

选择 alternate function 后，GPIO 的 electrical configuration 仍然可能需要设置。

根据具体 peripheral 和 MCU，常见相关设置包括：

- push-pull / open-drain output type；
- pull-up / pull-down；
- output speed。

这些配置必须符合对应 peripheral signal 的电气要求。

见 [Output Types](../005-output-types/README.md) 和 [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md)。

---
## 7. Example: USART2 TX on PA2
---

以 STM32F446 的 `PA2` 输出 `USART2_TX` 为例，概念上的配置过程是：

1. 选择 PA2；
2. 把 PA2 配置成 alternate-function mode；
3. 选择把 PA2 连接到 USART2_TX 的 AF mapping；
4. 配置 USART peripheral。

完成后，USART peripheral 负责产生 TX signal，GPIO alternate-function routing 再把这个 signal 接到 PA2。

---
## 8. Related Knowledge
---

- [GPIO Fundamentals](../001-gpio-fundamentals/README.md)
- [GPIO Input](../002-gpio-input/README.md)
- [GPIO Output](../003-gpio-output/README.md)
- [Output Types](../005-output-types/README.md)
- [GPIO Register Model](../007-gpio-register-model/README.md)

---
## 9. References
---

- STMicroelectronics, [RM0390 — STM32F446xx reference manual](https://www.st.com/resource/en/reference_manual/rm0390-stm32f446xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
- STMicroelectronics, [DS10693 — STM32F446xC/E datasheet](https://www.st.com/resource/en/datasheet/stm32f446re.pdf)
- STMicroelectronics, [Getting started with GPIO](https://wiki.st.com/stm32mcu/wiki/GPIO_feature_overview)
