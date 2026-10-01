# Code Explanation Rules

Applies to AI explanations of code during embedded-system learning.

## 1. Start With the Whole Block

Before explaining details, state in one sentence what the code block exists to do.

Then show the main execution path or data path in the order it actually happens.

Do not begin with line-by-line translation.

## 2. Explain by Functional Blocks

Group code by purpose, such as:

- initialization,
- input,
- processing,
- output,
- interrupt handling,
- error handling.

Explain what each block contributes to the program before inspecting individual lines.

For a complete function, explain its purpose and execution flow before its parameters, syntax, or implementation details.

## 3. Trace Real Execution Order

When control flow matters, explain the order the CPU actually executes code.

Distinguish clearly between:

- normal function calls,
- loops,
- callbacks or interrupt handlers,
- initialization code,
- runtime events.

When explaining hardware behavior, keep initialization/configuration separate from runtime signal or data flow. Do not mix them into one sequence.

## 4. Identify What an Unfamiliar Item Is

Before explaining an unfamiliar name, identify its kind when useful:

- C syntax or language feature,
- project-defined macro or function,
- library API,
- library-provided constant or macro,
- interrupt service routine or callback,
- hardware register or hardware concept.

Do not call every unfamiliar function-like name an API.

## 5. Explain Unfamiliar Hardware APIs

For an unfamiliar hardware API, explain only what is needed:

1. Where it comes from.
2. What it does and what its parameters mean.
3. What hardware step it represents in the current project.

Expand further only when the learner asks or when a missing detail blocks understanding.

## 6. Keep Software and Hardware Layers Clear

When relevant, distinguish these layers:

- C language,
- library or API,
- MCU internal hardware,
- board-level circuit,
- external device or computer.

Explain which layer the current code is acting on.

For board-level wiring, let the learner inspect the schematic first. Explain specific components or connections when asked.

## 7. Use Concrete Traces

When a concept is still abstract, use one small concrete example and trace it through the code.

For example, for a string-output function, trace a short value such as `"ABC"` through the relevant buffers, API calls, peripheral, pin, and destination.

Do not add multiple examples unless they improve understanding.

## 8. Explain Syntax Only When It Matters

Do not stop on every variable, operator, type, or keyword.

Explain syntax when:

- it is unfamiliar,
- it changes the meaning of the current code,
- or it blocks understanding of the current block.

Keep the explanation tied to the code being studied.

## 9. End by Returning to the Program

After explaining details, restate the role of the code in the current program.

The learner should be able to answer:

- Why does this code exist?
- When does it run?
- What does it receive or depend on?
- What does it change or produce?
