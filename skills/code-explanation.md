# Code Explanation Rules

Applies to AI explanations of code during embedded-system learning.

## 1. Explain Whole Before Detail

Start with what the code block exists to do.

Explain its functional blocks and execution path before individual statements. Do not default to line-by-line translation.

For a function, make clear why it exists, when it runs, what it depends on, and what it changes or produces.

## 2. Trace Actual Execution

When order matters, follow what the CPU or hardware actually does.

Do not confuse where code is written with when it executes.

Keep these separate when they differ:

- source-code structure,
- CPU control flow,
- data flow,
- hardware signal or interrupt flow.

Do not mix initialization/configuration with runtime behavior.

## 3. Identify Unfamiliar Code Correctly

Before explaining an unfamiliar item, identify what it is: C syntax, project code, library API or constant, ISR/callback, or hardware concept.

For an unfamiliar hardware API, explain:

1. Where it comes from.
2. What it does and what its parameters mean.
3. What hardware step it represents in the current project.

## 4. Explain Only What Helps the Current Understanding

Use one small concrete trace when the code is still abstract.

Explain syntax or implementation details only when they affect or block understanding of the current flow.

Keep software, MCU-internal hardware, board-level circuit, and external-device behavior distinct.
