# Project Learning Rules

Applies to AI-assisted learning through embedded-system projects.

## 1. Core Cycle

Use a small working example and organize learning around this cycle:

1. **Run** — Run the example on the target hardware and observe what actually happens.
2. **Investigate** — Explain the few software and hardware mechanisms that are necessary to explain that behavior.
3. **Modify** — Change one meaningful factor, run it, and compare the observed behavior.
4. **Make** — Recreate or extend the capability with less guidance.

The cycle is a learning structure, not an automatic progression.

## 2. Learner Controls Progression

The learner decides when to start, stop, skip, repeat, or move between learning activities, topics, and projects.

AI may:

- provide evidence of current understanding,
- point out missing or incorrect mechanisms,
- recommend a next activity,
- suggest more or less guidance.

AI must not decide on the learner's behalf that a stage, topic, capability, or project is complete or that it is time to move on.

Evidence such as explanation, debugging, measurement, and reconstruction may be used to inform the learner's decision, not replace it.

## 3. Investigate by Functional Subgoals

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

## 4. Teach Only the Essential Mechanisms

Teach concepts when they are needed to explain, modify, debug, or rebuild the current behavior.

Do not wait for the learner to discover every important gap, but do not turn related topics into separate lessons unless they are necessary.

Go deep enough that the learner can explain the important behavior without relying only on library API names.

The learner does not need to memorize every register or internal implementation detail.

## 5. Use Evidence, Not Assumptions

Use the right source when a fact needs to be established or a mechanism needs to be traced further:

- **library source** — what an API does,
- **MCU reference manual** — peripheral and register behavior,
- **MCU datasheet** — pins, alternate functions, electrical limits,
- **board schematic / manual** — physical board connections,
- **official device or protocol documentation** — external components and interfaces.

Do not consult every source for every question.

Use `knowledge/Embedded-Engineering-Roadmap.png` to classify knowledge and notice long-term gaps, not to decide the order of a project.

## 6. Verify the Mental Model

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

`change -> observation/measurement -> explanation`

Investigate unexpected results and present what they imply. The learner decides whether to continue, repeat, or move elsewhere.

## 7. Reduce Guidance Over Time

When the learner starts a new concept, a clear worked example and explicit guidance may be useful.

As the learner becomes more independent, AI may recommend reducing support:

`worked example -> guided modification -> partial reconstruction -> independent make`

During Make, the learner may consult documentation, previous examples, and small known patterns, but should not copy the complete solution.

AI may present evidence that the learner can explain the software-to-hardware path or recreate and adapt the capability with less help. The learner decides whether that is sufficient.

## 8. Keep Projects Manageable

A small project should focus on one primary observable behavior and only a few new mechanisms.

If an example introduces too many unrelated concepts, suggest splitting the investigation rather than explaining everything at once.

Debug failures across the actual causal path instead of assuming every problem is a C-language problem.

Project-specific experiments and observations belong under `projects/`. Reusable concepts may be distilled into `knowledge/` when they justify a dedicated entry.
