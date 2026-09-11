# 11 — Concurrency and Interrupts

C-level reasoning about data shared across execution contexts.

## Scope

- interrupt service routines
- ISR/main shared state
- task/shared-state interaction
- atomicity
- race conditions
- reentrancy
- critical sections
- compiler-visible sharing
- `_Atomic` and C atomics where applicable
- why `volatile` is not synchronization

Hardware-facing use of `volatile` belongs primarily in `../10-hardware-access/`.

## Look Here When

Interrupts, RTOS tasks, callbacks, DMA completion paths, or other execution contexts share data.

## Entries

No entries yet.
