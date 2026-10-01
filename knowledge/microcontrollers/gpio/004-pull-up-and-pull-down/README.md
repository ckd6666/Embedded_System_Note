# Pull-Up and Pull-Down

---
## 1. Overview
---

Digital input 需要一个明确的电压状态，才能稳定地被读取为 LOW 或 HIGH。

**Pull-up** 和 **pull-down** resistor 用来在没有更强外部驱动时，为 pin 提供默认电气状态：

- pull-up：弱地把 pin 偏向 HIGH；
- pull-down：弱地把 pin 偏向 LOW。

---
## 2. Why Pull Resistors Are Needed
---

如果 input pin 没有连接到明确的电气驱动源，它的电压可能无法稳定在某个状态，这种情况称为 **floating**。

Floating input 可能因为 leakage current、electrical noise 或附近信号而改变电压，从而导致数字读取结果不稳定。

Pull resistor 通过给 pin 提供一个较弱的默认连接，避免 pin 长时间处于不确定状态。

---
## 3. Pull-Up
---

Pull-up resistor 把 signal 弱地连接到 positive supply。

当没有其他电路驱动这条线时，pin 会倾向于保持 HIGH。

常见按钮电路会使用 pull-up resistor，并让 switch 在按下时接地：

- switch open：pull-up 把 pin 保持在 HIGH；
- switch closed：switch 提供更强的 ground path，pin 变成 LOW。

Pull-up 并不是无条件强制 HIGH，而是在没有更强驱动时提供默认 HIGH。

---
## 4. Pull-Down
---

Pull-down resistor 把 signal 弱地连接到 ground。

当没有其他电路驱动时，pin 会倾向于保持 LOW。

如果外部电路主动把 signal 驱动到 HIGH，这个更强的 drive 会覆盖 pull-down 的默认作用。

---
## 5. Internal and External Pull Resistors
---

很多 MCU 都提供可配置的 **internal pull-up** 和 **internal pull-down** resistor。

当只需要一个弱的默认状态时，internal pull 很方便。

如果电路对 resistance、timing、current 或其他 electrical characteristic 有明确要求，则仍然可能需要 external pull resistor。

Internal pull resistor 的实际 resistance 是 device-specific，具体数值要查对应 MCU 文档。

---
## 6. Pull Resistor vs Active Output
---

Pull resistor 是一种较弱的 bias，不等于 active output drive。

Push-pull output 会通过 output stage 主动驱动 HIGH 或 LOW。

Pull-up / pull-down 只是在没有更强驱动时建立默认电平。

这个区别在 open-drain output 中尤其重要。

见 [Output Types](../005-output-types/README.md)。

---
## 7. Related Knowledge
---

- [GPIO Fundamentals](../001-gpio-fundamentals/README.md)
- [GPIO Input](../002-gpio-input/README.md)
- [GPIO Output](../003-gpio-output/README.md)
- [Output Types](../005-output-types/README.md)

---
## 8. References
---

- STMicroelectronics, [Getting started with GPIO](https://wiki.st.com/stm32mcu/wiki/GPIO_feature_overview)
- STMicroelectronics, [AN4899 — Guidelines for GPIO hardware settings and low-power consumption on STM32 MCUs](https://www.st.com/resource/en/application_note/an4899-guidelines-for-gpio-hardware-settings-and-lowpower-consumption-on-stm32-mcus-stmicroelectronics.pdf)
- STMicroelectronics, [RM0390 — STM32F446xx reference manual](https://www.st.com/resource/en/reference_manual/rm0390-stm32f446xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
