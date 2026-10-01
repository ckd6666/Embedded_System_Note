# GPIO Input

---
## 1. Overview
---

GPIO input mode 让软件能够读取 MCU pin 上的数字信号。

基本路径是：

`external voltage -> pin -> GPIO input circuitry -> input state -> software read`

当 pin 被配置为普通 digital input 时，GPIO output driver 不会主动把 pin 驱动成 HIGH 或 LOW；如果启用了 pull-up 或 pull-down，它们仍然可以对 pin 提供较弱的偏置。

---
## 2. Input Path
---

当 pin 配置为 digital input 后，input buffer 会观察 pin 上的电压，并把它解释成逻辑 LOW 或 HIGH。

软件再通过 GPIO peripheral 读取这个数字状态。

在 STM32 中，当前 digital input state 通常通过 `GPIOx_IDR`（Input Data Register）提供给软件。

LOW / HIGH 对应的具体电压阈值由器件决定，需要查 datasheet。

---
## 3. Reading an Input
---

读取 GPIO input，本质上是在读取 GPIO input path 已经得到的数字状态。

例如，一个外部按钮电路让 pin 处于 LOW 时，软件读取到逻辑 0；当 pin 处于 HIGH 时，软件读取到逻辑 1。

读取操作本身不会制造这个状态。状态首先来自 pin 上的实际电气电平，然后由 GPIO input circuitry 转换成软件可以读取的数字值。

---
## 4. Floating Inputs
---

Input pin 需要有明确的电气状态。

如果没有任何电路主动驱动 pin，同时也没有 pull resistor 把它保持在某个已知电平，那么这个 pin 就处于 **floating** 状态。

Floating input 的电压可能受到 leakage current、electrical noise 或附近信号的影响，从而在 HIGH 和 LOW 之间不稳定变化。

Pull-up 和 pull-down resistor 的作用，就是在没有更强外部驱动时，为 pin 提供默认状态。

见 [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md)。

---
## 5. Input Mode and Other Pin Functions
---

把 pin 配置成 GPIO input，表示当前主要使用 GPIO 的 digital input path。

如果 pin 被配置成 alternate function，则可能由其他 peripheral 使用同一个物理 pin 作为输入。

如果 pin 被配置成 analog mode，则数字 input path 可能被关闭或绕过，具体行为取决于 MCU。

同一个物理 pin 可以支持多种内部功能，因此必须通过配置明确选择当前用途。

---
## 6. Example
---

假设一个按钮电路使用 pull-up resistor，并在按下时把 pin 接到 ground：

- 按钮松开：pull-up 把 pin 保持在 HIGH，软件读取 1；
- 按钮按下：pin 被拉到 LOW，软件读取 0。

GPIO peripheral 在这里负责报告 pin 当前的数字电平。

---
## 7. Related Knowledge
---

- [GPIO Fundamentals](../001-gpio-fundamentals/README.md)
- [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md)
- [Alternate Functions](../006-alternate-functions/README.md)
- [GPIO Register Model](../007-gpio-register-model/README.md)

---
## 8. References
---

- STMicroelectronics, [Getting started with GPIO](https://wiki.st.com/stm32mcu/wiki/GPIO_feature_overview)
- STMicroelectronics, [AN4899 — Guidelines for GPIO hardware settings and low-power consumption on STM32 MCUs](https://www.st.com/resource/en/application_note/an4899-guidelines-for-gpio-hardware-settings-and-lowpower-consumption-on-stm32-mcus-stmicroelectronics.pdf)
- STMicroelectronics, [RM0390 — STM32F446xx reference manual](https://www.st.com/resource/en/reference_manual/rm0390-stm32f446xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
