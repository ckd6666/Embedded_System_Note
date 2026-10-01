# GPIO Alternate Functions

---
## 1. Overview
---

A microcontroller pin can often serve more than one internal function.

An **alternate function** connects a physical pin to an internal peripheral such as a timer, USART, SPI, or I2C controller instead of using the pin only as ordinary GPIO input or output.

This is a form of **pin multiplexing**.

---
## 2. Why Alternate Functions Exist
---

A microcontroller has more internal peripheral signals than can be exposed as dedicated package pins.

Pin multiplexing lets one physical pin support several possible internal functions.

Software selects which function is connected to the pin.

The available choices depend on:

- the microcontroller model;
- the specific pin;
- the package.

A pin cannot be assumed to support an arbitrary peripheral function.

---
## 3. GPIO Mode and Function Selection
---

On STM32 devices, using a non-analog peripheral signal on a GPIO pin normally requires two related configurations:

1. configure the pin for **alternate-function mode**;
2. select which alternate function is connected to that pin.

These are separate decisions.

Alternate-function mode says:

> this pin is controlled by a peripheral path rather than ordinary GPIO output logic.

The alternate-function selection says:

> which peripheral signal is connected to the pin.

---
## 4. Alternate-Function Numbers
---

STM32 devices use alternate-function selections such as `AF0`, `AF1`, through device-supported higher values.

The meaning of an AF number depends on the pin and the device.

For example, on STM32F446, `PA2` can use `AF7` for `USART2_TX`.

This does **not** mean that AF7 always means USART2_TX on every pin. The datasheet's alternate-function mapping table is the authoritative source for the valid mapping.

---
## 5. Signal Direction
---

An alternate-function connection may carry:

- a peripheral output toward the pin;
- a pin input toward the peripheral;
- a bidirectional peripheral signal.

For example:

- a USART TX signal is driven by the USART peripheral toward the pin;
- a USART RX signal is received from the pin by the USART peripheral.

The physical pin is the same kind of package pin, but the selected internal path changes which hardware controls or observes it.

---
## 6. Electrical Configuration Still Matters
---

Selecting an alternate function does not remove the GPIO electrical configuration.

Depending on the peripheral and device, software may still configure:

- push-pull or open-drain output type;
- pull-up or pull-down;
- output speed.

These settings must match the electrical behavior required by the peripheral signal.

See [Output Types](../005-output-types/README.md) and [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md).

---
## 7. Example: USART2 TX on PA2
---

For an STM32F446 USART2 transmit signal on PA2, the relevant configuration is conceptually:

1. choose PA2;
2. put PA2 into alternate-function mode;
3. select the AF mapping that connects PA2 to USART2_TX;
4. configure the USART peripheral.

After that, the USART peripheral produces the TX signal and the GPIO alternate-function routing connects that signal to PA2.

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
