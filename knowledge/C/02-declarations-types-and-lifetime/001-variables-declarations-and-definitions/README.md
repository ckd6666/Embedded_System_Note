# Variables, Declarations, and Definitions

## 1. Why This Matters

几乎所有 C 程序都会不断回答同一组问题：

- 这个名字是什么？
- 它表示什么实体？
- 它是什么类型？
- 这里只是在声明它，还是已经定义了它？
- 如果它是一个 object，它是否已经有有效初值？

例如：

```c
int count;
```

这一行虽然简单，却已经涉及贯穿 C 语言的大量核心概念：

- identifier（标识符）
- object（对象）
- variable（变量，常用工程术语）
- type（类型）
- declaration（声明）
- declarator（声明符）
- definition（定义）
- initializer（初始化器）

这些概念如果一开始混在一起，后面学习 `extern`、`static`、指针、数组、头文件、链接和多文件工程时会反复产生混乱。

本章先建立下面这条主线：

```text
identifier
    ↓
declaration 描述这个名字及其属性
    ↓
declarator 说明“这个名字如何具有该类型”
    ↓
definition 真正定义相应实体
    ↓
如果定义的是 object，则涉及实际存储
    ↓
initializer 可以为 object 提供初始值
```

> 本章只处理基础声明模型。初始化规则、作用域、存储期、链接等细节会在本模块后续章节展开。

---

## 2. Identifier — 标识符

### 2.1 Identifier 是什么

**Identifier** 是源码中用于命名实体的名字。

例如：

```c
int count;
int system_tick;
int uart_rx_count;
```

其中：

```text
count
system_tick
uart_rx_count
```

都是 identifiers。

C 中 identifier 可以用于命名多种实体，例如：

- objects
- functions
- typedef names
- enumeration constants
- structure / union / enum tags
- labels

不同类别的 identifier 还涉及 C 的不同 name spaces；这些细节暂时不展开。

本章主要关注：

> **用于命名 object 的 ordinary identifier。**

---

### 2.2 常用 ASCII 命名规则

对嵌入式工程最常见的 ASCII identifier，可以先记：

- 可以使用英文字母、数字和下划线 `_`
- 不能以数字开头
- C 区分大小写
- C 关键字不能作为 identifier

例如：

```c
int count;
int uart_rx_count;
int value2;
```

是合法的。

而：

```c
int 2value;   // invalid
int while;    // invalid: keyword
```

是不合法的。

下面两个名字不同：

```c
int count;
int Count;
```

> C 标准还允许比 ASCII 更广的 identifier 字符集合。对于嵌入式源码，本仓库优先采用简单、可移植的 ASCII 命名。

---

### 2.3 Reserved identifiers — 保留标识符

这里需要区分：

1. **C 标准真正保留的名字**
2. **工程上主动采用的更保守命名规则**

C23 中最重要的保留规则包括：

- 以双下划线 `__` 开头的 identifier 保留
- 以下划线加大写字母开头，例如 `_UART`，保留
- 任何以下划线开头的 identifier，在 file scope 的 ordinary/tag name space 中保留
- 标准库还会额外保留一些名字

因此用户代码不要写：

```c
int __driver_state;
int _UART_State;
```

而对于：

```c
void foo(void)
{
    int _count;
}
```

不能简单说“C 标准在所有上下文都禁止它”。

不过，为了避免与编译器、标准库和平台实现发生冲突，本仓库采用更保守的工程规则：

> **用户自定义 identifier 不以下划线开头。**

优先：

```c
int driver_state;
int uart_state;
int count;
```

SEI CERT C 的 DCL37-C 也要求避免声明或定义 reserved identifier。

---

## 3. Object and Variable

### 3.1 Object — C 语言模型中的核心概念

C 中的 **object** 是执行环境中的一块数据存储区域，其内容可以表示一个值。

例如：

```c
int count;
```

如果这条声明在当前上下文中定义了一个 `int` object，那么程序就有一个具有 `int` 类型属性的 object。

一个 object 后续还会涉及：

