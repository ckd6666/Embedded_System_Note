# C Program Structure

## Purpose

Understand how a C program is split into source files and header files, and how those files become an executable program.

This entry is the foundation for later topics such as declarations, header guards, translation units, libraries, symbols, and multi-file embedded projects.

## Source Files

C implementation code is normally written in `.c` files.

```c
// main.c

#include "led.h"

int main(void)
{
    led_init();

    while (1)
    {
        led_toggle();
    }
}
```

A source file can contain function definitions, object definitions, declarations, and preprocessor directives.

## Header Files

Header files normally use the `.h` extension and expose declarations that need to be shared.

```c
// led.h

#ifndef LED_H
#define LED_H

void led_init(void);
void led_toggle(void);

#endif
```

The header exposes the interface. The function definitions would normally live in `led.c`.

## Include Directives

The preprocessor handles `#include` before normal C compilation.

```c
#include <stdbool.h>
#include "led.h"
```

Common convention:

- `<...>` for system or toolchain headers
- `"..."` for project headers

## Header Guards

A header guard prevents the same header contents from being processed more than once in one translation unit.

```c
#ifndef LED_H
#define LED_H

void led_init(void);

#endif
```

- `#ifndef LED_H` checks whether the marker is absent.
- `#define LED_H` defines the marker.
- `#endif` closes the conditional block.

The macro name is only a preprocessor marker.

## Declaration vs. Definition

A declaration tells the compiler that a name and type exist.

```c
int add(int a, int b);
```

A definition provides a function body or defines an object.

```c
int add(int a, int b)
{
    return a + b;
}
```

Typical relationship:

```text
math.h   -> shared declarations
math.c   -> definitions
main.c   -> includes math.h and uses the interface
```

For deeper rules about scope, storage duration, and linkage, see `../../02-declarations-types-and-lifetime/`.

## Translation Unit

After preprocessing, one source file together with the content made visible through its includes forms a translation unit.

Each translation unit is compiled separately before the linker connects references between them.

## Build Process

A simplified build flow is:

```text
source code
    ↓
preprocessing
    ↓
compilation
    ↓
assembly
    ↓
object files
    ↓
linking
    ↓
executable / firmware image
```

### Preprocessing

Processes directives such as:

```c
#include
#define
#if
#ifndef
#endif
```

### Compilation

Checks C syntax and semantics and translates a translation unit toward target machine code.

### Assembly

Produces object code for the target architecture.

### Linking

Combines object files and libraries and resolves references between them.

For example, `main.c` may call `led_init()`, while the function definition is compiled from `led.c`. The linker resolves that reference.

## Typical Embedded Layout

```text
src/
├── main.c
└── led.c

include/
└── led.h
```

The exact directory layout varies. The important relationship is:

```text
header -> shared interface
source -> implementation
caller -> includes the header and uses the interface
```

## Key Points

- `.c` files normally contain implementations.
- `.h` files normally expose shared declarations.
- `#include` is handled during preprocessing.
- Header guards prevent duplicate header processing inside a translation unit.
- Declarations introduce names and types; definitions provide implementations or objects.
- Source files are compiled separately.
- The linker resolves references between compiled units.
- Translation units are essential for understanding `static`, `extern`, and linkage.
