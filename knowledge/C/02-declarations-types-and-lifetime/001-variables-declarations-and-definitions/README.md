# Variables, Declarations, and Definitions

## Definition

C 中需要先区分几个紧密相关但不同的概念：

| Concept | Meaning |
| --- | --- |
| identifier | 源码中的名字，例如 `count` |
| object | 执行环境中的数据存储实体，其内容可以表示一个值 |
| variable | 工程中通常指通过 identifier 访问的 object |
| declaration | 描述 identifier 及其实体属性的声明 |
| declarator | declaration 中包含 identifier、并参与构成完整类型的部分 |
| definition | 一种 declaration；它实际定义相应实体 |
| initializer | 在定义 object 时提供初始值的语法部分 |

最重要的关系：

```text
identifier ≠ object

definition ⊂ declaration

definition ≠ initialization
```

---

## Identifier, Object, and Variable

### Identifier

Identifier 是源码中的名字：

```c
int count = 10;
```

这里：

```text
count → identifier
```

C 区分大小写，关键字不能作为 identifier。

工程上建议用户自定义名称不要以下划线开头，以避免与实现保留名称发生冲突。C 对 reserved identifiers 有更精确的规则，SEI CERT C DCL37-C 也要求避免声明或定义保留标识符。

### Object

Object 是实际的数据存储实体。

```c
int count = 10;
```

可以拆成：

```text
count
    ↓
identifier

它命名
    ↓
一个 int object

该 object 当前保存
    ↓
10
```

Object 不一定有 identifier。例如：

```c
(int){42}
```

这里存在一个值为 `42` 的 `int` object，但没有类似 `count` 的 identifier。

因此：

```text
identifier = 名字
object     = 实体
```

### Variable

在普通工程语境中：

```c
int temperature = 25;
```

可以直接称为：

```text
int 变量 temperature
```

讨论标准语义时，则可以更精确地写成：

```text
temperature
    ↓
identifier

它命名
    ↓
一个 int object
```

---

## Declaration

Declaration 描述一个 identifier 所表示实体的类型和其他属性。

最简单的例子：

```c
int count;
```

这里声明了 identifier `count`。

另一个常见例子：

```c
extern int system_tick;
```

它声明了 `system_tick`，但在典型 file-scope 用法中，这条 declaration 本身不是 object definition。

工程上应保证 identifier 在使用前已有正确 declaration。SEI CERT C DCL31-C 对此有明确要求。

---

## Declaration Specifiers and Declarator

简单 declaration 可以先按下面的结构理解：

```text
declaration specifiers
        +
declarator
        +
optional initializer
```

例如：

```c
int count = 10;
```

拆开：

```text
int
    ↓
type specifier

count
    ↓
declarator
    ↓
identifier 也是 count

10
    ↓
initializer
```

### Declarator

Declarator 不只是 identifier 的另一种叫法。

比较：

| Declaration | Type specifier | Declarator | Identifier | Complete type |
| --- | --- | --- | --- | --- |
| `int value;` | `int` | `value` | `value` | `int` |
| `int *p;` | `int` | `*p` | `p` | pointer to `int` |
| `int buffer[16];` | `int` | `buffer[16]` | `buffer` | array of 16 `int` |
| `int read_value(void);` | `int` | `read_value(void)` | `read_value` | function returning `int` |

因此：

```text
identifier
```

只是 declarator 中的名字；`*`、`[]`、`()` 等 declarator 语法也参与决定完整类型。

### 多个 declarators

```c
int *p, value;
```

应拆成：

```text
int
    ↓
共同的 type specifier

*p
    ↓
declarator 1

value
    ↓
declarator 2
```

所以：

```text
p     → pointer to int
value → int
```

这说明 `*` 属于 declarator `*p`，不能简单把 `int *` 当成所有后续名字共享的完整类型。

工程代码中，如果多个 declarator 的形状不同，优先拆开：

```c
int *p;
int value;
```

