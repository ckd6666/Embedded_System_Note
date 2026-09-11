# C

Reusable C knowledge for embedded systems development.

This knowledge base is organized for two use cases:

1. **Lookup** — quickly locate the concept behind a problem encountered during embedded development.
2. **Learning and review** — follow a recommended path without coupling that path to the physical directory layout.

The classification is inspired by established C and embedded-C bodies of knowledge, especially ISO C, SEI CERT C, MISRA C, and BARR-C. The structure adapts those systems for practical embedded learning and reference rather than copying any one standard verbatim.

## Structure

```text
C/
├── README.md
├── 01-preprocessor-and-build/
├── 02-declarations-types-and-lifetime/
├── 03-expressions-and-control-flow/
├── 04-integers-and-bits/
├── 05-arrays-strings-and-buffers/
├── 06-pointers-and-memory/
├── 07-objects-and-data-layout/
├── 08-functions-and-api/
├── 09-error-handling/
├── 10-hardware-access/
├── 11-concurrency-and-interrupts/
└── 12-reliability-and-portability/
```

The two-digit module number gives each knowledge domain a stable catalog position. It is **not** a strict prerequisite order.

Inside each module, concrete knowledge entries use:

```text
NNN-name/
├── README.md
├── examples.c
└── exercises.md
```

The default entry template is:

- `README.md` — required. The primary learning and lookup document.
- `examples.c` — optional. Add when runnable or compiler-observable examples materially improve understanding.
- `exercises.md` — optional. Add for chapters in the active learning path when deliberate practice is useful.

Do not create optional files merely to satisfy the template. Add them when they serve a clear learning purpose.

Numbering is local to the module. Use the next available number and never renumber existing entries.

## Module Index

| No. | Module | Main concerns |
| --- | --- | --- |
| 01 | [Preprocessor and Build](./01-preprocessor-and-build/) | Source/header organization, preprocessing, macros, translation units, compilation, linking, symbols, and sections |
| 02 | [Declarations, Types, and Lifetime](./02-declarations-types-and-lifetime/) | Declarations, definitions, types, qualifiers, scope, storage duration, lifetime, linkage, `static`, and `extern` |
| 03 | [Expressions and Control Flow](./03-expressions-and-control-flow/) | Operators, precedence, sequencing, side effects, conditions, loops, and `switch` |
| 04 | [Integers and Bits](./04-integers-and-bits/) | Integer widths, signed/unsigned rules, promotions, conversions, overflow, shifts, masks, and bit operations |
| 05 | [Arrays, Strings, and Buffers](./05-arrays-strings-and-buffers/) | Arrays, strings, bounds, explicit lengths, multidimensional arrays, and embedded buffers |
| 06 | [Pointers and Memory](./06-pointers-and-memory/) | Pointer semantics, pointer arithmetic, `void *`, null/invalid pointers, ownership, and dynamic storage |
| 07 | [Objects and Data Layout](./07-objects-and-data-layout/) | `struct`, `union`, size, alignment, padding, endianness, object representation, bit-fields, and aliasing |
| 08 | [Functions and API](./08-functions-and-api/) | Function declarations, parameters, return values, callbacks, function pointers, API design, and validation |
| 09 | [Error Handling](./09-error-handling/) | Status codes, error propagation, assertions, timeouts, failure reporting, and recovery patterns |
| 10 | [Hardware Access](./10-hardware-access/) | Memory-mapped I/O, `volatile`, register access, read-modify-write, and hardware-visible side effects |
| 11 | [Concurrency and Interrupts](./11-concurrency-and-interrupts/) | ISR/main sharing, races, atomicity, reentrancy, critical sections, and C atomics where applicable |
| 12 | [Reliability and Portability](./12-reliability-and-portability/) | Undefined/unspecified/implementation-defined behavior, optimization, diagnostics, static analysis, MISRA, CERT, and portability |

## Quick Lookup

| Problem or symptom | Start here |
| --- | --- |
| Header, macro, include, or build-stage problem | [01-preprocessor-and-build](./01-preprocessor-and-build/) |
| `static`, `extern`, scope, lifetime, or type declaration is unclear | [02-declarations-types-and-lifetime](./02-declarations-types-and-lifetime/) |
| Expression result or control flow is surprising | [03-expressions-and-control-flow](./03-expressions-and-control-flow/) |
| Signed/unsigned, integer promotion, overflow, shift, or mask issue | [04-integers-and-bits](./04-integers-and-bits/) |
| Array, string, UART/SPI/DMA buffer, or bounds problem | [05-arrays-strings-and-buffers](./05-arrays-strings-and-buffers/) |
| Pointer fault, invalid address, `void *`, or dynamic memory issue | [06-pointers-and-memory](./06-pointers-and-memory/) |
| `sizeof(struct)`, padding, alignment, endian, protocol layout, or aliasing issue | [07-objects-and-data-layout](./07-objects-and-data-layout/) |
| Callback, function pointer, parameter, return value, or driver API issue | [08-functions-and-api](./08-functions-and-api/) |
| Timeout, status code, assertion, or failure-propagation design | [09-error-handling](./09-error-handling/) |
| Peripheral register or memory-mapped I/O behaves unexpectedly | [10-hardware-access](./10-hardware-access/) |
| ISR/main/RTOS contexts share data incorrectly | [11-concurrency-and-interrupts](./11-concurrency-and-interrupts/) |
| Debug works but optimized/release build fails, or code is compiler/target dependent | [12-reliability-and-portability](./12-reliability-and-portability/) |

When a problem spans modules, start with the module that best matches the **observed symptom**, then follow cross-links.

## Recommended Learning Path

The physical module numbers are stable catalog positions, not mandatory learning order. A practical embedded-C learning path is:

```text
02 declarations, types, and lifetime
        ↓
03 expressions and control flow
        ↓
08 functions and API
        ↓
05 arrays, strings, and buffers
        ↓
06 pointers and memory
        ↓
04 integers and bits
        ↓
07 objects and data layout
        ↓
01 preprocessor and build
        ↓
10 hardware access
        ↓
09 error handling
        ↓
11 concurrency and interrupts
        ↓
12 reliability and portability
```

The path is guidance only. Individual entries should remain useful as references.

## Entry Rules

1. Put an entry in the module that owns its **primary concept**.
2. Do not duplicate a full explanation across modules; use relative links for related concepts.
3. Keep one entry focused on one reusable concept or a tightly coupled concept set.
4. Prefer examples relevant to embedded C.
5. Explain important failure modes next to the concept that causes them.
6. Undefined behavior should be explained locally where it occurs and may also be indexed from `12-reliability-and-portability/`.
7. Module `README.md` files define scope and act as indexes; long-form knowledge belongs in numbered entries.
8. Add entries only when actually learned or needed. Do not pre-create empty numbered entries.
9. Preserve existing entry numbers. New entries use the next available number within their module.
10. Project-specific implementation details belong in `projects/`, not in this reusable knowledge base.

## Reference Baseline

Use the C language standard as the semantic baseline.

For engineering practice and review, use recognized guidance such as:

- ISO C — language semantics and normative behavior
- SEI CERT C — secure and reliable C rules organized by problem domain
- MISRA C — predictable, analyzable C for critical and embedded systems
- BARR-C — practical embedded-C coding guidance

These references inform the knowledge organization; this repository remains a learning and lookup system rather than a substitute for the standards themselves.
