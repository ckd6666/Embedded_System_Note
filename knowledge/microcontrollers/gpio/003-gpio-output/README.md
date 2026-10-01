# GPIO Output

---
## 1. Overview
---

GPIO output mode lets the microcontroller drive a digital state onto a pin.

The essential path is:

`software output state -> GPIO output logic -> pin driver -> pin voltage`

Software changes the GPIO output state; the GPIO peripheral turns that state into electrical behavior at the pin.

---
## 2. Output State
---

A digital GPIO output normally has two logical states:

- **LOW** — logic 0.
- **HIGH** — logic 1.

On STM32 devices, the intended output state is stored by the GPIO peripheral. The output circuitry then drives the corresponding pin according to that state and the configured output type.

The exact output voltage that satisfies LOW or HIGH depends on supply voltage, output load, and device electrical characteristics.

---
## 3. Setting, Clearing, and Toggling
---

Common GPIO output operations are:

- **set** — make the output state HIGH;
- **clear/reset** — make the output state LOW;
- **toggle** — change HIGH to LOW or LOW to HIGH.

Toggling is not a special electrical mode. It is a software operation that changes the stored output state to the opposite value.

On STM32 devices, output state is associated with registers such as `GPIOx_ODR` and `GPIOx_BSRR`. Their details are covered in [GPIO Register Model](../007-gpio-register-model/README.md).

---
## 4. Output Mode Is Not Enough by Itself
---

Configuring a pin as output selects the GPIO output path, but the electrical behavior also depends on output configuration.

Important output properties include:

- **push-pull or open-drain output type**;
- **output speed / slew-rate setting**;
- optional **pull-up or pull-down** configuration.

These are covered in [Output Types](../005-output-types/README.md) and [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md).

---
## 5. Output State and Physical Load
---

The GPIO pin does not exist in isolation. It drives an external electrical load such as:

- an LED circuit;
- another digital input;
- an enable pin;
- a transistor or logic gate.

Changing the software output state only guarantees the intended GPIO drive behavior. The resulting voltage and current must still satisfy the electrical requirements of the connected circuit.

For example, an LED turns on only if the GPIO state and the surrounding circuit create sufficient current through the LED.

---
## 6. Example
---

Suppose a GPIO pin is configured as a push-pull output and connected through a resistor to an LED circuit.

If software changes the output state from LOW to HIGH:

1. the GPIO peripheral stores the new output state;
2. the output driver changes the pin's electrical level;
3. the external LED circuit responds to that new level.

The visible LED behavior comes from the whole software-to-circuit path, not from the C function name itself.

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