完整 pointer、array 和 function declarator 语义分别属于后续相关章节。

---

## Definition

Definition 是 declaration 的一种：

```text
Every definition is a declaration.
Not every declaration is a definition.
```

### Object definition

在 block scope：

```c
void foo(void)
{
    int count;
}
```

`int count;` 定义了一个 object。

但：

```text
definition ≠ initialization
```

这里没有显式 initializer；它的初值规则属于 [003-initialization](../003-initialization/)。

### Declaration without object definition

在典型 file-scope 用法中：

```c
extern int system_tick;
```

是 declaration，不是 object definition。

对应 definition 可以是：

```c
int system_tick = 0;
```

`extern`、linkage 和多文件规则属于 [008-linkage-static-and-extern](../008-linkage-static-and-extern/)。

### File-scope tentative definition

在 file scope：

```c
int count;
```

属于 **tentative definition**，不能简单按普通 block-scope definition 理解。

本条目只保留这个区别；完整规则见 [008-linkage-static-and-extern](../008-linkage-static-and-extern/)。

---

## Initializer

```c
int retry_count = 3;
```

其中 `3` 是 initializer。

Initialization 与 assignment 不同：

```c
int count = 10;  // initialization
count = 20;      // assignment
```

初始化的完整规则，包括不同 storage duration 下未显式初始化 object 的初值，属于 [003-initialization](../003-initialization/)。

---

## Compatible Declarations

同一个 function 或 object 如果出现多次 declaration，这些 declarations 必须具有 compatible types。

例如下面两条如果意图声明同一个 object，则类型不兼容：

```c
extern int sensor_count;
extern short sensor_count;
```

SEI CERT C DCL40-C 对此有明确要求。

工程上，共享 declaration 应集中在一个 authoritative header 中，避免不同源文件手写出彼此不一致的声明。

Header、translation unit 和 linker 的关系见 [01-preprocessor-and-build](../../01-preprocessor-and-build/)。

---

## Related Knowledge

- [002-fundamental-types](../002-fundamental-types/) — fundamental types
- [003-initialization](../003-initialization/) — initialization rules
- [006-scope-and-name-visibility](../006-scope-and-name-visibility/) — scope and identifier visibility
- [007-storage-duration-and-lifetime](../007-storage-duration-and-lifetime/) — storage duration and object lifetime
- [008-linkage-static-and-extern](../008-linkage-static-and-extern/) — linkage, `static`, and `extern`
- [01-preprocessor-and-build](../../01-preprocessor-and-build/) — headers, translation units, and linking
- [06-pointers-and-memory](../../06-pointers-and-memory/) — pointer declarators and pointer semantics
- [08-functions-and-api](../../08-functions-and-api/) — function declarations and interfaces

---

## References

1. **ISO/IEC 9899:2024 (C23)** — C language standard.
2. **cppreference — Declarations**  
   https://en.cppreference.com/c/language/declarations
3. **cppreference — External and tentative definitions**  
   https://en.cppreference.com/c/language/extern
4. **cppreference — Objects and alignment**  
   https://en.cppreference.com/c/language/object
5. **cppreference — Initialization**  
   https://en.cppreference.com/c/language/initialization
6. **SEI CERT C — DCL31-C: Declare identifiers before using them**  
   https://cmu-sei.github.io/secure-coding-standards/sei-cert-c-coding-standard/rules/declarations-and-initialization-dcl/dcl31-c/
7. **SEI CERT C — DCL37-C: Do not declare or define a reserved identifier**  
   https://cmu-sei.github.io/secure-coding-standards/sei-cert-c-coding-standard/rules/declarations-and-initialization-dcl/dcl37-c/
8. **SEI CERT C — DCL40-C: Do not create incompatible declarations of the same function or object**  
   https://cmu-sei.github.io/secure-coding-standards/sei-cert-c-coding-standard/rules/declarations-and-initialization-dcl/dcl40-c/
