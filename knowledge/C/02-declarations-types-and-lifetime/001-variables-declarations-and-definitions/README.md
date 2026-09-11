# Variables, Declarations, and Definitions

## 1. Why This Matters

几乎所有 C 程序都从“声明一个名字”开始。

```c
int count;
```

这一行看起来很简单，但它已经涉及几个贯穿整个 C 语言的核心概念：

- identifier（标识符）
- object（对象）
- variable（变量，常用工程术语）
- type（类型）
- declaration（声明）
- definition（定义）
- initializer（初始化器）

如果这些概念一开始混在一起，后面学习 `extern`、`static`、指针、头文件、链接和多文件工程时会反复困惑。

本章目标不是背语法，而是建立一个稳定的心智模型：

```text
源码中的名字
    ↓
declaration 告诉编译器“这个名字表示什么”
    ↓
definition 进一步让实体真正被定义
    ↓
如果定义的是 object，程序需要相应的存储
    ↓
initializer 可以给 object 一个初始值
```

---

## 2. Core Concepts

### 2.1 Identifier — 标识符

**Identifier** 是程序中用于给实体命名的名字。

例如：

```c
int count;
int system_tick;
int uart_rx_count;
```

这里：

```text
count
system_tick
uart_rx_count
```

都是 identifiers。

标识符可以用来命名很多东西，例如：

- objects
- functions
- typedef names
- enumeration constants
- structure / union / enum tags
- labels

本章主要关注“用于命名 object 的 identifier”。

### 2.2 Identifier 的基本命名规则

常见形式：

```text
字母、数字、下划线 _
```

但不能以数字开头。

合法示例：

```c
int count;
int uart_rx_count;
int value2;
```

非法示例：

```c
int 2value;
```

C 区分大小写：

```c
int count;
int Count;
```

这两个 identifier 不同。

关键字不能作为 identifier：

```c
// invalid
int while;
int static;
```

### 2.3 不要随意使用保留标识符

标准库和实现会保留一部分 identifier。

实际嵌入式工程中，一个简单而安全的习惯是：

> 不要给自己的变量、函数或宏起以下划线开头的名字。

例如应避免：

```c
int _count;
int __driver_state;
```

这不仅是风格问题。SEI CERT C 的 DCL37-C 明确要求不要声明或定义 reserved identifier。

---

## 3. Object and Variable

### 3.1 Object — C 标准里的核心概念

在 C 的语言模型里，**object** 可以理解为：

> 程序执行期间用于保存一个值的一块数据存储。

例如：

```c
int count;
```

如果这里定义了一个 `int` object，那么程序需要有一块适合保存这个 `int` 值的存储。

### 3.2 Variable — 常用工程术语

“Variable” 是我们日常写程序时更常说的词：

```c
int temperature;
```

通常会说：

> `temperature` 是一个变量。

更精确地理解，可以拆成：

```text
temperature
    ↓
identifier

它命名了
    ↓
一个 int object
```

所以在后续学习里，看到“variable”时可以先把它理解为：

> 一个通过 identifier 访问的 object。

C 标准讨论底层语义时经常使用 object 这个词，因此这个概念必须尽早熟悉。

---

## 4. Type — 类型

看：

```c
int count;
```

这里 `int` 指定了类型。

类型决定很多事情，例如：

- object 能表示什么种类的值
- 可以对它执行哪些操作
- 编译器如何解释相关表达式
- 通常需要多少存储空间（具体大小由实现决定）

先建立最简单的模型：

```text
int count;
│   └── identifier
└────── type
```

更完整的 fundamental types 会在：

```text
002-fundamental-types/
```

学习。

整数宽度、signed/unsigned 运算、promotion 等更深入问题属于：

```text
04-integers-and-bits/
```

---

## 5. Declaration — 声明

cppreference 对 C declaration 的概括非常准确：

> declaration 会把 identifier 引入程序，并说明它的含义和属性。

可以把 declaration 理解成编译器需要的一份“说明”。

