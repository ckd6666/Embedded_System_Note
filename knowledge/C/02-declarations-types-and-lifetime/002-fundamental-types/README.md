# Fundamental Types

---

## 1. Overview

---

C 的 type 决定一个值应当如何被解释，以及这个值可以参与哪些操作。

日常嵌入式 C 中，最常见的基础类型可以先分成下面几组：

| 用途 | 常用类型 |
| --- | --- |
| 整数 | `short`, `int`, `long`, `long long` 及其 `unsigned` 版本 |
| 字符与字节数据 | `char`, `signed char`, `unsigned char` |
| 真 / 假 | `bool` |
| 浮点数 | `float`, `double`, `long double` |
| 表示“没有值” | `void` |

学习这些类型时，最重要的不是记住一张类型名称表，而是理解：

- signed 与 unsigned 表示的值域不同；
- `char`、`signed char`、`unsigned char` 是三个不同类型；
- `int`、`long` 等类型的实际宽度由 implementation 决定；
- `sizeof(char)` 永远是 `1`，但一个 C byte 不一定等于 8 bits；
- 浮点类型的实际精度、表示和执行成本依赖实现和目标平台；
- `void` 表示没有可用的值，不能用来定义普通 object。

---

## 2. Core Concepts

---

### 2.1 Integer Types

C 提供以下常用 signed integer types：

```c
signed char
short
int
long
long long
```

除 `bool` 外，每个标准 signed integer type 都有对应的 unsigned type：

```c
unsigned char
unsigned short
unsigned int
unsigned long
unsigned long long
```

`signed` 类型能够表示负值、零和正值；对应的 `unsigned` 类型只表示非负值。

例如：

```c
int temperature = -20;
unsigned int retry_count = 3;
```

`short` 是 `short int` 的简写，`long` 是 `long int` 的简写，`unsigned` 是 `unsigned int` 的简写。

整数运算中的 promotion、conversion、overflow 和 signed/unsigned 混合运算属于 [04-integers-and-bits](../../04-integers-and-bits/)，本章先建立类型本身的基本认识。

---

### 2.2 Integer Width Is Implementation-Defined

不能从 `short`、`int`、`long` 这些名称直接推出固定 bit width。

C 只规定这些标准整数类型至少具有下面的宽度：

| Type | Minimum width |
| --- | ---: |
| `char`, `signed char`, `unsigned char` | 8 bits |
| `short`, `unsigned short` | 16 bits |
| `int`, `unsigned int` | 16 bits |
| `long`, `unsigned long` | 32 bits |
| `long long`, `unsigned long long` | 64 bits |

并保证：

`sizeof(char) <= sizeof(short) <= sizeof(int) <= sizeof(long) <= sizeof(long long)`

因此下面这种假设并不由 C 标准保证：

```c
/* 不要假设 sizeof(int)  * CHAR_BIT == 32 */
/* 不要假设 sizeof(long) * CHAR_BIT == 32 */
```

在实际工程中，应通过目标编译器和 ABI 文档、`sizeof`、`<limits.h>` 等确认平台上的实际范围和宽度。

需要明确宽度的整数时，相关类型和规则见 [04-integers-and-bits](../../04-integers-and-bits/)。

---

### 2.3 Character Types

C 中存在三个不同的 character types：

```c
char
signed char
unsigned char
```

`char` 与另外两个类型都不相同。

Plain `char` 最终表现为 signed 还是 unsigned 由 implementation 决定。因此，当代码的正确性依赖负值范围或完整的非负字节范围时，不应仅依赖 plain `char` 的 signedness。

另一个重要规则是：

`sizeof(char) == 1`

这里的 `1` 表示 **一个 C byte**。一个 byte 包含多少 bits 由 `CHAR_BIT` 决定，标准保证 `CHAR_BIT >= 8`。

因此：

```text
1 byte in C != necessarily 8 bits
```

`unsigned char` 还可以用于访问 object 的原始 byte representation。完整规则见 [07-objects-and-data-layout](../../07-objects-and-data-layout/)。

