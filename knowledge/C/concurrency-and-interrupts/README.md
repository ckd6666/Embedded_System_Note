# Concurrency and Interrupts

C-level reasoning about data shared across execution contexts.

## Scope

- ISR/main shared state
- atomicity
- race conditions
- reentrancy
- critical sections
- compiler-visible sharing
- `_Atomic` and C atomics where applicable
- interaction between `volatile` and synchronization

`volatile` hardware access belongs primarily in `../hardware-access/`.

## Look Here When

Interrupts, RTOS tasks, DMA callbacks, or other execution contexts share data.

## Entries

No entries yet.
