# GPIO Output

---
## 1. Overview
---

GPIO output mode 让 MCU 能够主动把数字状态输出到 pin。

基本路径是：

`software output state -> GPIO output logic -> pin driver -> pin voltage`

软件改变 GPIO peripheral 中的 output state，GPIO peripheral 再通过 output driver 把这个状态体现为 pin 上的实际电压。

---
## 2. Output State
---

Digital GPIO output 通常有两个逻辑状态：

- **LOW** — logic 0；
- **HIGH** — logic 1。

在 STM32 中，GPIO peripheral 会保存希望输出的状态，随后 output circuitry 根据这个状态和当前 output type 驱动对应 pin。

实际输出电压还会受到 supply voltage、load current 和器件 electrical characteristics 的影响。

---
## 3. Setting, Clearing, and Toggling
---

常见的 GPIO output 操作包括：

- **set** — 把 output state 设为 HIGH；
- **clear/reset** — 把 output state 设为 LOW；
- **toggle** — 把 HIGH 改成 LOW，或把 LOW 改成 HIGH。

Toggle 不是一种特殊电气模式，它只是软件把当前 output state 改成相反状态。

在 STM32 中，output state 与 `GPIOx_ODR`、`GPIOx_BSRR` 等 registers 有关。具体 register model 见 [GPIO Register Model](../007-gpio-register-model/README.md)。

---
## 4. Output Electrical Configuration
---

把 pin 配置成 output，只是选择了 GPIO output path。

Pin 的实际电气行为还受到其他配置影响，例如：

- **push-pull / open-drain output type**；
- optional **pull-up / pull-down**；
- **output speed / slew-rate capability**。

Output speed 控制 output driver 改变 pin 电压的速度能力。它不会让 pin 自动以某个频率切换，也不等于软件或 peripheral 实际产生的 signal frequency。

见 [Output Types](../005-output-types/README.md) 和 [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md)。

---
## 5. Output State and Physical Load
---

GPIO pin 最终会连接到某个外部 electrical load，例如：

- LED circuit；
- 另一个 digital input；
- enable pin；
- transistor 或 logic gate。

软件改变 output state，只是确定 GPIO 希望怎样驱动 pin。最终 pin 上的实际电压和电流，还取决于连接的电路以及 MCU 的 electrical limits。

例如，LED 是否点亮取决于 GPIO state 和外围电路是否形成了足够的 LED current。

---
## 6. Example
---

假设一个 GPIO pin 被配置成 push-pull output，并通过 resistor 连接到 LED circuit。

软件把 output state 从 LOW 改成 HIGH 时：

1. GPIO peripheral 保存新的 output state；
2. output driver 改变 pin 的电气电平；
3. 外部 LED circuit 对新的电平作出响应。

因此 LED 的可见变化来自完整的软件到电路路径，而不是某个 C function name 本身。

---
## 7. Related Knowledge
---

- [GPIO Fundamentals](../001-gpio-fundamentals/README.md)
- [GPIO Input](../002-gpio-input/README.md)
- [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md)
- [Output Types](../005-output-types/README.md)
- [GPIO Register Model](../007-gpio-register-model/README.md)

---
## 8. References
---

- STMicroelectronics, [Getting started with GPIO](https://wiki.st.com/stm32mcu/wiki/GPIO_feature_overview)
- STMicroelectronics, [AN4899 — Guidelines for GPIO hardware settings and low-power consumption on STM32 MCUs](https://www.st.com/resource/en/application_note/an4899-guidelines-for-gpio-hardware-settings-and-lowpower-consumption-on-stm32-mcus-stmicroelectronics.pdf)
- STMicroelectronics, [RM0390 — STM32F446xx reference manual](https://www.st.com/resource/en/reference_manual/rm0390-stm32f446xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
