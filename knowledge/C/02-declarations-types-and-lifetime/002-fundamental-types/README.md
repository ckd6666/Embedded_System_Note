# Fundamental Types

---

## 1. Overview

---

C 的基础类型决定一个值如何被表示和解释，以及它可以参与哪些基本操作。

日常 C 程序中，最常见的基础类型可以分成几组：

| 类型组 | 常用类型 | 主要用途 |
| --- | --- | --- |
| 整数 | `short`, `int`, `long`, `long long` 及其 unsigned 版本 | 表示整数 |
| 字符 | `char`, `signed char`, `unsigned char` | 表示字符或小整数；`unsigned char` 也常用于原始字节数据 |
| 布尔 | `bool` | 表示真 / 假 |
| 浮点 | `float`, `double`, `long double` | 表示带小数部分或较大动态范围的数值 |
| 无值 | `void` | 表示“没有值” |

本章的重点是先认识每种类型是什么、通常用来表示什么，以及最基本的使用方式。

---

## 2. Core Concepts

---

### 2.1 Integer Types

整数类型用于表示没有小数部分的数值。

C 中常用的 signed integer types 是：

```c
signed char
short
int
long
long long
```

它们都能够表示负数、零和正数。

其中 `int` 是最常见的普通整数类型：

```c
int temperature = -20;
int count = 10;
```

`short`、`long` 和 `long long` 也是整数类型。它们与 `int` 的主要区别之一是标准规定的最小范围不同：

```c
short small_value = 100;
long distance = 100000L;
long long large_value = 9000000000LL;
```

`short` 是 `short int` 的简写，`long` 是 `long int` 的简写，`long long` 是 `long long int` 的简写。

每个标准 signed integer type 都有对应的 unsigned type：

```c
unsigned char
unsigned short
unsigned int
unsigned long
unsigned long long
```

Unsigned integer types 只表示非负值：

```c
unsigned int retry_count = 3;
unsigned long packet_count = 1000UL;
```

`unsigned` 是 `unsigned int` 的简写。

整数的实际宽度不是由类型名称固定决定的。C 只规定最低范围，并保证：

`sizeof(char) <= sizeof(short) <= sizeof(int) <= sizeof(long) <= sizeof(long long)`

常见最低宽度如下：

| Type | Minimum width |
| --- | ---: |
| `char`, `signed char`, `unsigned char` | 8 bits |
| `short`, `unsigned short` | 16 bits |
| `int`, `unsigned int` | 16 bits |
| `long`, `unsigned long` | 32 bits |
| `long long`, `unsigned long long` | 64 bits |

因此，`int` 不能简单理解成“32-bit integer”。

可以使用 `sizeof` 和 `<limits.h>` 查看当前 implementation 中整数类型的实际大小和范围：

```c
#include <limits.h>

sizeof(int);
INT_MIN;
INT_MAX;
UINT_MAX;
```

整数 promotion、conversion、overflow 和 fixed-width integer types 见 [04-integers-and-bits](../../04-integers-and-bits/)。

---

### 2.2 Character Types

C 有三个不同的 character types：

```c
char
signed char
unsigned char
```

它们都是 integer types，但三个类型彼此不同。

#### `char`

`char` 主要用于表示字符，也是 C 字符串中单个字符元素的类型。

```c
char grade = 'A';
char newline = '\n';
char text[] = "hello";
```

字符常量使用单引号，字符串使用双引号。

Plain `char` 的 signedness 由 implementation 决定，因此 `char` 可能表现为 signed，也可能表现为 unsigned。

#### `signed char`

`signed char` 是明确带符号的 character type，可以表示负数、零和正数。

```c
signed char offset = -10;
signed char delta = 20;
```

它与 plain `char` 是不同类型，即使某个平台上的 `char` 本身表现为 signed。

#### `unsigned char`

`unsigned char` 是明确无符号的 character type，只表示非负值。

```c
unsigned char level = 200;
unsigned char byte = 0xFF;
```

它也常用于表示原始字节数据：

```c
unsigned char packet[4] = { 0x12, 0x34, 0xAB, 0xCD };
```

C 还允许通过 character type 访问 object representation；完整规则见 [07-objects-and-data-layout](../../07-objects-and-data-layout/)。

#### Character Size

C 保证：

`sizeof(char) == 1`

这里的 `1` 表示一个 C byte。

一个 C byte 包含多少 bits 由 `CHAR_BIT` 表示，标准保证 `CHAR_BIT >= 8`。

```c
#include <limits.h>

int bits_per_byte = CHAR_BIT;
```

因此，C 中的一个 byte 不一定等于 8 bits。

---

### 2.3 Boolean Type

Boolean type 用于表示逻辑上的真和假。

在 C23 中可以直接使用：

```c
bool ready = true;
bool error = false;
```

Boolean type 很适合保存条件结果：

```c
int temperature = 85;
bool over_limit = temperature > 80;
```

整数值转换为 Boolean type 时：

- `0` 转换为 `false`
- 非 `0` 值转换为 `true`

例如：

```c
bool a = 0;
bool b = 42;
```

这里 `a` 为 `false`，`b` 为 `true`。

在 C99、C11 和 C17 中，常见写法是通过 `<stdbool.h>` 使用 `bool`、`true` 和 `false`：

```c
#include <stdbool.h>

bool ready = true;
```

这些版本的内建 Boolean type 名称是 `_Bool`。

---

### 2.4 Floating Types

Floating types 用于表示带小数部分或较大动态范围的数值。

C 中最常见的 floating types 是：

```c
float
double
long double
```

#### `float`

`float` 是基本浮点类型之一。

```c
float voltage = 3.3f;
float ratio = 0.5f;
```

带 `f` 或 `F` 后缀的浮点常量具有 `float` 类型。

#### `double`

`double` 是另一种浮点类型。没有后缀的十进制浮点常量默认具有 `double` 类型。

```c
double voltage = 3.3;
double pi = 3.141592653589793;
```

#### `long double`

`long double` 是第三种标准实浮点类型。

```c
long double value = 1.0L;
```

带 `l` 或 `L` 后缀的浮点常量具有 `long double` 类型。

`float`、`double` 和 `long double` 的实际宽度、范围和精度由 implementation 决定，可以通过 `<float.h>` 查看相关限制。

---

### 2.5 `void`

`void` 表示没有可用的值。

最常见的用途是表示函数不返回值：

```c
void reset_device(void)
{
}
```

这里第一个 `void` 表示函数没有返回值。

参数列表中的 `void` 表示该函数不接受参数。

不能定义 `void` 类型的 object：

```c
void value;   /* invalid */
```

`void` 还可以构成 `void *`：

```c
void *buffer;
```

`void *` 是一种可以保存 object pointer 的通用 pointer type。Pointer conversion 和 dereference 规则见 [06-pointers-and-memory](../../06-pointers-and-memory/)。

---

## 3. Related Knowledge

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

## 4. References

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
8. **cppreference — Boolean type and `<stdbool.h>`**  
   https://en.cppreference.com/c/types/boolean
