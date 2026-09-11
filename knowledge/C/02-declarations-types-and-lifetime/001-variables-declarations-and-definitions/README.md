# Variables, Declarations, and Definitions

---

## 1. Overview

---

学习 C 的声明时，最容易混在一起的几个概念是：

- identifier：程序中的名称；
- object：实际保存数据的存储实体；
- declaration：告诉编译器某个名称代表什么；
- declarator：声明中描述名称及其类型形状的部分；
- definition：真正定义实体的 declaration；
- initializer：定义 object 时提供初始值的部分。

例如：

```c
int count = 10;
```

这一行同时涉及多个概念：

| 部分 | 含义 |
| --- | --- |
| `int` | type specifier |
| `count` | identifier |
| `count` | 这一声明中的 declarator |
| `= 10` | initializer |
| 整条语句 | declaration |
| 整条语句 | object definition |

程序运行时，真正保存整数值的是 `count` 所指示的 object；`count` 本身是这个 object 的名称。

---

## 2. Core Concepts

---

### 2.1 Identifier

Identifier 是程序中用来指示实体的名称。

例如：

```c
int count;
int temperature;
```

这里的 `count` 和 `temperature` 都是 identifiers。

Identifier 不只可以指示 object，还可以用于 function、typedef name 等其他语言实体。本章主要关注 identifier 与 object 的关系。

Identifier 本身不是数据存储。

在：

```c
int count = 10;
```

中：

- `count` 是 identifier；
- 它指示一个 `int` object；
- 值 `10` 存放在 object 中，而不是“存放在 identifier 中”。

---

### 2.2 Object and Variable

Object 是执行期间用于保存数据的存储区域，其内容可以表示一个值。

例如：

```c
int count = 10;
```

程序为这个 `int` object 提供存储，并在其中保存值 `10`。

日常编程中，像 `count` 这样通过名称访问的数据项通常称为 **variable**。

因此在普通代码阅读中，可以把：

```c
int count = 10;
```

理解为“定义一个名为 `count` 的整数变量”。

但在需要精确讨论语言规则时，要区分：

- `count`：identifier；
- 被 `count` 指示的存储实体：object。

Object 还具有 type、storage duration 和 lifetime。它们分别在本模块的其他章节中继续学习。

---

### 2.3 Declaration

Declaration 用来告诉编译器一个 identifier 的含义和属性。

最常见的 object declaration 会说明名称以及它的类型：

```c
int count;
double voltage;
unsigned int retry_count;
```

这些 declaration 分别告诉编译器：

- `count` 是 `int` 类型；
- `voltage` 是 `double` 类型；
- `retry_count` 是 `unsigned int` 类型。

Declaration 不一定同时提供初始值：

```c
int count;
```

也可以带 initializer：

```c
int count = 10;
```

一个 declaration 还可以声明多个名称：

```c
int x, y, z;
```

这里 `x`、`y`、`z` 都在同一个 declaration 中被声明。

---

### 2.4 Declarator

Declarator 是 declaration 中包含 identifier，并进一步描述其类型形式的部分。

最简单的 declarator 只有名称：

```c
int count;
```

这里：

- `int` 是 type specifier；
- `count` 是 declarator。

Declarator 也可以包含额外的类型结构：

```c
int *pointer;
int values[4];
```

这里：

- `*pointer` 是 declarator；
- `values[4]` 是 declarator。

它们与前面的 `int` 组合后，分别形成 pointer type 和 array type。

同一个 declaration 中的多个 declarators 可以得到不同的完整类型：

```c
int *pointer, value;
```

这里：

- `pointer` 是 pointer to `int`；
- `value` 是 `int`。

Pointer、array 和 function declarator 的完整规则分别属于对应主题，本章只需要先理解 declarator 在 declaration 中的作用。

---

### 2.5 Definition

Definition 是 declaration 的一种。

对于 object，definition 会使该 object 的存储被定义出来。

例如：

```c
int count = 10;
```

这既是 declaration，也是 `count` 对应 object 的 definition。

Declaration 则不一定是 definition。例如在 file scope：

```c
extern int system_tick;
```

这条语句声明了 `system_tick`，但没有在这里定义对应 object。

随后可以在某个 translation unit 中提供 definition：

```c
int system_tick = 0;
```

因此：

- 每个 definition 都是 declaration；
- declaration 不一定是 definition。

File-scope tentative definition、`extern` 和 linkage 的完整规则见 [008-linkage-static-and-extern](../008-linkage-static-and-extern/)。

Function 也有 declaration 和 definition；完整规则见 [08-functions-and-api](../../08-functions-and-api/)。

---

### 2.6 Initializer and Initialization

Initializer 是在 object 被定义时提供初始值的语法部分。

例如：

```c
int count = 10;
```

这里：

- `count` 是 declarator；
- `= 10` 是 initializer；
- object 获得初始值 `10` 的过程是 initialization。

Initializer 是 declaration / definition 的一部分。

它与 assignment 不同：

```c
int count = 10;  /* initialization */

count = 20;      /* assignment */
```

第一条语句创建并初始化 object。

第二条语句是在 object 已经存在之后修改它保存的值。

更完整的 initialization 规则见 [003-initialization](../003-initialization/)。

---

### 2.7 Putting the Concepts Together

再看一个完整例子：

```c
unsigned int retry_count = 3;
```

可以这样理解：

| 概念 | 对应内容 |
| --- | --- |
| type | `unsigned int` |
| identifier | `retry_count` |
| declarator | `retry_count` |
| initializer | `= 3` |
| declaration | 整条语句 |
| definition | 整条语句 |
| object | 运行时保存 `retry_count` 值的存储实体 |
| initial value | `3` |

这些概念描述的是同一条代码的不同层面，并不是互相替代的名称。

---

## 3. Related Knowledge

---

- [002-fundamental-types](../002-fundamental-types/) — fundamental types
- [003-initialization](../003-initialization/) — initialization
- [006-scope-and-name-visibility](../006-scope-and-name-visibility/) — scope and name visibility
- [007-storage-duration-and-lifetime](../007-storage-duration-and-lifetime/) — storage duration and lifetime
- [008-linkage-static-and-extern](../008-linkage-static-and-extern/) — linkage, `static`, `extern`, and tentative definitions
- [06-pointers-and-memory](../../06-pointers-and-memory/) — pointer declarators and pointer semantics
- [08-functions-and-api](../../08-functions-and-api/) — function declarations and definitions

---

## 4. References

---

1. **ISO/IEC 9899:2024 (C23), 6.2.1 Scopes of identifiers** — identifier scope.
2. **ISO/IEC 9899:2024 (C23), 6.2.4 Storage durations of objects** — object storage and lifetime terminology.
3. **ISO/IEC 9899:2024 (C23), 6.7 Declarations** — declarations, definitions, declaration specifiers, and declarators.
4. **WG14 N3220 — ISO/IEC 9899:2024 working draft**  
   https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf
5. **cppreference — Declarations**  
   https://en.cppreference.com/c/language/declarations
6. **cppreference — External and tentative definitions**  
   https://en.cppreference.com/c/language/extern
7. **cppreference — Initialization**  
   https://en.cppreference.com/c/language/initialization
