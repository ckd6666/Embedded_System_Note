# Pull-Up and Pull-Down

---
## 1. Overview
---

A digital input needs a defined voltage to be read reliably as LOW or HIGH.

A **pull-up** or **pull-down** resistor gives the pin a default electrical state when no stronger external source is driving it.

- pull-up: weakly biases the pin toward HIGH;
- pull-down: weakly biases the pin toward LOW.

---
## 2. Why Pull Resistors Are Needed
---

If an input pin is left electrically unconnected, its voltage may not stay at a stable level. The pin is then **floating**.

A floating input can change state because of leakage current, electrical noise, or nearby signals.

A pull resistor prevents this by giving the pin a weak default connection to a supply rail.

---
## 3. Pull-Up
---

A pull-up resistor connects the signal weakly toward the positive supply.

When nothing else drives the line, the pin tends to read HIGH.

A common button circuit uses a pull-up and a switch to ground:

- switch open: the pull-up holds the pin HIGH;
- switch closed: the switch provides a stronger path to ground, so the pin becomes LOW.

The pull-up does not force the pin HIGH under all conditions. It provides a weak default state that another circuit can override.

---
## 4. Pull-Down
---

A pull-down resistor connects the signal weakly toward ground.

When nothing else drives the line, the pin tends to read LOW.

If an external circuit actively drives the signal HIGH, that stronger drive overrides the pull-down.

---
## 5. Internal and External Pull Resistors
---

Many microcontrollers provide configurable **internal** pull-up and pull-down resistors.

They are convenient when a weak default state is sufficient and an external resistor is not required by the circuit.

External pull resistors are still used when the circuit needs a specific resistance, timing behavior, current level, or electrical requirement.

The resistance of an internal pull resistor is device-specific and must be taken from the microcontroller documentation when its value matters.

---
## 6. Pull Resistor vs Active Output
---

A pull resistor is a weak bias, not the same as actively driving a pin.

A push-pull output actively drives HIGH or LOW through its output stage.

A pull-up or pull-down only establishes a default level when stronger drivers are absent.

This distinction becomes especially important with open-drain outputs.

See [Output Types](../005-output-types/README.md).

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
