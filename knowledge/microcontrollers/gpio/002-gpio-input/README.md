# GPIO Input

---
## 1. Overview
---

GPIO input mode lets software observe a digital signal present on a microcontroller pin.

The essential path is:

`external voltage -> pin -> GPIO input circuitry -> input state -> software read`

In normal input mode, the GPIO output driver does not actively drive the pin HIGH or LOW. An optional pull-up or pull-down may still weakly bias the pin.

---
## 2. Input Path
---

When a pin is configured as a digital input, its input buffer observes the pin voltage and converts it into a logical LOW or HIGH state.

Software then reads that state through the GPIO peripheral.

On STM32 devices, the current digital input state is exposed through the GPIO input data register, commonly named `GPIOx_IDR`.

The exact electrical voltage thresholds for LOW and HIGH are device-specific and are defined in the datasheet.

---
## 3. Reading an Input
---

Reading a GPIO input means reading the state produced by the input path.

For example, if an external button circuit makes a pin electrically LOW, software reading that pin receives a logical 0. If the circuit makes the pin HIGH, software receives a logical 1.

The software-visible state follows the electrical state of the pin; it is not created by the read operation itself.

---
## 4. Floating Inputs
---

An input pin needs a defined electrical level.

If nothing actively drives the pin and no pull resistor holds it at a known level, the pin can be **floating**. A floating pin may be interpreted unpredictably because small leakage currents, noise, or nearby signals can move its voltage.

Pull-up and pull-down resistors provide a default state when no stronger external source is driving the pin.

See [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md).

---
## 5. Input Mode and Other Pin Functions
---

Configuring a pin as GPIO input means the GPIO input path is the intended digital function.

If the pin is instead assigned to an alternate function, another peripheral may use the same physical pin as its input.

If the pin is configured for analog use, the digital input path may be disabled or bypassed depending on the device.

These modes are configured explicitly because one physical pin can support several possible functions.

---
## 6. Example
---

Suppose a button circuit connects a pin to ground when pressed and otherwise holds it HIGH with a pull-up resistor.

The GPIO input behavior is:

- button released: pin is HIGH, software reads 1;
- button pressed: pin is LOW, software reads 0.

The GPIO peripheral is only reporting the digital level present at the pin.

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