例如：

```c
int count;
```

告诉编译器：

```text
有一个名字叫 count
它与 int 类型相关
```

另一个例子：

```c
extern int system_tick;
```

这也是 declaration。

它告诉编译器 `system_tick` 是一个 `int` object 的名字，但这里是否真正定义该 object，要继续看 declaration 的形式。

### 5.1 为什么必须先声明？

现代 C 不应依赖“编译器猜这个名字是什么意思”。

SEI CERT C DCL31-C 的核心原则是：

> Declare identifiers before using them.

例如函数调用应该先有可见声明：

```c
int add(int a, int b);

int main(void)
{
    int result = add(1, 2);
    return 0;
}
```

而不是先调用一个编译器从未见过的函数。

对于嵌入式开发，这一点尤其重要，因为错误的函数声明可能导致：

- 参数解释错误
- 返回值解释错误
- 指针宽度问题
- ABI / calling convention 层面的严重故障

---

## 6. Anatomy of a Simple Declaration

先看最常见形式：

```c
int count = 10;
```

可以拆成：

```text
int          count        = 10
│            │              │
│            │              └── initializer
│            └───────────────── declarator / identifier
└────────────────────────────── type specifier
```

对于入门阶段，可以先用这个模型：

```text
type + name + optional initializer
```

例如：

```c
int count;
unsigned int error_count;
char status;
int retry_count = 3;
```

更严格地说，C declaration 由 declaration specifiers、declarator，以及可选 initializer 等部分组成；复杂指针、数组和函数 declarator 会在后续模块继续学习。

### 6.1 一条声明可以声明多个名字

C 允许：

```c
int a, b, c;
```

也允许：

```c
int a = 0, b = 1;
```

但学习和工程代码中通常更推荐：

```c
int a = 0;
int b = 1;
```

原因不是语言要求，而是单独声明更容易：

- 阅读
- 修改
- code review
- 看到每个 object 的初始化状态
- 避免复杂 declarator 混在一起

---

## 7. Definition — 定义

**Definition 是一种 declaration，但它进一步完成了实体的定义。**

最重要的关系：

```text
Every definition is a declaration.
Not every declaration is a definition.
```

也就是：

> 所有 definition 都属于 declaration，但 declaration 不一定是 definition。

### 7.1 Object declaration vs. definition

例如：

```c
extern int system_tick;
```

通常只是 declaration。

它告诉编译器：

> 某处存在一个名为 `system_tick` 的 `int` object。

而：

```c
int system_tick = 0;
```

是 definition。

它真正定义了这个 object。

简单对比：

```text
extern int system_tick;
        ↓
declaration

int system_tick = 0;
        ↓
definition + declaration
```

### 7.2 `extern` 并不永远表示“不是定义”

注意：

```c
extern int system_tick = 0;
```

这个 declaration 带有 initializer，因此它是一个 definition。

所以不要形成错误口诀：

> “看到 extern 就一定不是 definition。”

真正需要判断的是 declaration 的完整形式和上下文。

`extern`、linkage 和多文件规则会在：

```text
008-linkage-static-and-extern/
```

完整学习。

### 7.3 `int count;` 的一个重要细节

在函数内部：

```c
void foo(void)
{
    int count;
}
```

`count` 的 declaration 同时定义了一个 object。

但在 file scope：

```c
int count;
```

它属于 C 中的 **tentative definition**。

第一次学习不需要深入 tentative definition，只要先记住：

> `int count;` 通常不应简单理解成“只有声明，没有定义”。

这个细节会在 linkage / multi-file 相关内容中继续展开。

---

## 8. Function Declaration and Definition

declaration / definition 不只适用于变量。

例如：

```c
int add(int a, int b);
```

这是 function declaration，也通常称为 prototype。

它告诉编译器：

```text
函数名：add
参数：两个 int
返回值：int
```

而：

```c
int add(int a, int b)
{
    return a + b;
}
```

是 function definition。

所以同样有：

