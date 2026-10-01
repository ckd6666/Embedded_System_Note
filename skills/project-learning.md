# Project Learning Rules

Applies to AI-assisted learning through embedded projects.

## 1. Core Loop

Use this four-step loop:

1. **Run** — Get a known-good example working on the target hardware.
2. **Understand** — Start from the running code and trace how it produces the observed hardware behavior.
3. **Modify** — Change one main factor, predict the result, run it, and compare.
4. **Rebuild** — Recreate the same capability from an empty or minimal source file.

Do not add extra stages unless the project requires them.

## 2. Understand From the Code Down

Start from the important code that is actually running.

Trace downward only as far as needed to explain the current behavior:

`code -> library/API -> MCU peripheral or register -> pin/signal -> board circuit or external behavior`

Do not separate hardware, protocol, or circuit knowledge into unrelated lessons when it can be learned through this path.

Use `knowledge/Embedded-Engineering-Roadmap.png` to identify and classify the knowledge encountered, not to decide the learning order.

Understanding must go beyond API names. The learner should be able to explain the important path even if the library API names are hidden.

Use the datasheet, reference manual, schematic, and library source when needed.

Use `code-explanation.md` when explaining code.

## 3. Verify and Rebuild

Prefer small, observable changes and change one main factor at a time when possible.

Investigate unexpected results before moving on.

During Rebuild, reference lookup, module-level copying, and AI-written partial code are allowed. Do not copy the complete source file.

## 4. Learn on Demand

When a missing concept blocks understanding or progress, learn enough of it to continue the project, then return to the project.

## 5. Repository Boundaries

Project-specific work belongs under `projects/`.

Reusable knowledge may be distilled into `knowledge/` when it justifies a dedicated entry.
