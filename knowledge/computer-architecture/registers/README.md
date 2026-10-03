# Registers

本目录用于系统学习嵌入式系统中的 **Register（寄存器）**。

这里的重点不是某一个 STM32 peripheral 的具体 register，而是建立可以复用于 GPIO、USART、Timer、ADC 等外设的通用 register 模型。

## Scope

建议按下面的知识条目逐步学习：

```text
registers/
├── README.md
├── 001-register-fundamentals/
├── 002-cpu-registers/
├── 003-peripheral-registers/
├── 004-memory-mapped-io/
├── 005-register-fields-and-access-semantics/
└── 006-reading-register-documentation/
```

各条目的主要职责：

- `001-register-fundamentals` — Register 是什么、bit width、保存硬件状态的基本模型。
- `002-cpu-registers` — CPU registers，例如 general-purpose registers、SP、PC、status register。
- `003-peripheral-registers` — Peripheral 的 control、status、data、configuration registers。
- `004-memory-mapped-io` — CPU 为什么能通过 address space 访问 peripheral registers。
- `005-register-fields-and-access-semantics` — bit field、reset value、RO/RW/WO、W1C、reserved bits 等 register 语义。
- `006-reading-register-documentation` — 如何阅读 reference manual 中的 register map、offset、access type 和 field description。

## Boundaries

C 语言中如何通过 pointer、`volatile`、mask、read-modify-write 等方式访问 register，主要属于：

`knowledge/C/10-hardware-access/`

具体 peripheral 的 register 结构属于对应 peripheral 知识，例如：

`knowledge/microcontrollers/gpio/007-gpio-register-model/`

本目录负责通用的 Register / Computer Architecture 基础，不重复这些章节的完整内容。
