# Variables, Declarations, and Definitions

## Definition

| Concept | Meaning |
| --- | --- |
| identifier | 源码中的名字，例如 `count` |
| object | 执行环境中的数据存储实体，其内容可以表示一个值 |
| variable | 工程中通常指通过 identifier 访问的 object |
| declaration | 对本条目讨论的普通声明，用于声明 identifier 并描述其实体的类型和其他属性 |
| declarator | declaration 中包含 identifier、并参与构成完整类型的部分 |
| definition | declaration 的一种；它实际定义相应实体 |
| initializer | declaration 中用于为 object 提供初始值的部分 |

核心关系：

```text
identifier ≠ object

definition ⊂ declaration

definition ≠ initialization
```

## Identifier, Object, and Variable

```c
int count = 10;
```

其中：

```text
count
    ↓
identifier
    ↓
命名一个 int object
    ↓
object 当前保存 10
```

日常工程中可以直接称 `count` 为一个 `int` variable。

Object 不一定有 identifier：

```c
(int){42}
```

这里存在一个值为 `42` 的 `int` object，但没有对应的 identifier。

## Declaration and Declarator

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

```text
int    → type specifier
count  → declarator / identifier
10     → initializer
```

Declarator 不只是 identifier 的另一种叫法。它还可以包含参与构成完整类型的语法。

最关键的例子：

```c
int *p, value;
```

应拆成：

```text
int    → 共同的 type specifier
*p     → declarator 1
value  → declarator 2
```

因此：

```text
p     → pointer to int
value → int
```

`*` 属于 declarator `*p`，不能把 `int *` 当成后续所有 declarator 共享的完整类型。

工程代码中，如果 declarator 的形状不同，优先拆开：

```c
int *p;
int value;
```

Pointer declarator 的完整语义见 [06-pointers-and-memory](../../06-pointers-and-memory/)。

使用 identifier 前应有正确 declaration；见 SEI CERT C DCL31-C。

## Definition

Definition 是 declaration 的一种：

```text
Every definition is a declaration.
Not every declaration is a definition.
```

常见形式：

```c
void foo(void)
{
    int local_count;          // block-scope object definition
}

extern int system_tick;       // file-scope declaration, not an object definition
int system_tick = 0;          // file-scope object definition
int global_count;             // file-scope tentative definition
```

`int global_count;` 在 file scope 是 **tentative definition**。完整规则见 [008-linkage-static-and-extern](../008-linkage-static-and-extern/)。

Definition 不表示 object 一定经过显式初始化：

```c
void foo(void)
{
    int a;
    int b = 0;
}
```

`a` 和 `b` 都被定义，但只有 `b` 有显式 initializer。初值规则见 [003-initialization](../003-initialization/)。

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

完整初始化规则见 [003-initialization](../003-initialization/)。

## Compatible Declarations

同一 function 或 object 的多次 declaration 必须具有 compatible types。

下面两条如果声明同一 object，则类型不兼容：

```c
extern int sensor_count;
extern short sensor_count;
```

见 SEI CERT C DCL40-C。

## Related Knowledge

- [002-fundamental-types](../002-fundamental-types/) — fundamental types
- [003-initialization](../003-initialization/) — initialization
- [006-scope-and-name-visibility](../006-scope-and-name-visibility/) — scope and visibility
- [007-storage-duration-and-lifetime](../007-storage-duration-and-lifetime/) — storage duration and lifetime
- [008-linkage-static-and-extern](../008-linkage-static-and-extern/) — linkage, `static`, and `extern`
- [01-preprocessor-and-build](../../01-preprocessor-and-build/) — headers, translation units, and linking
- [06-pointers-and-memory](../../06-pointers-and-memory/) — pointer declarators and pointer semantics
- [08-functions-and-api](../../08-functions-and-api/) — function declarations and interfaces

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
7. **SEI CERT C — DCL40-C: Do not create incompatible declarations of the same function or object**  
   https://cmu-sei.github.io/secure-coding-standards/sei-cert-c-coding-standard/rules/declarations-and-initialization-dcl/dcl40-c/