- size
- alignment
- storage duration
- lifetime
- value
- effective type 等属性

这些内容会在后续模块展开。

---

### 3.2 Object 不一定有名字

最容易形成的误解是：

```text
object = variable name
```

其实这两个概念不在同一个层次：

```text
identifier
    ↓
源码里的名字

object
    ↓
程序运行时实际存在的数据存储实体
```

最普通的情况是：

```c
int count = 10;
```

这里可以拆成：

```text
count
    ↓
identifier

它命名
    ↓
一个 int object

这个 object 当前保存
    ↓
10
```

所以这里既有“名字”，也有“object”。

但 C 里也可以出现 **没有 identifier 的 object**。

#### 最小例子 1：compound literal

```c
(int){42}
```

这里会产生一个 `int` object，它的值是 `42`。

但是源码里没有类似：

```c
int count = 42;
```

这样的 identifier。

可以理解为：

```text
(int){42}
    ↓
一个 int object

value = 42

但没有像 count 这样的名字
```

#### 最小例子 2：string literal

```c
"hello"
```

这个 string literal 对应一个字符数组 object，其中包含：

```text
'h' 'e' 'l' 'l' 'o' '\0'
```

但这个数组 object 本身没有一个你在源码中声明的 identifier。

也就是说：

```text
有 object
≠
一定有 identifier
```

#### 最小例子 3：以后会遇到的动态分配对象

先不用理解 `malloc()` 的细节，只看关系：

```c
int *p = malloc(sizeof(int));
```

这里有两个不同层次的东西：

```text
p
    ↓
identifier

p 自己命名
    ↓
一个 pointer object

malloc 得到的那块存储
    ↓
另一个 object / allocated region
    ↓
没有自己的 identifier
```

以后通常通过：

```c
*p
```

间接访问那块存储。

所以本节真正想让你记住的是：

> **identifier 是名字，object 是实体；object 可以有名字，也可以没有名字。**

当前阶段不需要深入 unnamed object，只要避免把“object”和“变量名”当成同一个概念即可。

---

### 3.3 Variable — 本仓库中的使用方式

工程交流里，我们通常不会每次都说：

> identifier `temperature` 命名了一个 `int` object。

而会直接说：

> 定义了一个变量 `temperature`。

例如：

```c
int temperature = 25;
```

可以从两个层次理解。

#### 日常工程说法

```text
temperature 是一个 int 变量
当前值是 25
```

这样说完全没有问题，也是本仓库大多数普通讲解会采用的表达。

#### 严格语义拆解

```text
temperature
    ↓
identifier

int
    ↓
type

temperature 命名
    ↓
一个 int object

这个 object 当前保存
    ↓
25
```

因此可以暂时把 variable 理解为：

> **日常编程中，对“一个通过 identifier 访问的 object”的常用称呼。**

不过不要反过来得到：

```text
所有 object 都一定是有名字的 variable
```

因为上一节已经看到：

```c
(int){42}
"hello"
```

这类 object 并没有用户声明的 identifier。

### 3.4 当前阶段怎么记最合适

先记下面三个层次就够了：

| 概念 | 当前阶段的理解 |
| --- | --- |
| identifier | 源码里的名字，例如 `count` |
| object | 程序执行时真正保存数据的实体 |
| variable | 工程上通常指“有名字、可以访问的 object” |

例如：

```c
int count = 10;
```

可以读成：

```text
count
    ↓
identifier

int
    ↓
type

保存 10 的实体
    ↓
object

日常统称
    ↓
variable count
```

最重要的是不要混淆：

```text
identifier ≠ object
```

而在普通代码交流中，说：

```text
count 是一个变量
```

完全可以。

---

## 4. Type — 类型

看：

```c
int count;
```

这里 `int` 是 type specifier。

类型决定或参与决定：

- object 能表示哪类值
- 可以执行哪些操作
- 表达式如何解释
- object 需要满足怎样的存储和对齐要求

先建立简单模型：

```text
int count;
│   └── identifier
└────── type specifier
```

但严格来说，一个 identifier 的完整类型并不总由前面的 type specifier 单独决定。

