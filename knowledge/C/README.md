# C

This module contains reusable knowledge about the C language, with emphasis on concepts that are important for embedded systems development.

## Scope

Topics in this module include:

- C program structure and the build process
- Types, expressions, operators, and control flow
- Functions and scope
- Arrays and strings
- Pointers and memory
- Structures, unions, and enumerations
- Preprocessor and header files
- Storage duration and linkage
- Bitwise operations
- `const`, `volatile`, and `static`
- Common undefined and implementation-defined behavior relevant to embedded C

The goal is not to mirror a textbook chapter by chapter. Each entry should capture one independent, reusable concept.

## Organization

Each knowledge entry is a directory named:

```text
NNN-name/
└── README.md
```

Use the next available number when adding an entry. Existing entries are never renumbered.

## Learning Order

The numbered order is the default learning order, but entries should remain independently useful as references.

## Index

- [001-program-structure](./001-program-structure/) — C source files, header files, preprocessing, compilation, and linking.