```text
int add(int a, int b);
        ↓
declaration

int add(int a, int b)
{
    return a + b;
}
        ↓
definition
```

函数的深入内容属于：

```text
08-functions-and-api/
```

---

## 9. Initializer — 初始化器

看：

```c
int retry_count = 3;
```

其中：

```c
= 3
```

属于 initializer syntax。

它给 object 提供初始值。

### 9.1 Initialization 不等于 Assignment

这两个语句看起来都有 `=`：

```c
int count = 10;
count = 20;
```

但概念不同。

第一行：

```c
int count = 10;
```

是在定义 object 时进行 **initialization**。

第二行：

```c
count = 20;
```

是 object 已经存在之后执行 **assignment**。

先记：

```text
definition time
    ↓
initialization

object already exists
    ↓
assignment
```

初始化规则将在：

```text
003-initialization/
```

完整学习。

---

## 10. Reading Declarations Step by Step

### Example 1

```c
int count;
```

先问：

1. type 是什么？  
   `int`

2. identifier 是什么？  
   `count`

3. 有没有 initializer？  
   没有。

4. 是 declaration 吗？  
   是。

5. 是 definition 吗？  
   对普通 block-scope object，是；file scope 下存在 tentative definition 的细节。

### Example 2

```c
unsigned int error_count = 0;
```

拆解：

```text
unsigned int
    ↓
type

error_count
    ↓
identifier

= 0
    ↓
initializer
```

这条 declaration 同时也是 object definition。

### Example 3

```c
extern int system_tick;
```

拆解：

```text
extern
    ↓
storage-class specifier

int
    ↓
type specifier

system_tick
    ↓
identifier
```

在典型用法中，它提供 declaration，但 object definition 位于其他地方。

---

## 11. Embedded C Example

一个常见的嵌入式项目可能有：

```c
/* system.h */

extern unsigned int system_tick;
```

以及：

```c
/* system.c */

unsigned int system_tick = 0;
```

先只关注 declaration / definition：

```text
system.h
    ↓
extern unsigned int system_tick;
    ↓
让其他 translation unit 知道这个名字和类型

system.c
    ↓
unsigned int system_tick = 0;
    ↓
真正定义 object
```

之后其他源文件可以：

```c
#include "system.h"
```

获得一致的 declaration。

为什么 header、translation unit、linker 能让这套模式工作，属于：

```text
01-preprocessor-and-build/
008-linkage-static-and-extern/
```

---

## 12. Common Mistakes

### 12.1 把 declaration 和 definition 当成同义词

错误心智模型：

```text
declaration = definition
```

正确关系：

```text
definition ⊂ declaration
```

也就是 definition 是 declaration 的一类。

---

### 12.2 认为 `extern` 永远不是 definition

```c
extern int value;
```

通常只是 declaration。

但：

```c
extern int value = 10;
```

是 definition。

---

### 12.3 在不同文件中给同一实体写不兼容的声明

例如：

```c
/* a.c */
extern int sensor_count;
```

而另一个文件：

```c
/* b.c */
short sensor_count;
```

这两个声明的类型不兼容。

SEI CERT C DCL40-C 明确要求：

> 同一 function 或 object 的多次 declaration 必须保持 compatible type。

这种问题在嵌入式系统中特别危险，因为结果可能不是简单的编译错误，而可能表现为：

- 读取错误宽度
- 错误解释内存
- memory overwrite
- hardware trap
- 仅在某些优化等级出现异常

正确做法之一是让共享 declaration 来自同一个 header。

---

### 12.4 使用保留标识符

避免：

```c
int _system_state;
int __uart_flag;
```

用户代码使用普通、清晰、领域相关的名字更安全：

```c
int system_state;
int uart_flag;
```

---

### 12.5 依赖“未声明也许编译器能猜”

不要写依赖旧式 C 隐式声明的代码。

现代 C 应明确提供 type 和 function declaration。

这也是 CERT DCL31-C 的核心要求。

---

## 13. Embedded Engineering Rules