例如以后会看到：

```c
int *p;
int a[10];
int f(void);
```

这里 `*`、`[10]`、`(void)` 都属于 declarator 的一部分，也会参与构成完整类型。

因此更准确的模型是：

```text
declaration specifiers
        +
declarator
        ↓
共同决定被声明实体的类型
```

完整 fundamental types 在：

```text
002-fundamental-types/
```

学习。

整数宽度、signed/unsigned 运算、promotion 等属于：

```text
04-integers-and-bits/
```

---

## 5. Declaration — 声明

### 5.1 Declaration 做什么

对本章讨论的普通声明，可以先理解为：

> declaration 向编译器引入 identifier，并描述它所表示实体的类型和其他属性。

例如：

```c
int count;
```

声明了 identifier：

```text
count
```

并说明它与 `int` 类型相关。

另一个例子：

```c
extern int system_tick;
```

也是 declaration。

它声明了 `system_tick`，但这条 declaration 本身通常并不定义相应 object。

> 严格的 C 语法中也存在不引入 identifier 的 declaration，例如某些 static assertion / attribute declaration。本章只讨论最常见的 identifier declarations。

---

### 5.2 Declare before use

现代 C 不应依赖编译器“猜测”一个 identifier 或 function 的类型。

例如：

```c
int add(int a, int b);

int main(void)
{
    int result = add(1, 2);
    return 0;
}
```

调用 `add()` 时，编译器已经看到了正确 function declaration。

这使编译器可以检查：

- argument count
- argument types
- return type
- type compatibility

SEI CERT C DCL31-C 将“使用前声明 identifier”作为明确规则。

> C23 允许某些使用 `auto` 的类型推导形式。本学习路线仍以显式类型声明为主，因为这更符合当前常见嵌入式 C 工程和较早 C 标准工具链的实际环境。

---

## 6. Declarator — 声明符

这一节只需要先解决一个问题：

> **declarator 到底是什么，为什么它不只是“变量名”的另一个叫法？**

最简单的答案是：

> **declarator 是围绕 identifier 的那部分声明语法，它和前面的 declaration specifiers 一起决定被声明实体的完整类型。**

先不要试图一次掌握复杂声明。只看四个最小例子。

---

### 6.1 从最简单的声明开始

```c
int count;
```

拆开：

```text
int        count
│          │
│          └── declarator
└───────────── type specifier
```

这里 declarator 恰好只有：

```text
count
```

而 `count` 同时也是 identifier。

所以在最简单的声明里：

```text
declarator = identifier
```

这也是为什么初学时很容易误以为：

> declarator 只是“变量名”的专业叫法。

但一旦声明稍微复杂，这个等式就不成立了。

---

### 6.2 Declarator 可以包含 identifier 周围的类型结构

比较下面四条声明：

```c
int count;
int *p;
int buffer[16];
int read_value(void);
```

它们都以 `int` 为基础 type specifier，但 declarator 不同：

| Declaration | Type specifier | Declarator | Identifier | 最终含义 |
| --- | --- | --- | --- | --- |
| `int count;` | `int` | `count` | `count` | `count` 是 `int` object |
| `int *p;` | `int` | `*p` | `p` | `p` 是 pointer to `int` |
| `int buffer[16];` | `int` | `buffer[16]` | `buffer` | `buffer` 是 array of 16 `int` |
| `int read_value(void);` | `int` | `read_value(void)` | `read_value` | `read_value` 是返回 `int` 的 function |

所以：

```text
identifier
```

只是 declarator 里面真正的“名字”。

而：

```text
*
[]
()
```

这些围绕 identifier 的语法也属于 declarator，并参与决定完整类型。

---

### 6.3 最重要的反例：`int *p, value;`

看：

```c
int *p, value;
```

如果误把：

```text
int *
```

整体当成“左边的类型”，很容易误以为：

```text
p     → pointer to int
value → pointer to int
```

但实际不是。

正确拆解是：

```text
int
    ↓
共同的 type specifier

*p
    ↓
第一个 declarator

value
    ↓
第二个 declarator
```

