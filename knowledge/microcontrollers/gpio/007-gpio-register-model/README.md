# GPIO Register Model

---
## 1. Overview
---

Microcontroller GPIO is controlled through hardware registers that software can read and write.

On STM32, GPIO registers are **memory-mapped**: each register is assigned an address in the processor's address space. A C library or driver ultimately accesses those addresses, and the GPIO peripheral reacts to the register values.

The core bridge is:

`C / library call -> memory-mapped GPIO register -> GPIO hardware behavior`

---
## 2. Memory-Mapped I/O
---

With memory-mapped I/O, peripheral registers are accessed using the processor's normal memory load and store operations.

The address does not refer to ordinary RAM. It refers to hardware state inside a peripheral.

For GPIO, software can therefore:

- write configuration bits;
- write output state;
- read input state.

The processor performs a memory access, while the peripheral interprets that access as a hardware control or status operation.

---
## 3. One Port, Several Registers
---

A GPIO port contains several registers because different parts of pin behavior need independent configuration.

For STM32F446 GPIO ports, the main registers include:

| Register | Main role |
| --- | --- |
| `GPIOx_MODER` | Selects input, output, alternate-function, or analog mode |
| `GPIOx_OTYPER` | Selects push-pull or open-drain output type |
| `GPIOx_OSPEEDR` | Selects output speed |
| `GPIOx_PUPDR` | Selects no pull, pull-up, or pull-down |
| `GPIOx_IDR` | Reports current digital input state |
| `GPIOx_ODR` | Holds GPIO output data |
| `GPIOx_BSRR` | Sets or resets output bits without a read-modify-write sequence |
| `GPIOx_AFRL/H` | Selects alternate functions for pins |

The exact register names and layout are device-specific, but the separation between configuration, input state, output state, and peripheral routing is common in microcontroller GPIO designs.

---
## 4. Configuration Registers
---

Configuration registers determine how a pin behaves before normal input or output operations occur.

For example, on STM32F446:

- `MODER` chooses the pin's basic mode;
- `OTYPER` chooses push-pull or open-drain when an output driver is involved;
- `OSPEEDR` selects the output speed;
- `PUPDR` configures internal pull-up or pull-down resistors;
- `AFRL` and `AFRH` select alternate-function routing.

Changing these registers changes the hardware configuration of the pin.

---
## 5. Input Data Register
---

`GPIOx_IDR` exposes the digital state observed by the GPIO input path.

Each relevant bit corresponds to a pin in the port.

Reading the bit does not create the input state. The electrical level at the pin is interpreted by the GPIO input circuitry, and the resulting digital state is reflected in the register.

See [GPIO Input](../002-gpio-input/README.md).

---
## 6. Output Data Register
---

`GPIOx_ODR` stores GPIO output data.

For an output pin, the corresponding bit represents the intended output state used by the GPIO output path.

For example, if bit 5 of GPIOA's output data is changed, the GPIOA output logic for PA5 receives that new state, subject to the pin's configured mode and output type.

See [GPIO Output](../003-gpio-output/README.md).

---
## 7. Bit Set/Reset Register
---

STM32 also provides `GPIOx_BSRR` for setting or resetting selected output bits.

This allows software to request bit changes without first reading the complete output data register and then writing a modified value back.

That matters when different code paths may modify different GPIO bits, because a read-modify-write sequence can otherwise create unwanted interference.

The exact set/reset bit layout is defined in the device reference manual.

---
## 8. Alternate-Function Registers
---

STM32 GPIO ports use alternate-function registers to select which peripheral signal is connected to a pin.

For STM32F446:

- `AFRL` covers the lower-numbered pins in a port;
- `AFRH` covers the higher-numbered pins.

The selected AF value must match a function that the device datasheet lists as valid for that pin.

See [GPIO Alternate Functions](../006-alternate-functions/README.md).

---
## 9. From API to Register
---

A library API hides register syntax but does not bypass the register model.

For example, a call that configures a GPIO pin as output eventually changes the appropriate mode bits in the GPIO peripheral.

A call that reads a GPIO input eventually reads the peripheral's input state.

A call that changes an output eventually updates GPIO output state through the relevant register interface.

This is why understanding the register model explains what GPIO APIs actually control without requiring application code to manipulate raw addresses directly.

---
## 10. Related Knowledge
---

- [GPIO Fundamentals](../001-gpio-fundamentals/README.md)
- [GPIO Input](../002-gpio-input/README.md)
- [GPIO Output](../003-gpio-output/README.md)
- [Pull-Up and Pull-Down](../004-pull-up-and-pull-down/README.md)
- [Output Types](../005-output-types/README.md)
- [Alternate Functions](../006-alternate-functions/README.md)

---
## 11. References
---

- STMicroelectronics, [RM0390 — STM32F446xx reference manual](https://www.st.com/resource/en/reference_manual/rm0390-stm32f446xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
- STMicroelectronics, [DS10693 — STM32F446xC/E datasheet](https://www.st.com/resource/en/datasheet/stm32f446re.pdf)
- STMicroelectronics, [AN4899 — Guidelines for GPIO hardware settings and low-power consumption on STM32 MCUs](https://www.st.com/resource/en/application_note/an4899-guidelines-for-gpio-hardware-settings-and-lowpower-consumption-on-stm32-mcus-stmicroelectronics.pdf)
