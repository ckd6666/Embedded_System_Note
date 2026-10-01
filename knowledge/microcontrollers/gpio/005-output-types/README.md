# GPIO Output Types

---
## 1. Overview
---

GPIO pin 作为 output 时，**output type** 决定 output driver 用什么电气方式产生 LOW 和 HIGH。

常见的两种 output type 是：

- **push-pull**；
- **open-drain**。

Output type 和 output mode 不是同一个概念。

Output mode 表示这个 pin 当前用作 GPIO output；output type 则决定 output driver 具体怎样产生电平。

---
## 2. Push-Pull Output
---

Push-pull output 可以主动向两个方向驱动 pin：

- 主动驱动到 HIGH；
- 主动驱动到 LOW。

当 MCU 需要直接产生两个数字状态时，push-pull 是最常见的方式。

基本行为是：

- output state = HIGH → output stage 主动把 pin 拉高；
- output state = LOW → output stage 主动把 pin 拉低。

实际输出电压仍然取决于 supply、load current 和器件 electrical characteristics。

---
## 3. Open-Drain Output
---

Open-drain output 只主动驱动一个方向：

- LOW：主动驱动；
- HIGH：output stage 不主动驱动。

当 output 被释放时，需要 pull-up 把 signal 拉到 HIGH。

基本行为是：

- output active → pin 被拉到 LOW；
- output released → HIGH 由 pull-up 决定。

Open-drain 很适合多个 device 共享一条 signal，因为各 device 都不会主动把 line 驱动到 HIGH。

I2C 常使用这种 signaling，但 I2C protocol 本身不属于本章范围。

---
## 4. Pull Resistors and Open-Drain
---

如果 open-drain line 需要得到有效 HIGH，通常必须在电路中的某处存在 pull-up。

Pull-up 可以来自：

- MCU internal pull-up；
- board 上的 external pull-up。

需要多大的 resistance，取决于具体电路的 electrical 和 timing 要求。

见 [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md)。

---
## 5. Output Type and Output State
---

Output type 和 output state 是两个独立概念。

Output state 回答：

> 当前 GPIO 希望表示 LOW 还是 HIGH？

Output type 回答：

> Output driver 应该怎样用电气方式实现这个状态？

对于 push-pull，LOW 和 HIGH 都由 output stage 主动驱动。

对于 open-drain，LOW 主动驱动，而 HIGH 通常依赖 pull-up。

Output speed 是另一个独立的 GPIO output 设置，见 [GPIO Output](../003-gpio-output/README.md)。

---
## 6. Related Knowledge
---

- [GPIO Fundamentals](../001-gpio-fundamentals/README.md)
- [GPIO Output](../003-gpio-output/README.md)
- [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md)
- [GPIO Register Model](../007-gpio-register-model/README.md)

---
## 7. References
---

- STMicroelectronics, [AN4899 — Guidelines for GPIO hardware settings and low-power consumption on STM32 MCUs](https://www.st.com/resource/en/application_note/an4899-guidelines-for-gpio-hardware-settings-and-lowpower-consumption-on-stm32-mcus-stmicroelectronics.pdf)
- STMicroelectronics, [RM0390 — STM32F446xx reference manual](https://www.st.com/resource/en/reference_manual/rm0390-stm32f446xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
- STMicroelectronics, [Getting started with GPIO](https://wiki.st.com/stm32mcu/wiki/GPIO_feature_overview)
