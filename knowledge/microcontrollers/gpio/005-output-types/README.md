# GPIO Output Types

---
## 1. Overview
---

When a GPIO pin is used as an output, its **output type** defines how the pin's driver produces LOW and HIGH electrical states.

Two common output types are:

- **push-pull**;
- **open-drain**.

Output type is different from output mode. Output mode says that the pin is being used as a GPIO output; output type defines how the electrical driver behaves.

---
## 2. Push-Pull Output
---

A push-pull output can actively drive the pin in both directions:

- drive toward HIGH;
- drive toward LOW.

This is the common choice when the microcontroller should directly produce both digital states.

The basic behavior is:

- output state HIGH -> the output stage drives the pin high;
- output state LOW -> the output stage drives the pin low.

The exact output voltage depends on the device supply, load current, and electrical characteristics.

---
## 3. Open-Drain Output
---

An open-drain output actively drives only one direction:

- LOW is actively driven;
- HIGH is not actively driven by the output stage.

When the output is released, a pull-up can bring the line HIGH.

The basic behavior is therefore:

- output active -> pin pulled LOW;
- output released -> pull-up determines the HIGH level.

Open-drain outputs are useful when multiple devices must share one signal without each device actively driving the line HIGH.

I2C commonly uses this style of signaling, but the I2C protocol itself is outside the scope of this chapter.

---
## 4. Pull Resistors and Open-Drain
---

An open-drain output normally needs a pull-up somewhere in the circuit if the line must reach a valid HIGH state.

The pull-up may be:

- internal to the microcontroller;
- external on the board.

The required resistance depends on the electrical and timing requirements of the circuit.

See [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md).

---
## 5. Output Type and Output State
---

Output type and output state are separate concepts.

The output state answers:

> Should the GPIO currently represent LOW or HIGH?

The output type answers:

> How should the output driver electrically produce that state?

For push-pull, both LOW and HIGH are actively driven.

For open-drain, LOW is actively driven and HIGH normally depends on a pull-up.

Output speed is another independent GPIO output setting and is introduced in [GPIO Output](../003-gpio-output/README.md).

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
