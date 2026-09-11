# C

Reusable C knowledge for embedded systems development.

This directory is organized by **problem domain**, not by textbook chapter order. The primary goal is fast lookup during embedded development, debugging, and review. A separate learning path is provided below without encoding learning order into directory names.

## Organization

```text
C/
├── README.md
├── language-basics/
├── types-and-values/
├── integers-and-bits/
├── expressions-and-control-flow/
├── functions-and-interfaces/
├── pointers-arrays-and-buffers/
├── objects-and-data-layout/
├── scope-lifetime-and-linkage/
├── preprocessor-and-build/
├── hardware-access/
├── concurrency-and-interrupts/
└── reliability-and-portability/
```

Each module owns one problem domain. Knowledge entries inside a module use:

```text
NNN-name/
└── README.md
```

Numbering is local to the module. Use the next available number and never renumber existing entries.

## Module Index

| Module | Main concerns |
| --- | --- |
| [language-basics](./language-basics/) | Core C syntax, declarations, statements, and basic control structures |
| [types-and-values](./types-and-values/) | Fundamental types, qualifiers, `typedef`, `enum`, values, and representations at the language level |
| [integers-and-bits](./integers-and-bits/) | Fixed-width integers, signed/unsigned rules, promotions, conversions, overflow, shifts, masks, and bit operations |
| [expressions-and-control-flow](./expressions-and-control-flow/) | Operators, precedence, sequencing, side effects, conditions, loops, and `switch` |
| [functions-and-interfaces](./functions-and-interfaces/) | Function declarations, parameters, return values, callbacks, function pointers, and C API design |
| [pointers-arrays-and-buffers](./pointers-arrays-and-buffers/) | Pointers, arrays, strings, pointer arithmetic, buffers, bounds, and `void *` |
| [objects-and-data-layout](./objects-and-data-layout/) | `struct`, `union`, padding, alignment, endianness, object representation, and aliasing |
| [scope-lifetime-and-linkage](./scope-lifetime-and-linkage/) | Scope, storage duration, lifetime, `static`, `extern`, globals, locals, and dynamic storage |
| [preprocessor-and-build](./preprocessor-and-build/) | Source/header organization, macros, includes, translation units, compilation, linking, symbols, and sections |
| [hardware-access](./hardware-access/) | Memory-mapped I/O, `volatile`, register access, read-modify-write, and hardware-facing C patterns |
| [concurrency-and-interrupts](./concurrency-and-interrupts/) | ISR/main sharing, atomicity, races, reentrancy, critical sections, and C atomics where applicable |
| [reliability-and-portability](./reliability-and-portability/) | Undefined, unspecified, and implementation-defined behavior; optimization, diagnostics, portability, and coding standards |

## Quick Lookup

| Problem or symptom | Start here |
| --- | --- |
| `uint8_t + uint8_t` behaves unexpectedly | [integers-and-bits](./integers-and-bits/) |
| Signed/unsigned comparison gives a surprising result | [integers-and-bits](./integers-and-bits/) |
| A shift or register mask is wrong | [integers-and-bits](./integers-and-bits/) |
| Pointer access causes a fault | [pointers-arrays-and-buffers](./pointers-arrays-and-buffers/) |
| Buffer, array, or string is corrupted | [pointers-arrays-and-buffers](./pointers-arrays-and-buffers/) |
| `sizeof(struct)` is larger than expected | [objects-and-data-layout](./objects-and-data-layout/) |
| Protocol bytes appear in the wrong order | [objects-and-data-layout](./objects-and-data-layout/) |
| Unsure what `static` or `extern` means in context | [scope-lifetime-and-linkage](./scope-lifetime-and-linkage/) |
| Header include or macro behaves unexpectedly | [preprocessor-and-build](./preprocessor-and-build/) |
| Linker reports an undefined or duplicate symbol | [preprocessor-and-build](./preprocessor-and-build/) |
| Peripheral register access is optimized away or behaves oddly | [hardware-access](./hardware-access/) |
| ISR and main/RTOS code share data incorrectly | [concurrency-and-interrupts](./concurrency-and-interrupts/) |
| Debug build works but optimized build fails | [reliability-and-portability](./reliability-and-portability/) |

When a problem spans modules, start with the module that best matches the **observed symptom**, then follow cross-links from the relevant entry.

## Learning Path

Directory structure is for lookup. The recommended learning path is separate:

```text
language-basics
    ↓
types-and-values
    ↓
expressions-and-control-flow
    ↓
functions-and-interfaces
    ↓
pointers-arrays-and-buffers
    ↓
integers-and-bits
    ↓
objects-and-data-layout
    ↓
scope-lifetime-and-linkage
    ↓
preprocessor-and-build
    ↓
hardware-access
    ↓
concurrency-and-interrupts
    ↓
reliability-and-portability
```

The path is guidance, not a dependency graph. Entries should remain useful as independent references.

## Entry Rules

1. Put an entry in the module that owns its **primary concept**.
2. Do not duplicate the same explanation in multiple modules; use relative links for related concepts.
3. Keep one entry focused on one reusable concept or tightly coupled concept set.
4. Prefer examples that are relevant to embedded C.
5. Explain important failure modes next to the concept that causes them.
6. Undefined behavior should be explained locally where it occurs and may also be indexed from `reliability-and-portability/`.
7. Module `README.md` files act as indexes and scope definitions, not long-form chapters.
8. Add entries only when they are actually learned or needed; do not pre-create empty numbered entries.

## Reference Baseline

Use the C language standard as the semantic baseline. For embedded engineering practice, use safety and reliability guidance such as MISRA C, SEI CERT C, and established embedded C coding standards as secondary references.

Project-specific rules belong in `projects/`, not here.