因此：

```text
p
    ↓
pointer to int

value
    ↓
int
```

也就是：

```c
int *p, value;
```

等价于分别写：

```c
int *p;
int value;
```

这个例子非常重要，因为它直接说明：

> **`*` 属于 declarator `*p`，而不是简单属于前面的 `int`。**

这也是为什么 declarator 这个概念不能被简化成“变量名”。

---

### 6.4 一个更容易记住的模型

当前阶段可以先把普通 declaration 看成：

```text
declaration specifiers
        +
declarator
        +
optional initializer
```

例如：

```c
unsigned int error_count = 0;
```

拆开：

```text
unsigned int
    ↓
declaration specifiers

error_count
    ↓
declarator
    ↓
其中的 identifier 也是 error_count

0
    ↓
initializer
```

再例如：

```c
int *p = 0;
```

拆开：

```text
int
    ↓
type specifier

*p
    ↓
declarator

p
    ↓
identifier

0
    ↓
initializer
```

最终：

```text
p 是一个 pointer-to-int object
```

> 这里暂时不用学习 pointer 本身，只需要知道 `*p` 整体是 declarator。

---

### 6.5 为什么 C 的 declarator 要这样写？

这是理解 declarator 最有帮助的一条思路。

C 的声明语法在很大程度上刻意让 declarator 看起来像以后使用这个 identifier 时的表达式形式。成熟 C 参考资料通常也用这个思路解释 declarator。

例如：

#### Pointer

```c
int *p;
```

以后如果：

```c
*p
```

那么 `*p` 得到的是一个 `int`。

可以帮助你把声明理解为：

```text
*p is int
    ↓
therefore
p is pointer to int
```

#### Array

```c
int buffer[16];
```

以后：

```c
buffer[0]
```

得到一个 `int`。

所以可以帮助理解为：

```text
buffer[i] is int
    ↓
buffer is array of int
```

#### Function

```c
int read_value(void);
```

以后调用：

```c
read_value()
```

得到一个 `int` 返回值。

所以：

```text
read_value() returns int
```

这不是完整的复杂声明解析算法，但对理解 C 为什么把 `*`、`[]`、`()` 放在 identifier 周围非常有帮助。

---

### 6.6 当前阶段只需要识别四种形状

先记：

```c
int value;
```

```text
declarator: value
→ int object
```

---

```c
int *p;
```

```text
declarator: *p
→ pointer to int
```

---

```c
int values[10];
```

```text
declarator: values[10]
→ array of 10 int
```

---

```c
int read_value(void);
```

```text
declarator: read_value(void)
→ function returning int
```

现在**不需要**学习这种复杂形式：

```c
int (*handler)(int);
int (*table[4])(void);
```

它们会在 pointer、array、function pointer 相关章节中逐步展开。

这一章只需要建立：

> **完整类型不是永远只写在 identifier 左边；declarator 本身也携带类型信息。**

---

### 6.7 一条 declaration 可以有多个 declarators

C 允许：

```c
int a, b, c;
```

它可以理解为：

```text
共同 declaration specifier:
    int

declarator 1:
    a

declarator 2:
    b

declarator 3:
    c
```

同样：

```c
int *p, value;
```

有：

```text
共同 specifier:
    int

declarator:
    *p

declarator:
    value
```

从语言上完全合法。

但在嵌入式工程和学习代码中，如果 declarator 的形状不同，本仓库优先拆开写：

```c
int *p;
int value;
```

而不是：

```c
int *p, value;
```

这样可以减少把 `value` 误读为 pointer 的风险，也更方便 code review。

---

### 6.8 当前阶段怎么判断 declarator

看到一条简单 declaration 时：

```c
int *p = 0;
```

可以按下面顺序：

```text
1. 先找 declaration specifier
   int

2. 再找 identifier
   p

3. 看 identifier 周围还有什么声明语法
   *p

4. 因此 declarator 是
   *p

5. specifier + declarator 一起得到完整类型
   p is pointer to int

6. 最后再看有没有 initializer
   0
```

