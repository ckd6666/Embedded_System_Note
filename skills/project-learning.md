# Project Learning Rules

Applies to AI-assisted learning through embedded projects.

The goal is to learn by making real code run on real hardware, then understanding and extending it with the minimum necessary theory.

## 1. Core Learning Loop

Use this four-step loop:

1. **Run** — Get a small, known-good example working on the target hardware.
2. **Understand** — Identify the program goal, its main functional blocks, and the critical path from code to hardware behavior.
3. **Modify** — Change one main factor at a time, predict the result, run it, and compare the observed behavior with the prediction.
4. **Rebuild** — Reimplement the same small capability with reduced dependence on the original example.

Do not add extra learning stages unless they are necessary for the current project.

## 2. Read by Functional Blocks

Explain code by functional block and program purpose, not line by line by default.

Start with questions such as:

- What is this program trying to do?
- Which parts are essential to that behavior?
- How does execution reach the final hardware effect?

Only inspect individual lines or language details when they are required to understand the current block or solve a concrete problem.

Do not interrupt the main learning path to explain every unfamiliar keyword, macro, API, or implementation detail.

## 3. Learn Only What the Project Needs

When a knowledge gap blocks progress, learn the smallest amount needed to understand or solve the current problem, then return to the project immediately.

Do not expand into related theory merely because it is nearby or interesting.

Use the project to decide when deeper knowledge is needed.

## 4. Modify with a Clear Cause and Effect

Before a modification, make a short prediction about the expected behavior.

Change one main factor at a time whenever possible so the result has a clear cause.

After the change:

- build and run it,
- observe the real result,
- compare it with the prediction,
- investigate only if the result is unexpected.

Prefer small, observable modifications over large rewrites.

## 5. Rebuild to Check Understanding

Before considering a small project or capability learned, attempt to rebuild it without copying the original solution directly.

Documentation, datasheets, API references, `knowledge/`, and earlier notes may be consulted when needed.

A failure to rebuild is not a reason to restart from the beginning. Use the exact point of difficulty to identify the next knowledge gap.

## 6. Keep Project and Knowledge Responsibilities Separate

Project-specific work, experiments, results, and decisions belong under `projects/`.

Knowledge that is reusable beyond the current project may be distilled into `knowledge/` after it has become useful enough to justify a dedicated entry.

Do not turn every project observation into a knowledge document.

## 7. AI Teaching Behavior

When guiding a project, the AI should:

- keep the current goal visible,
- prefer one useful next action over a long plan,
- explain only what is needed for the current step,
- avoid default line-by-line code narration,
- avoid unnecessary theory detours,
- let the learner predict and test instead of revealing every result in advance,
- increase independence gradually as understanding improves.

The default pattern is:

> **Run → Understand → Modify → Rebuild**

If something blocks progress:

> **Learn the minimum needed → return to the project.**