### Rule 1 — Declare before use

在使用 identifier 前，让编译器看到正确 declaration。

特别是 function prototype。

这样编译器才能检查：

- argument count
- argument types
- return type
- type compatibility

---

### Rule 2 — Keep repeated declarations compatible

同一个外部 object 或 function 如果在多个 translation unit 中出现 declaration，它们必须保持兼容。

工程上最常见的办法是：

```text
public declaration
    ↓
one header

definition
    ↓
one source file
```

---

### Rule 3 — Prefer one clear definition

对于需要跨文件访问的普通全局 object，通常保持：

```text
一个 authoritative definition
+
需要的 declarations
```

这比在多个源文件中重复定义要可靠得多。

---

### Rule 4 — Avoid reserved names

不要用实现或标准保留的 identifier 命名自己的实体。

特别避免以下划线开头的自定义名字。

---

### Rule 5 — Initialize before use

完整初始化规则属于下一阶段的：

```text
003-initialization/
```

但从工程习惯开始就应该记住：

> object 在读取之前必须具有有效值。

BARR-C:2018 的变量规则也明确要求变量在使用前初始化。

---

## 14. Key Takeaways

### 核心模型

```text
identifier
    ↓
名字

object
    ↓
保存值的数据存储

type
    ↓
描述 object / entity 的类型属性

declaration
    ↓
向编译器说明一个 identifier 表示什么

definition
    ↓
一种更完整的 declaration，真正定义实体

initializer
    ↓
在初始化时提供初始值
```

### 必须记住的关系

```text
Every definition is a declaration.
Not every declaration is a definition.
```

### 看到一条简单 declaration 时，按顺序问

```text
1. type 是什么？
2. identifier 是什么？
3. declarator 表达什么？
4. 有没有 initializer？
5. 只是 declaration，还是同时也是 definition？
```

如果这五个问题能稳定回答，本章的核心目标就已经达到。

---

## 15. Related Knowledge

继续学习：

- [002-fundamental-types](../002-fundamental-types/) — C 有哪些基础类型，类型到底描述什么
- [003-initialization](../003-initialization/) — object 的初始值从哪里来
- [006-scope-and-name-visibility](../006-scope-and-name-visibility/) — identifier 在哪里可见
- [007-storage-duration-and-lifetime](../007-storage-duration-and-lifetime/) — object 存在多久
- [008-linkage-static-and-extern](../008-linkage-static-and-extern/) — 多个 translation unit 如何共享或隐藏名字
- [01-preprocessor-and-build](../../01-preprocessor-and-build/) — headers、translation units、compiler 和 linker
- [08-functions-and-api](../../08-functions-and-api/) — function declarations、prototypes 和接口

---

## 16. References

本章以 C 语言标准语义为基线，并参考以下成熟资料重新组织内容：

1. **ISO/IEC 9899:2024 (C23)** — C language standard. Relevant areas include identifiers, declarations, definitions, types, storage duration, and linkage.
2. **cppreference — C Declarations**  
   https://en.cppreference.com/c/language/declarations
3. **cppreference — C Initialization**  
   https://en.cppreference.com/c/language/initialization
4. **SEI CERT C — DCL31-C: Declare identifiers before using them**  
   https://cmu-sei.github.io/secure-coding-standards/sei-cert-c-coding-standard/rules/declarations-and-initialization-dcl/dcl31-c/
5. **SEI CERT C — DCL37-C: Do not declare or define a reserved identifier**  
   https://wiki.sei.cmu.edu/confluence/spaces/c/pages/87152308/
6. **SEI CERT C — DCL40-C: Do not create incompatible declarations of the same function or object**  
   https://wiki.sei.cmu.edu/confluence/spaces/c/pages/87151998/
7. **BARR-C:2018 Embedded C Coding Standard — Variable Rules / Initialization**  
   https://barrgroup.com/7-variable-rules

The wording in this note is primarily a learning-oriented synthesis rather than a reproduction of the source standards.