对于现在的学习阶段，这套方法已经足够。

### 6.9 本节只需要记住三句话

```text
identifier 是名字。

declarator 包含 identifier，
并可能包含 *, [], () 等类型结构。

declaration specifiers + declarator
共同决定完整类型。
```

如果看到：

```c
int *p, value;
```

能够准确说出：

```text
*p     是一个 declarator
value  是另一个 declarator

p     是 pointer to int
value 是 int
```

那么 declarator 这个概念就已经掌握到当前阶段需要的程度。

---

## 7. Definition — 定义

### 7.1 Declaration 与 Definition 的关系

核心关系：

```text
Every definition is a declaration.
Not every declaration is a definition.
```

即：

> **definition 是 declaration 的一种，但 declaration 不一定是 definition。**

definition 会把相应实体完整定义出来。

对于 function：

```c
int add(int a, int b);
```

是 declaration。

而：

```c
int add(int a, int b)
{
    return a + b;
}
```

是 definition。

对于 object，需要进一步考虑 declaration 的形式和所在 scope。

---

### 7.2 Block scope object definition

例如：

```c
void foo(void)
{
    int count;
}
```

这里：

```c
int count;
```

定义了一个 block-scope object。

但要特别注意：

> **definition 不等于 initialization。**

```c
int count;
```

确实定义了 object，但对这种普通 automatic object 来说，没有 initializer 并不意味着它自动获得 0。

初始化规则在：

```text
003-initialization/
```

学习。

---

### 7.3 `extern` declaration

在 file scope：

```c
extern int system_tick;
```

通常是 declaration，不是 object definition。

更准确地说，它声明了一个具有相应 linkage 的 identifier；如果程序实际需要该 object，就必须在适当位置存在相应 definition。

例如：

```c
/* system.c */
int system_tick = 0;
```

定义了 object。

---

### 7.4 `extern` 带 initializer

在 **file scope**：

```c
extern int system_tick = 0;
```

这是 definition。

因此错误口诀：

```text
extern = 只声明、不定义
```

是不成立的。

正确理解是：

> 是否为 definition，要看完整 declaration 及其上下文。

另外，block scope 中具有 linkage 的 object declaration 不能这样带 initializer：

```c
void foo(void)
{
    // invalid form for this purpose
    // extern int system_tick = 0;
}
```

`extern`、scope 和 linkage 的完整规则属于：

```text
008-linkage-static-and-extern/
```

---

### 7.5 File scope 的 tentative definition

下面这行如果出现在 file scope：

```c
int count;
```

不是简单意义上的“普通 declaration”，而是 **tentative definition**。

例如：

```c
int count;
int count;
```

在同一个 translation unit 中可以构成多个 tentative definitions。

如果这个 translation unit 中始终没有出现真正的 external definition，那么在 translation unit 结束时，它会按规则表现为一个实际 definition，并获得相应的空初始化效果。

第一次学习只需要记住：

```text
block scope:
    int count;
    → object definition

file scope:
    int count;
    → tentative definition
```

更深入规则留给：

```text
008-linkage-static-and-extern/
```

---

## 8. Definition Does Not Mean Initialized

这是本章必须单独建立的概念。

比较：

```c
void foo(void)
{
    int a;
    int b = 0;
}
```

两行都定义了 object。

但是：

```text
a
    ↓
defined
but no explicit initializer

b
    ↓
defined
and explicitly initialized
```

所以：

```text
defined
    ≠
has a known initial value
```

实际初值取决于：

- storage duration
- initializer
- C 初始化规则

这些属于：

```text
003-initialization/
007-storage-duration-and-lifetime/
```

---

## 9. Initializer — 初始化器

看：

```c
int retry_count = 3;
```

从语法上可以理解为：

```text
int
    ↓
declaration specifier

retry_count
    ↓
declarator

3
    ↓
initializer expression
```

整体的：

```text
= 3
```

构成初始化语法的一部分。

### 9.1 Initialization 不等于 Assignment

比较：

```c
int count = 10;
count = 20;
```

第一行发生：

```text
definition
+
initialization
```

