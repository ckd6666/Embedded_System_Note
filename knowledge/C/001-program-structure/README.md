# C Program Structure

## Purpose

Understand how a C program is split into source files and header files, and how those files become an executable program.

This is foundational knowledge for later topics such as functions, declarations, header guards, libraries, and multi-file embedded projects.

## 1. Source Files

C implementation code is normally written in `.c` files.

Example:

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

A source file can contain:

- function definitions
- object definitions
- declarations
- preprocessor directives

## 2. Header Files

Header files normally use the `.h` extension and contain declarations or definitions that need to be shared between source files.

Example:

```c
// led.h

#ifndef LED_H
#define LED_H

void led_init(void);
void led_toggle(void);

#endif
```

The header tells other source files what interfaces are available. The actual function definitions would normally live in `led.c`.

## 3. `#include`

The preprocessor handles `#include` before normal C compilation.

```c
#include <stdbool.h>
#include "led.h"
```

Common convention:

- `<...>` — system or toolchain headers
- `"..."` — project headers

Conceptually, the included header content becomes visible to the current translation unit before compilation.

## 4. Header Guards

A header guard prevents the same header contents from being processed more than once in one translation unit.

```c
#ifndef LED_H
#define LED_H

void led_init(void);

#endif
```

The three important directives are:

- `#ifndef LED_H` — continue only if `LED_H` has not been defined
- `#define LED_H` — define the macro
- `#endif` — end the conditional block

The macro name itself does not provide functionality. It is simply a unique marker used by the preprocessor.

## 5. Declaration vs. Definition

A **declaration** tells the compiler that a name and its type exist.

```c
int add(int a, int b);
```

A **definition** provides the actual function body or creates the object.

```c
int add(int a, int b)
{
    return a + b;
}
```

A common multi-file pattern is:

```text
math.h   -> function declarations
math.c   -> function definitions
main.c   -> uses the functions through math.h
```

## 6. From Source Code to Program

A simplified C build process is:

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

Checks C syntax and semantics and translates each translation unit toward machine code.

### Assembly

Produces object code for the target architecture.

### Linking

Combines object files and libraries and resolves references between them.

For example, `main.c` may call `led_init()`, while the actual definition is compiled from `led.c`. The linker connects those pieces.

## 7. Translation Unit

After preprocessing, a source file together with everything brought in through its includes forms a **translation unit**.

This explains why declarations from headers are visible while compiling a particular `.c` file.

## 8. Typical Embedded C Layout

A small embedded module often looks like:

```text
src/
├── main.c
└── led.c

include/
└── led.h
```

The exact directory layout varies between projects. The important relationship is:

```text
header -> public declarations
source -> implementation
caller -> includes header and uses the interface
```

## Key Points

- `.c` files normally contain implementations.
- `.h` files normally expose shared declarations.
- `#include` is handled by the preprocessor.
- Header guards prevent duplicate header processing.
- A declaration introduces a name and type; a definition provides the implementation or storage.
- C source files are compiled separately and connected by the linker.
- Understanding translation units is essential for later learning about scope, linkage, `static`, and `extern`.
