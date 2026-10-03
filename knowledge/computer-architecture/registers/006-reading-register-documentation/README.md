# Reading Register Documentation

---
## 1. Overview
---

学习 peripheral register 时，最重要的资料通常是 MCU 的 **reference manual**。

Reference manual 会告诉你：

- peripheral 在 memory map 中的位置；
- register 名称和 address offset；
- register reset value；
- 每个 bit / bit field 的位置；
- access type；
- 每个 value 的硬件含义；
- read / write 是否有特殊副作用。

阅读 register 文档的目标不是背 register，而是能够回答：

> 这个 Register 控制什么？我要修改哪几个 bit？写入后 hardware 会怎样？

---
## 2. Start From the Peripheral Chapter
---

先找到对应 peripheral 的章节，例如：

- GPIO；
- RCC；
- USART；
- Timer。

不要只搜索某个 register name 后直接修改代码。

Peripheral chapter 前面的 functional description 会说明这个 peripheral 的整体工作方式，后面的 register description 才能放到正确上下文中理解。

---
## 3. Find the Register Map
---

Peripheral 章节通常会提供 **register map**。

Register map 常见信息包括：

| 项目 | 含义 |
| --- | --- |
| Register name | Register 名称 |
| Offset | 相对 peripheral base address 的偏移 |
| Reset value | reset 后的初始值 |
| Access | read / write 权限 |
| Fields | Register 内不同 bit 的含义 |

如果已经知道 peripheral base address，则 register address 可以由：

`register address = peripheral base address + register offset`

得到。

实际编程时，library 或 device header 通常已经提供这些 address 定义，不需要人工计算每一次访问地址。

---
## 4. Read the Register Header
---

打开某个具体 Register description 时，先看几个基本信息：

### Register name

例如：

`GPIOx_MODER`

这里的 `x` 表示不同 GPIO Port，例如 GPIOA、GPIOB。

### Address offset

例如：

`Address offset: 0x00`

表示这个 Register 位于 GPIO peripheral base address 加 `0x00` 的位置。

### Reset value

例如：

`Reset value: ...`

表示对应 reset 后 Register 的初始状态。

不同 Port 可能具有不同 reset value，因此要看 reference manual 对具体 device 的说明。

---
## 5. Read the Bit Layout
---

Register description 通常会把 32-bit 或其他宽度 Register 展开成 bit fields。

例如 GPIO mode register 中，一个 pin 可能使用 2-bit field：

```text
MODER5[1:0]
```

这表示 PA5 / PB5 等对应 pin 5 的 mode 由这个 2-bit field 决定。

随后文档会定义不同 bit pattern，例如：

```text
00 -> Input
01 -> Output
10 -> Alternate function
11 -> Analog
```

真正需要记住的是“field 控制什么”和“当前任务需要哪个 value”，不是整张表的所有 bit。

---
## 6. Check Access Semantics
---

在 write 任何 register 或 field 之前，必须确认 access semantics。

需要检查：

- 是 RO、WO 还是 RW；
- write 是否直接保存值；
- 是否 W1C；
- read 是否会产生 side effect；
- hardware 是否会自动修改；
- reserved bits 应怎样处理。

如果文档对某个 field 有专门的 note，那个 note 属于 register 语义的一部分，不能忽略。

见 [Register Fields and Access Semantics](../005-register-fields-and-access-semantics/README.md)。

---
## 7. Use Reset Value as the Starting State
---

理解初始化代码时，可以把 reset value 当成 hardware 初始状态的基线。

例如：

1. 先看 reset 后 mode field 是什么；
2. 再看 initialization code 修改了哪些 field；
3. 对比修改前后 peripheral state 的变化。

这样可以回答：

> 这行初始化代码到底改变了什么？

而不是只记住：

> 这个 API 必须调用。

---
## 8. Connect the Manual Back to Code
---

读完 register description 后，要回到实际代码。

例如代码：

```c
gpio_mode_setup(GPIOA, GPIO_MODE_OUTPUT, GPIO_PUPD_NONE, GPIO5);
```

如果继续向下追，可以去 library source 查看它修改了哪些 GPIO registers，再回到 RM0390 查对应 register 和 field 的硬件含义。

常见学习路径是：

`application code -> library source -> register name -> reference manual -> hardware meaning`

这条路径能够把 API 和真正的 MCU hardware mechanism 连接起来。

---
## 9. Reference Manual, Datasheet, and Programming Manual
---

不同文档负责不同层次的信息。

| 文档 | 主要用途 |
| --- | --- |
| Reference Manual | Peripheral、register、memory map、hardware behavior |
| Datasheet | Pin function、alternate function、电气参数、package、device limits |
| Programming Manual | Processor core、instruction、CPU registers、exception model 等 |

例如：

- 查 `GPIOA_MODER`：Reference Manual；
- 查 PA2 是否支持 USART2_TX AF7：Datasheet；
- 查 `R13 / SP`、`R15 / PC`：Cortex-M4 Programming Manual。

选对文档比在所有官方资料中同时搜索更重要。

---
---
## 10. Course Resources
---

阅读 register 文档更适合跟着真实 STM32 文档示范一次，再回到 reference manual 自己查。

- **正点原子 STM32 HAL 库开发课程 — 第 4、16–18 讲**  
  Bilibili：https://www.bilibili.com/video/BV1bv4y1R7dp/  
  中文讲解。第 4 讲用于学习查看数据手册；第 16–18 讲用于理解存储器映射和寄存器映射。
## 11. Related Knowledge
---

- [Register Fundamentals](../001-register-fundamentals/README.md)
- [CPU Registers](../002-cpu-registers/README.md)
- [Peripheral Registers](../003-peripheral-registers/README.md)
- [Memory-Mapped I/O](../004-memory-mapped-io/README.md)
- [Register Fields and Access Semantics](../005-register-fields-and-access-semantics/README.md)

---
## 12. References
---

- STMicroelectronics, [RM0390 — STM32F446xx reference manual](https://www.st.com/resource/en/reference_manual/rm0390-stm32f446xx-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
- STMicroelectronics, [PM0214 — STM32 Cortex-M4 MCUs and MPUs programming manual](https://www.st.com/resource/en/programming_manual/dm00046982-stm32-cortex-m4-mcus-and-mpus-programming-manual-stmicroelectronics.pdf)
- STMicroelectronics, [STM32F446 documentation page](https://www.st.com/en/microcontrollers-microprocessors/stm32f446/documentation.html)
- Arm, [CMSIS-SVD register description model](https://arm-software.github.io/CMSIS_5/SVD/html/svd_Example_pg.html)