---

### 2.4 Boolean Type

Boolean type 用于表示逻辑上的真和假。

在 C23 中：

```c
bool ready = true;
bool error = false;
```

`bool`、`true` 和 `false` 可以直接作为语言提供的名称使用。

在大量仍使用 C99、C11 或 C17 的嵌入式代码中，常见写法是：

```c
#include <stdbool.h>

bool ready = true;
```

这些版本的核心 Boolean type 名称是 `_Bool`，而 `<stdbool.h>` 提供常用的 `bool`、`true` 和 `false`。

---

### 2.5 Floating Types

常用的 floating types 是：

```c
float
double
long double
```

它们用于表示具有小数部分或较大动态范围的数值。

通常可以把它们理解为不同精度级别的浮点类型，但不能仅凭类型名称假定：

- 固定 bit width；
- 一定使用某个 IEEE 754 format；
- 某种类型一定由硬件直接支持；
- `double` 在所有 MCU 上都具有相同的执行成本。

实际范围和精度可以通过 `<float.h>` 以及目标编译器、ABI 和处理器文档确认。

```c
float voltage = 3.3f;
double result = 0.0;
```

---

### 2.6 `void`

`void` 表示没有可用的值。

最常见的用途之一是表示函数不返回值：

```c
void reset_device(void);
```

不能定义 `void` 类型的普通 object：

```c
void value;   // invalid
```

`void` 还用于构成 `void *`。Pointer 和 function 的完整规则分别见 [06-pointers-and-memory](../../06-pointers-and-memory/) 和 [08-functions-and-api](../../08-functions-and-api/)。

---

## 3. Important Distinctions

---

### 3.1 Type Name Does Not Mean Fixed Width

`int` 并不等于“32-bit integer”，`long` 也不等于“32-bit integer”。

类型的实际宽度属于 implementation 的选择。

---

### 3.2 `char` Is Not the Same as `signed char`

即使某个平台上的 plain `char` 表现为 signed，`char` 和 `signed char` 仍然是不同类型。

同理，plain `char` 也不等同于 `unsigned char`。

---

### 3.3 Signedness Is Part of the Type

`int` 和 `unsigned int` 是不同类型。

它们不仅值域不同，参与表达式运算时的转换和算术规则也不同。完整规则见 [04-integers-and-bits](../../04-integers-and-bits/)。

---

## 4. Related Knowledge

---

- [001-variables-declarations-and-definitions](../001-variables-declarations-and-definitions/) — type 在 declaration 中的作用
- [003-initialization](../003-initialization/) — object 的 initialization
- [004-typedef-and-enum](../004-typedef-and-enum/) — type aliases 与 enumerated types
- [005-const-and-type-qualifiers](../005-const-and-type-qualifiers/) — qualified types
- [04-integers-and-bits](../../04-integers-and-bits/) — integer widths, conversions, overflow, and fixed-width integer types
- [06-pointers-and-memory](../../06-pointers-and-memory/) — pointer types and `void *`
- [07-objects-and-data-layout](../../07-objects-and-data-layout/) — object representation
- [08-functions-and-api](../../08-functions-and-api/) — function types and `void` return type

---

## 5. References

---

1. **ISO/IEC 9899:2024 (C23), 6.2.5 Types** — type categories and fundamental type properties.
2. **ISO/IEC 9899:2024 (C23), 5.2.4.2 Numerical limits** — minimum ranges and implementation limits.
3. **ISO/IEC 9899:2024 (C23), 6.7.2 Type specifiers** — type specifier rules.
4. **cppreference — Arithmetic types**  
   https://en.cppreference.com/c/language/arithmetic_types
5. **cppreference — Numeric limits**  
   https://en.cppreference.com/c/types/limits
6. **cppreference — `<limits.h>`**  
   https://en.cppreference.com/c/header/limits
7. **cppreference — `<float.h>`**  
   https://en.cppreference.com/c/header/float
