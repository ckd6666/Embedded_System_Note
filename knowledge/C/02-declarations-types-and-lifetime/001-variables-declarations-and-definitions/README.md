# Variables, Declarations, and Definitions

## Definition

| Concept | Definition |
| --- | --- |
| identifier | 由 identifier-start 后接零个或多个 identifier-continue 字符组成、用于指示一个或多个实体的词法记号。 |
| object | 执行环境中的数据存储区域，其内容可以表示值。 |
| variable | 常用工程术语；涉及 ISO C 的精确语义时优先使用 `object`。 |
| declaration | 指定一组 identifiers 的解释和属性。 |
| declarator | declaration 中包含被声明 identifier（若有），并可提供附加类型信息的部分。 |
| definition | declaration 的一种；对于 object，definition 使该 object 的存储被保留。 |
| initializer | 与 declarator 关联、用于为 object 提供初始值的初始化语法。 |

必要关系：

- `identifier` 与 `object` 不是同一概念。
- 每个 definition 都是 declaration，但 declaration 不一定是 definition。
- Definition 与 initialization 是不同概念。

## Core Rules

### Identifier and Object

Identifier 用于指示实体；object 是执行环境中的数据存储实体，因此二者不是同一概念。

Object 的类型、storage duration 和 lifetime 分别由其他语言规则规定。

### Declaration

C23 中，declaration 用于指定一组 identifiers 的解释和属性。

Declaration specifiers 指示 declarator 所表示实体的 linkage、storage duration 以及部分类型信息；declarator 包含被声明的 identifier（若有），并可提供附加类型信息。

一个 declaration 可以包含多个 declarators。每个 declarator 分别与公共 declaration specifiers 组合后确定相应 identifier 的完整声明类型。

Identifier 在使用前必须具有适用的 declaration；SEI CERT C DCL31-C 对此作出明确要求。

### Declarator

Declarator 是 declaration 的组成部分，不等同于 identifier。

Declarator 可以只包含 identifier，也可以包含改变或补充完整类型的声明语法。完整的 pointer、array 和 function declarator 规则分别属于对应主题，本条目不展开。

### Definition

Definition 是 declaration 的一种。

对于 object，definition 使存储被保留；对于 function，definition 包含 function body。Enumeration constant 和 typedef name 也具有标准规定的 definition 条件。

File scope 下某些 object declarations 属于 tentative definitions。Tentative definition、`extern` 与 linkage 的完整规则见 [008-linkage-static-and-extern](../008-linkage-static-and-extern/)。

Object 被定义不等于已经显式初始化。Initialization 的完整规则见 [003-initialization](../003-initialization/)。

### Initializer

Object declaration 可以通过 initialization 提供初始值。Initializer 是 initialization 语法的一部分。

### Compatible Declarations

同一 scope 中引用同一 object 或 function 的 declarations 必须指定 compatible types。

SEI CERT C DCL40-C 同样要求不得为同一 function 或 object 创建 incompatible declarations。

## Related Knowledge

- [002-fundamental-types](../002-fundamental-types/) — fundamental types
- [003-initialization](../003-initialization/) — initialization
- [006-scope-and-name-visibility](../006-scope-and-name-visibility/) — scope and visibility
- [007-storage-duration-and-lifetime](../007-storage-duration-and-lifetime/) — storage duration and lifetime
- [008-linkage-static-and-extern](../008-linkage-static-and-extern/) — linkage, `static`, `extern`, and tentative definitions
- [01-preprocessor-and-build](../../01-preprocessor-and-build/) — headers, translation units, and linking
- [06-pointers-and-memory](../../06-pointers-and-memory/) — pointer declarators and pointer semantics
- [08-functions-and-api](../../08-functions-and-api/) — function declarations and definitions

## References

1. **ISO/IEC 9899:2024 (C23), 6.7 Declarations** — declaration, definition, declaration specifiers, declarators.
2. **WG14 N3220 — ISO/IEC 9899:2024 working draft**  
   https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3220.pdf
3. **WG14 N3096 — ISO/IEC 9899:2023 working draft** — object terminology.  
   https://www.open-std.org/jtc1/sc22/wg14/www/docs/n3096.pdf
4. **cppreference — Declarations**  
   https://en.cppreference.com/c/language/declarations
5. **cppreference — External and tentative definitions**  
   https://en.cppreference.com/c/language/extern
6. **cppreference — Initialization**  
   https://en.cppreference.com/c/language/initialization
7. **SEI CERT C — DCL31-C: Declare identifiers before using them**  
   https://cmu-sei.github.io/secure-coding-standards/sei-cert-c-coding-standard/rules/declarations-and-initialization-dcl/dcl31-c/
8. **SEI CERT C — DCL40-C: Do not create incompatible declarations of the same function or object**  
   https://cmu-sei.github.io/secure-coding-standards/sei-cert-c-coding-standard/rules/declarations-and-initialization-dcl/dcl40-c/
