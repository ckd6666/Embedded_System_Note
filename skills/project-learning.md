# Project Learning Rules

Applies to AI-assisted learning through embedded projects.

## 1. Core Learning Loop

Use this four-step loop:

1. **Run** — Get a small, known-good example working on the target hardware.
2. **Understand** — Identify the program goal, main functional blocks, and the critical path from code to hardware behavior.
3. **Modify** — Change one main factor, predict the result, run it, and compare the result with the prediction.
4. **Rebuild** — Recreate the same small capability from an empty or minimal source file while understanding the essential steps required to make it work.

Do not add extra stages unless required by the current project.

## 2. Understand by Functional Blocks

Explain code by functional block and program purpose, not line by line by default.

Inspect individual lines, syntax, APIs, or implementation details only when they are needed to understand the current block or solve a concrete problem.

When an unfamiliar hardware API appears, answer these five questions:

1. Which module or header does it belong to?
2. What does it do, and what do its parameters mean?
3. What MCU hardware does it control?
4. On the current board, how is that hardware actually connected in the schematic?
5. What step does it perform in the current code-to-hardware path?

## 3. Learn on Demand

When a knowledge gap blocks progress, learn the minimum needed to continue, then return to the project.

Do not expand into related theory unless the current project requires it.

## 4. Verify Through Changes

Prefer small, observable changes.

Change one main factor at a time when possible. If the result differs from the prediction, investigate that difference before moving on.

During Rebuild, module-level copying, reference lookup, and AI-written code are allowed. Do not copy the complete source file. The learner should be able to identify the essential steps needed to recreate the capability from an empty source file.

If rebuilding fails, use the point of failure to identify the next knowledge gap instead of restarting from the beginning.

## 5. Repository Boundaries

Project-specific work, experiments, results, and decisions belong under `projects/`.

Reusable knowledge may be distilled into `knowledge/` when it justifies a dedicated entry.

Do not turn every project observation into a knowledge document.

## 6. AI Guidance

Prefer one useful next action over a long plan.

Do not reveal results that the learner can reasonably predict and verify through the project.
