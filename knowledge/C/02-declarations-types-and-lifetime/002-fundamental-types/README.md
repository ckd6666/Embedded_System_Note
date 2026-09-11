# Fundamental Types

---

## 1. Definition

---

本仓库使用 **fundamental types** 作为章节名称；ISO C 的正式分类中，`void` 单独列出，其余内建基础类型主要归入 **basic types**。

本条目聚焦嵌入式 C 中最常用的基础类型：

| Category | Types |
| --- | --- |
| Boolean | `bool`；`_Bool` 是 C23 中保留但已弃用的兼容拼写 |
| Character | `char`, `signed char`, `unsigned char` |
| Standard signed integer | `signed char`, `short int`, `int`, `long int`, `long long int` |
| Standard unsigned integer | `unsigned char`, `unsigned short int`, `unsigned int`, `unsigned long int`, `unsigned long long int` |
| Real floating | `float`, `double`, `long double` |
| No-value type | `void` |

C23 还定义了 bit-precise integer types（`_BitInt`）以及其他浮点类型类别；本条目不展开这些类型的专门规则。

---

## 2. Core Rules

---

### 2.1 Type

Type 决定 object 中二进制表示的解释方式，并约束 expression 可以表示的值及可进行的操作。

不同 type names 不应仅按“占多少字节”理解。类型还决定值域、表示、转换和运算语义。

---

### 2.2 Boolean Type

C23 的 Boolean type 使用关键字 `bool`，其值为 `true` 或 `false`。

`_Bool` 自 C99 起存在，并在 C23 中成为 `bool` 的 deprecated alternative spelling。C23 中 `bool`、`true` 和 `false` 已是语言关键字，不再依赖 `<stdbool.h>` 提供宏定义。

```c
bool ready = true;
```

Boolean conversion 的完整规则属于 expression/conversion 语义，本条目不展开。

---

### 2.3 Character Types

`char`、`signed char` 和 `unsigned char` 是三个不同的类型。

Plain `char` 的表示和行为与 `signed char` 或 `unsigned char` 中的一种对应，具体选择由 implementation 决定；因此不能假定 `char` 一定有符号或一定无符号。

`sizeof(char) == 1`。C 中一个 byte 由 `CHAR_BIT` 个 bits 组成，标准保证 `CHAR_BIT >= 8`，因此不能把 C 的一个 byte 无条件等同于 8 bits。

`unsigned char` 还具有访问 object representation 的特殊语言地位；完整 object representation 规则属于 [07-objects-and-data-layout](../../07-objects-and-data-layout/)。

---

### 2.4 Standard Integer Types

标准 signed integer types 按 rank 递增为：

`signed char`, `short int`, `int`, `long int`, `long long int`。

每个标准 signed integer type 都有对应的 unsigned type。

常用简写是同一类型的不同 type-specifier 写法，例如 `short` 等价于 `short int`，`unsigned` 等价于 `unsigned int`。

标准只保证最低宽度：

| Type family | Minimum width |
| --- | ---: |
| `char`, `signed char`, `unsigned char` | 8 bits |
| `short`, `unsigned short` | 16 bits |
| `int`, `unsigned int` | 16 bits |
| `long`, `unsigned long` | 32 bits |
| `long long`, `unsigned long long` | 64 bits |

同时保证：

`1 == sizeof(char) <= sizeof(short) <= sizeof(int) <= sizeof(long) <= sizeof(long long)`

因此，`int`、`long` 等名称不能直接推出固定 bit width。

当 bit width、signedness、integer conversions 或 overflow 影响程序正确性时，见 [04-integers-and-bits](../../04-integers-and-bits/)。

---

### 2.5 Signed and Unsigned Integer Types

Signed 和对应 unsigned integer type 是不同类型，但具有相同的 storage requirement。

Unsigned integer type 的值域从 `0` 开始，并按模 `2^N` 的规则表示，其中 `N` 是 value bits 的数量。

Signed integer 的表示、integer promotions、usual arithmetic conversions、overflow 和 mixed signed/unsigned arithmetic 属于 [04-integers-and-bits](../../04-integers-and-bits/)，本条目只保留类型分类。

---

### 2.6 Real Floating Types

标准 real floating types 是：

- `float`
- `double`
- `long double`

它们按 floating-point conversion rank 递增，但 implementation 可以让相邻类型具有相同的表示。

不能仅凭类型名称假定具体 IEEE 754 format、bit width 或硬件执行成本。实际范围和精度应以 implementation 及 `<float.h>` 提供的信息为准。

```c
float temperature;
double calculation;
```

---

### 2.7 `void`

`void` 是 incomplete type，并且不能被 completed。

它表示没有可用的 value，因此不能定义 type 为 `void` 的 object。

`void` 可用于 function return type、无参数 function prototype，以及构成 pointer-to-`void` 类型。Function 和 pointer 的完整语义分别见 [08-functions-and-api](../../08-functions-and-api/) 与 [06-pointers-and-memory](../../06-pointers-and-memory/)。

---

## 3. Related Knowledge

---

- [001-variables-declarations-and-definitions](../001-variables-declarations-and-definitions/) — type 在 declaration 中的作用
- [003-initialization](../003-initialization/) — 不同类型 object 的 initialization
- [004-typedef-and-enum](../004-typedef-and-enum/) — type aliases 与 enumerated types
- [005-const-and-type-qualifiers](../005-const-and-type-qualifiers/) — qualified types
- [04-integers-and-bits](../../04-integers-and-bits/) — integer widths, conversions, overflow, and bit operations
- [06-pointers-and-memory](../../06-pointers-and-memory/) — pointer types and `void *`
- [07-objects-and-data-layout](../../07-objects-and-data-layout/) — object representation and byte-level access
- [08-functions-and-api](../../08-functions-and-api/) — function types and `void` return type

---

## 4. References

---

1. **ISO/IEC 9899:2024 (C23), 6.2.5 Types** — C type classification and properties.
2. **ISO/IEC 9899:2024 (C23), 6.2.6 Representations of types** — integer and object representations.
3. **ISO/IEC 9899:2024 (C23), 6.7.2 Type specifiers** — type specifier rules.
4. **cppreference — Type**  
   https://en.cppreference.com/w/c/language/type
5. **cppreference — Arithmetic types**  
   https://en.cppreference.com/w/c/language/arithmetic_types
6. **cppreference — `bool` keyword**  
   https://en.cppreference.com/w/c/keyword/bool
7. **cppreference — `_Bool` keyword**  
   https://en.cppreference.com/w/c/keyword/_Bool
8. **cppreference — `<limits.h>`**  
   https://en.cppreference.com/w/c/types/limits
9. **cppreference — `<float.h>`**  
   https://en.cppreference.com/w/c/types/limits
