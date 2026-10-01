# GPIO Fundamentals

---
## 1. Overview
---

GPIO 是 **General-Purpose Input/Output（通用输入/输出）** 的缩写。

GPIO pin 是 MCU 上一种可以由软件配置用途的数字引脚。它不像某些专用引脚那样只承担固定功能，而是可以根据配置用作 input、output、alternate function 等不同角色。

GPIO 是软件和物理世界之间最常见的接口之一。软件修改 GPIO peripheral 的状态，GPIO peripheral 再把这些状态体现为 pin 上的实际电气行为。

---
## 2. Ports and Pins
---

GPIO pin 通常按照 **Port** 分组。

在 STM32 中，像 `PA5`、`PC13` 这样的名称表示：

- `PA5`：Port A 的 pin 5；
- `PC13`：Port C 的 pin 13。

一个 GPIO Port 管理一组 pin。软件配置或访问 GPIO 时，通常需要同时指定 Port 和 Pin。

具体有多少个 Port、每个 Port 有多少可用 pin，取决于具体 MCU 型号和 package。

---
## 3. Digital Levels
---

数字 GPIO 通常使用两个逻辑状态：

- **LOW** — logic 0；
- **HIGH** — logic 1。

LOW 和 HIGH 是逻辑状态，不是固定不变的电压值。什么电压范围会被 MCU 识别为 LOW 或 HIGH，要以具体器件 datasheet 中的 electrical characteristics 为准。

GPIO 作为 output 时，output circuitry 会把 pin 驱动到对应的 LOW 或 HIGH 电平。

GPIO 作为 input 时，input circuitry 会根据 pin 上的实际电压判断当前是 LOW 还是 HIGH。

---
## 4. GPIO Modes
---

GPIO pin 通常可以配置成几种基本角色。

### 4.1 Input

Pin 用来接收 MCU 外部的数字信号。

见 [GPIO Input](../002-gpio-input/README.md)。

---

### 4.2 Output

MCU 通过 pin 向外输出数字电平。

见 [GPIO Output](../003-gpio-output/README.md)。

---

### 4.3 Alternate Function

Pin 连接到 MCU 内部的其他 peripheral，例如 timer、USART、SPI、I2C。

这类连接不是普通 GPIO output，而是由对应 peripheral 使用这个物理 pin。

见 [Alternate Functions](../006-alternate-functions/README.md)。

---

### 4.4 Analog

Pin 主要用于 analog peripheral，例如 ADC 或 DAC，此时数字 GPIO 路径不是主要信号路径。

---
## 5. GPIO as a Software-Hardware Interface
---

GPIO 的配置和状态通过 hardware registers 暴露给软件。

在 STM32 中，每个 GPIO Port 都有自己的 configuration registers 和 data registers。

软件修改这些 registers，GPIO peripheral 根据 register 中的状态改变 pin 的行为。

核心关系是：

`software -> GPIO peripheral state -> pin electrical behavior`

GPIO 的 register model 见 [GPIO Register Model](../007-gpio-register-model/README.md)。

---
## 6. Related Knowledge
---

- [GPIO Input](../002-gpio-input/README.md)
- [GPIO Output](../003-gpio-output/README.md)
- [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md)
- [Output Types](../005-output-types/README.md)
- [Alternate Functions](../006-alternate-functions/README.md)
- [GPIO Register Model](../007-gpio-register-model/README.md)

---
## 7. References
---

- STMicroelectronics, [Getting started with GPIO](https://wiki.st.com/stm32mcu/wiki/GPIO_feature_overview)
- STMicroelectronics, [AN4899 — Guidelines for GPIO hardware settings and low-power consumption on STM32 MCUs](https://www.st.com/resource/en/application_note/an4899-guidelines-for-gpio-hardware-settings-and-lowpower-consumption-on-stm32-mcus-stmicroelectronics.pdf)
- STMicroelectronics, [RM0390 — STM32F446xx reference manual](https://www.st.com/resource/en/reference_manual/rm0390-stm32f446xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
