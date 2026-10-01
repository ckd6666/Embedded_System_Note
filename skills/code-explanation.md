# Code Explanation Rules

Applies to AI explanations of code during embedded-system learning.

## 1. Start With the Program Meaning

Before explaining details, state in one sentence what the code block exists to do.

For a function, establish these first when relevant:

- why it exists,
- when it runs,
- what it receives or depends on,
- what it changes or produces.

Do not begin with line-by-line translation.

## 2. Explain by Subgoals

Break the code into a few functional subgoals and name each by what it accomplishes, such as:

- configure USART,
- check the interrupt source,
- send the buffer,
- update program state.

Explain the subgoals before inspecting individual statements.

For a complete function, explain its overall purpose and execution path before its parameters, syntax, or implementation details.

## 3. Trace What Actually Executes

When execution is important, trace the order the CPU actually follows.

Do not confuse where code is written with when it executes.

Distinguish clearly between:

- normal function calls,
- loops,
- interrupt handlers or callbacks,
- initialization,
- runtime events.

For embedded code, keep these flows separate when they differ:

- source-code structure,
- CPU control flow,
- data flow,
- hardware signal or interrupt flow.

Do not combine initialization/configuration with runtime signal flow into one sequence.

## 4. Build a Concrete Mental Model

When the code is difficult to understand, trace one small concrete case through it.

Track the values or state that actually change.

For example, if a function sends a buffer, a short value such as `"ABC"` may be traced as:

`printf` -> buffer -> `_write` -> USART API -> USART peripheral -> TX pin -> computer.

Use one useful example rather than several repetitive examples.

## 5. Move Between Abstraction Levels Carefully

Prefer this order:

1. Explain the idea in simple language.
2. Introduce the precise technical term.
3. Map the term back to the exact code.

When relevant, distinguish these layers:

- C language,
- project code,
- library or API,
- MCU internal hardware,
- board-level circuit,
- external device or computer.

Do not leave an analogy or simplified description in place when a more precise model is needed.

## 6. Classify Unfamiliar Code Before Explaining It

When an unfamiliar identifier matters, first identify what kind of thing it is:

- C syntax or language feature,
- project-defined macro, variable, or function,
- library API,
- library-provided constant or macro,
- interrupt service routine or callback,
- hardware register or hardware concept.

Do not call every unfamiliar function-like name an API.

For an unfamiliar hardware API, explain only:

1. Where it comes from.
2. What it does and what its parameters mean.
3. What hardware step it represents in the current project.

## 7. Control Cognitive Load

Explain only the details needed to understand the current code.

Do not stop on every type, operator, keyword, register detail, or related concept.

Expand a detail when:

- it is unfamiliar and important,
- it changes the meaning of the current code,
- or it blocks understanding of the current flow.

Keep side topics deferred until they become necessary.

For board-level wiring, let the learner inspect the schematic first. Explain specific components or connections when asked.

## 8. Return to the Whole Program

After explaining the necessary details, reconnect them to the original code block.

The learner should be able to answer:

- Why does this code exist?
- When does it execute?
- What path does execution or data follow?
- What state, hardware, or output does it change?
