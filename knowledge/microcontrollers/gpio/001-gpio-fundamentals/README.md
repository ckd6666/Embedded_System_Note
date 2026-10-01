# GPIO Fundamentals

---
## 1. Overview
---

GPIO stands for **General-Purpose Input/Output**. A GPIO pin is a microcontroller pin whose digital function can be configured by software rather than being fixed to one dedicated purpose.

GPIO is one of the main interfaces between software and the physical world. Software configures and reads or writes GPIO state; the GPIO peripheral turns that state into electrical behavior at a pin.

---
## 2. Ports and Pins
---

GPIO pins are commonly grouped into **ports**.

On STM32 devices, names such as `PA5` and `PC13` mean:

- `PA5`: Port A, pin 5.
- `PC13`: Port C, pin 13.

A port groups several pins under one GPIO peripheral. Software normally selects both the port and the pin when configuring or accessing GPIO.

The exact number of ports and pins depends on the microcontroller and package.

---
## 3. Digital Levels
---

A digital GPIO works with logical states usually written as:

- **LOW** — logic 0.
- **HIGH** — logic 1.

These are logical states, not universal voltage values. The actual voltage ranges accepted as LOW or HIGH are defined by the device's electrical characteristics.

When a GPIO is used as an output, the output circuitry drives the pin toward a LOW or HIGH electrical level.

When a GPIO is used as an input, the input circuitry interprets the pin voltage as a digital state.

---
## 4. GPIO Modes
---

A GPIO pin is normally configured for one of several roles.

### 4.1 Input

The pin receives a digital signal from outside the microcontroller.

See [GPIO Input](../002-gpio-input/README.md).

---

### 4.2 Output

The microcontroller drives a digital level onto the pin.

See [GPIO Output](../003-gpio-output/README.md).

---

### 4.3 Alternate Function

The pin is connected internally to another peripheral, such as a timer, USART, SPI, or I2C controller.

See [Alternate Functions](../006-alternate-functions/README.md).

---

### 4.4 Analog

The digital GPIO path is not used as the primary signal path. This mode is commonly used when the pin belongs to an analog peripheral such as an ADC or DAC.

---
## 5. GPIO as a Software-Hardware Interface
---

GPIO is controlled through hardware state exposed to software. On STM32, this includes configuration and data registers belonging to each GPIO port.

Software changes those registers; the GPIO peripheral changes how the corresponding pins behave.

This relationship is the basic bridge:

`software -> GPIO peripheral state -> pin electrical behavior`

The register model is covered separately in [GPIO Register Model](../007-gpio-register-model/README.md).

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
