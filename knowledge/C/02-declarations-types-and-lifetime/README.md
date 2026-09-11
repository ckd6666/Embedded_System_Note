# 02 — Declarations, Types, and Lifetime

How C names and objects are declared, typed, initialized, made visible, stored, and connected across a program.

This module follows a stable learning sequence inspired by ISO C language semantics, the SEI CERT C DCL problem domain, and embedded-C engineering guidance such as MISRA C and BARR-C.

## Module Goal

Build a correct mental model for C declarations and objects before moving into deeper pointer, integer, memory-layout, and hardware-access topics.

After completing this module, you should be able to answer:

- What exactly is being declared or defined?
- What type does an object or identifier have?
- How is an object initialized?
- Where is a name visible?
- How long does an object exist?
- Does a name have linkage?
- What do `const`, `static`, and `extern` actually mean in context?

## Chapter Order

1. [001-variables-declarations-and-definitions](./001-variables-declarations-and-definitions/) — objects, variables, identifiers, declarations, definitions, and initializers.
2. [002-fundamental-types](./002-fundamental-types/) — C fundamental types and what a type describes.
3. [003-initialization](./003-initialization/) — explicit initialization, zero initialization, indeterminate values, and initialization rules.
4. [004-typedef-and-enum](./004-typedef-and-enum/) — type aliases, enumerated types, and named constants.
5. [005-const-and-type-qualifiers](./005-const-and-type-qualifiers/) — `const` and the role of type qualifiers.
6. [006-scope-and-name-visibility](./006-scope-and-name-visibility/) — where identifiers are visible and how nested scopes interact.
7. [007-storage-duration-and-lifetime](./007-storage-duration-and-lifetime/) — when objects exist and how storage duration differs from scope.
8. [008-linkage-static-and-extern](./008-linkage-static-and-extern/) — internal/external linkage and the meanings of `static` and `extern`.

## Boundaries

This module owns declaration, type, scope, storage-duration, lifetime, and linkage semantics.

Related topics belong elsewhere:

- Integer promotions, signed/unsigned arithmetic, overflow, shifts, and masks → [04-integers-and-bits](../04-integers-and-bits/)
- Arrays, strings, and buffer bounds → [05-arrays-strings-and-buffers](../05-arrays-strings-and-buffers/)
- Pointer mechanics and pointer-qualified forms → [06-pointers-and-memory](../06-pointers-and-memory/)
- `struct`, `union`, padding, alignment, and object representation → [07-objects-and-data-layout](../07-objects-and-data-layout/)
- Function interfaces and callbacks → [08-functions-and-api](../08-functions-and-api/)
- Hardware-facing `volatile` usage → [10-hardware-access](../10-hardware-access/)
- C atomics and synchronization → [11-concurrency-and-interrupts](../11-concurrency-and-interrupts/)

## Study Rule

Learn these chapters in order on the first pass. During later development, use them as independent references.

Engineering rules from CERT C, MISRA C, and BARR-C should be recorded next to the language concept they constrain rather than isolated into a separate "best practices" chapter.