第二行发生：

```text
assignment
```

即：

```text
object 被定义时建立初始状态
    ↓
initialization

object 已经存在后写入新值
    ↓
assignment
```

完整规则在：

```text
003-initialization/
```

---

## 10. Function Declaration and Definition

declaration / definition 不只适用于 objects。

例如：

```c
int add(int a, int b);
```

这是 function declaration，并且因为它给出了参数类型列表，也是 function prototype。

它说明：

```text
function identifier: add
parameters: int, int
return type: int
```

而：

```c
int add(int a, int b)
{
    return a + b;
}
```

是 function definition。

完整函数知识属于：

```text
08-functions-and-api/
```

---

## 11. Reading a Simple Declaration

以后遇到 declaration，可以固定按下面顺序分析。

### Example 1

```c
int count;
```

先问：

1. declaration specifier 是什么？  
   `int`

2. declarator 是什么？  
   `count`

3. identifier 是什么？  
   `count`

4. 有 initializer 吗？  
   没有。

5. declaration 还是 definition？  
   取决于 scope：
   - block scope：object definition
   - file scope：tentative definition

---

### Example 2

```c
unsigned int error_count = 0;
```

拆解：

```text
unsigned int
    ↓
declaration specifiers

error_count
    ↓
declarator / identifier

0
    ↓
initializer
```

这是 object definition，并且 object 被显式初始化。

---

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
declarator / identifier
```

在典型 file-scope 用法中：

```text
declaration
not the object definition
```

---

### Example 4

```c
int *value_ptr;
```

即使暂时还没正式学习 pointer，也可以先正确拆解：

```text
int
    ↓
type specifier

*value_ptr
    ↓
declarator

value_ptr
    ↓
identifier
```

最终声明的是：

```text
value_ptr
    ↓
object of type pointer to int
```

这就是为什么必须尽早认识 declarator。

---

## 12. Embedded C Multi-file Pattern

一个典型嵌入式项目可能这样组织共享 object。

### Header

```c
/* system.h */

extern unsigned int system_tick;
```

### Source

```c
/* system.c */

unsigned int system_tick = 0;
```

关系：

```text
system.h
    ↓
extern unsigned int system_tick;
    ↓
共享 declaration

system.c
    ↓
unsigned int system_tick = 0;
    ↓
authoritative definition
```

其他源文件：

```c
#include "system.h"
```

从同一个 header 获得一致 declaration。

这是一种非常重要的工程模式：

```text
public declarations
    ↓
header

implementation / definitions
    ↓
source file
```

header、translation unit、compiler 和 linker 的工作方式属于：

```text
01-preprocessor-and-build/
```

linkage 的语言规则属于：

```text
008-linkage-static-and-extern/
```

---

## 13. Common Mistakes

### 13.1 把 declaration 和 definition 当成同义词

错误：

```text
declaration = definition
```

正确：

```text
definition ⊂ declaration
```

---

### 13.2 把 definition 和 initialization 当成同义词

错误：

```text
object 被定义
    ↓
一定已经有我期望的初始值
```

正确：

```text
definition
    ↓
实体被定义

initialization
    ↓
