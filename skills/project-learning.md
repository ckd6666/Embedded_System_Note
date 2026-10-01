# Project Learning Rules

Applies to AI-assisted learning through embedded-system projects.

## 1. Core Cycle

Use a small working example and follow this cycle:

1. **Predict** — When the learner has enough context, predict the important observable behavior.
2. **Run** — Run the example on the target hardware and observe what actually happens.
3. **Investigate** — Explain the few software and hardware mechanisms that are necessary to explain that behavior.
4. **Modify** — Change one meaningful factor, predict the result, run it, and compare.
5. **Make** — Recreate or extend the capability with less guidance.

If the learner does not yet have enough knowledge to make a useful prediction, run first and use prediction during Modify.

## 2. Investigate by Functional Subgoals

Break the example into a few meaningful subgoals rather than explaining it line by line.

For each important behavior, connect only the layers needed to explain it:

`project code -> library/driver -> MCU mechanism -> pin/signal/protocol -> circuit or external device -> observed result`

Do not stop at API names, but do not expand the whole hardware or software stack when it is not needed.

Keep different flows separate when they differ, especially:

- initialization/configuration,
- CPU control flow,
- data flow,
- interrupt flow,
- electrical or protocol signal flow.

Use `code-explanation.md` for detailed code explanations.

## 3. Teach Only the Essential Mechanisms

Teach concepts when they are needed to explain, modify, debug, or rebuild the current behavior.

Do not wait for the learner to discover every important gap, but do not turn related topics into separate lessons unless they are necessary.

Go deep enough that the learner can explain the important behavior without relying only on library API names.

The learner does not need to memorize every register or internal implementation detail.

## 4. Use Evidence, Not Assumptions

Use the right source for the layer being investigated:

- **library source** — what an API does,
- **MCU reference manual** — peripheral and register behavior,
- **MCU datasheet** — pins, alternate functions, electrical limits,
- **board schematic / manual** — physical board connections,
- **official device or protocol documentation** — external components and interfaces.

Use `knowledge/Embedded-Engineering-Roadmap.png` to classify knowledge and notice long-term gaps, not to decide the order of a project.

## 5. Verify the Mental Model

Prefer changes that test a causal explanation.

Change one main factor at a time when possible.

Use the simplest useful observation method:

- visible hardware behavior,
- serial output,
- debugger or register state,
- multimeter,
- logic analyzer,
- oscilloscope.

Use instruments only when they add useful evidence.

When possible, use:

`prediction -> observation/measurement -> explanation`

Investigate unexpected results before moving on.

## 6. Reduce Guidance Over Time

Begin new concepts with a clear worked example and enough guidance to understand the important subgoals.

As competence grows, reduce support:

`worked example -> guided modification -> partial reconstruction -> independent make`

During Make, the learner may consult documentation, previous examples, and small known patterns, but should not copy the complete solution.

A capability is learned when the learner can explain the important software-to-hardware path, predict meaningful changes, and recreate or adapt it with substantially less help.

## 7. Keep Projects Manageable

A small project should focus on one primary observable behavior and only a few new mechanisms.

If an example introduces too many unrelated concepts, split the investigation rather than explaining everything at once.

Debug failures across the actual causal path instead of assuming every problem is a C-language problem.

Project-specific experiments and observations belong under `projects/`. Reusable concepts may be distilled into `knowledge/` when they justify a dedicated entry.