初始值规则是另一件事
```

---

### 13.3 认为所有 `extern` 都只是 declaration

```c
extern int value;
```

典型情况下不是 definition。

但在 file scope：

```c
extern int value = 10;
```

是 definition。

---

### 13.4 忽略 file scope tentative definition

```c
int value;
```

在 function 内与 file scope 下不是完全相同的语言情形。

必须先看 scope，再判断它是哪种 definition/declaration。

---

### 13.5 给同一实体写不兼容的 declarations

例如：

```c
/* a.c */
extern int sensor_count;
```

另一个文件：

```c
/* b.c */
short sensor_count;
```

如果它们指向同一个 object，这些 declarations 的类型不兼容。

SEI CERT C DCL40-C 指出，同一 function 或 object 出现不兼容 declarations 会导致 undefined behavior。

实际后果可能包括：

- 读取错误宽度
- 错误解释存储
- memory overwrite
- hardware trap

共享 declaration 应尽量来自同一个 authoritative header。

---

### 13.6 使用 reserved identifier

不要把：

```text
所有以下划线开头的 identifier 都是任何位置的语言错误
```

当成标准规则。

但作为嵌入式工程约定，本仓库仍建议：

> 用户定义名称不要以下划线开头。

这样最简单，也最不容易与 implementation-reserved names 冲突。

---

### 13.7 依赖旧式 implicit declarations

新代码不要依赖历史 C 行为去猜 function 或 object 的类型。

先提供正确 declaration，让编译器尽可能早地发现错误。

---

## 14. Embedded Engineering Rules

### Rule 1 — Declare before use

使用 identifier 前，确保正确 declaration 已经可见。

尤其是 function prototype。

对应参考：

```text
SEI CERT C DCL31-C
```

---

### Rule 2 — Keep repeated declarations compatible

同一个有 linkage 的 object 或 function 如果出现多次 declaration，它们的类型必须兼容。

对应参考：

```text
SEI CERT C DCL40-C
```

---

### Rule 3 — Put shared declarations in one authoritative header

不要在多个 `.c` 文件手写彼此可能逐渐漂移的 declarations。

优先：

```text
one shared declaration
        ↓
header

one intended external definition
        ↓
source file
```

---

### Rule 4 — Avoid implementation-reserved names

标准的 reserved identifier 规则比“不能用下划线”更细。

为了工程简单性，本仓库统一采用更保守约定：

```text
user-defined names
    ↓
do not begin with _
```

对应参考：

```text
SEI CERT C DCL37-C
```

---

### Rule 5 — Ensure an object has a valid value before reading it

完整初始化语义属于：

```text
003-initialization/
```

但从第一章就建立工程习惯：

> 在读取 object 之前，必须能够证明它具有有效值。

BARR-C:2018 也明确规定变量在使用前应初始化。

---

## 15. Key Takeaways

### 核心概念

```text
identifier
    ↓
源码中的名字

object
    ↓
执行环境中的数据存储实体

type
    ↓
描述实体和值的类型属性

declaration
    ↓
描述 identifier 及其实体属性

declarator
    ↓
说明 identifier 如何具有完整类型

definition
    ↓
真正定义实体

initializer
    ↓
为 object 建立初始值
```

### 三个不能混淆的关系

```text
definition ⊂ declaration
```

但：

```text
definition ≠ initialization
```

并且：

```text
identifier ≠ object
```

### 阅读 declaration 的固定顺序

看到一条 declaration 时，依次问：

```text
1. declaration specifiers 是什么？
2. declarator 是什么？
3. identifier 在哪里？
4. 完整 type 是什么？
5. 有没有 initializer？
6. 当前 scope 是什么？
7. 它只是 declaration、tentative definition，
   还是实际 definition？
```

如果这七个问题能逐步回答，本章的核心目标已经达到。

---

## 16. Related Knowledge

继续学习：

- [002-fundamental-types](../002-fundamental-types/) — fundamental types 与类型范围
- [003-initialization](../003-initialization/) — object 的初始值规则
- [006-scope-and-name-visibility](../006-scope-and-name-visibility/) — identifier 在哪里可见
- [007-storage-duration-and-lifetime](../007-storage-duration-and-lifetime/) — object 存在多久
- [008-linkage-static-and-extern](../008-linkage-static-and-extern/) — 多个 translation unit 如何共享或隐藏名字
- [01-preprocessor-and-build](../../01-preprocessor-and-build/) — header、translation unit、compiler 和 linker
- [06-pointers-and-memory](../../06-pointers-and-memory/) — pointer declarator 和指针语义
- [08-functions-and-api](../../08-functions-and-api/) — function declarations、prototypes 和接口

---

## 17. References

本章以 C 语言标准语义为基线，结合成熟工程规则重新组织。

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
9. **BARR-C:2018 Embedded C Coding Standard — 7.2 Initialization**  
   https://barrgroup.com/72-initialization

The wording in this note is a learning-oriented synthesis rather than a reproduction of the source standards.
