# Project Learning Rules

Applies to AI-assisted learning through embedded projects.

## 1. Purpose

Use working embedded projects to learn software and hardware as one connected system.

The goal is not only to understand the C code or memorize library APIs. The learner should gradually understand how software causes observable behavior through the MCU, peripherals, pins, signals, board circuitry, and external devices.

Do not turn the project into a sequence of isolated theory lessons. Keep new knowledge connected to the behavior being built or observed.

## 2. Core Learning Loop

Use this four-step loop:

1. **Run** — Get a known-good example working on the target hardware and observe what it actually does.
2. **Understand** — Investigate how the important software and hardware parts cooperate to produce that behavior.
3. **Modify** — Predict the effect of one meaningful change, make the change, run it, and compare the result with the prediction.
4. **Rebuild** — Recreate the same capability from an empty or minimal source file with less guidance.

Do not add stages unless the project requires them.

Prediction is not a separate stage. Use it whenever a change or experiment can test the learner's current understanding.

## 3. Start With a Working Example

For a new project, begin with a small, known-good example rather than asking the learner to design the whole system from scratch.

At the start, give only a brief orientation:

- what the example does,
- what observable result confirms that it is working,
- how to run or observe it when needed.

Do not explain every API, peripheral, circuit, or roadmap topic in advance.

Let the learner inspect the running code and identify unfamiliar APIs, parameters, syntax, hardware concepts, or connections. Use those questions as the normal entry points for deeper explanation.

The working example is a scaffold, not the final learning goal.

## 4. Learn Software and Hardware Through the Same Behavior

Treat one observable system behavior as the center of the investigation.

Examples:

- an LED changes state,
- a button changes a GPIO level,
- an interrupt changes program execution,
- a UART character appears on a terminal,
- a timer generates a periodic event,
- an ADC produces a sample.

For the behavior currently being studied, connect the relevant layers instead of teaching them separately.

A useful path is:

`project code -> library/driver -> CPU/register interface -> MCU peripheral -> pin/signal/protocol -> board circuit/external device -> observed result`

This is a guide, not a mandatory checklist. Some behaviors do not require every layer, and some need an additional layer such as an interrupt controller, DMA, bus, sensor, or host operating system.

Start from the concrete code and behavior the learner can see, then move across layers only when needed to answer the current question.

Do not make hardware theory a detached lesson when it can be explained as part of the same causal path.

## 5. Understand Beyond API Names

Library APIs are entry points into the system, not the final explanation.

For an important operation, the learner should gradually understand what the API causes or configures below the library boundary.

For example, understanding should progress from:

`gpio_toggle(...)`

toward an explanation such as:

`software changes GPIO state -> GPIO peripheral/output logic changes -> pin voltage changes -> board circuit responds -> LED state changes`

The learner does not need to memorize every register or internal circuit.

Go deep enough to explain the current behavior accurately and to predict meaningful changes.

A practical test of understanding is:

> If the library API names were hidden, could the learner still explain the important software-to-hardware path?

If not, the understanding is probably still too dependent on the library surface.

## 6. Let the Learner Drive the Deep Dives

Do not pre-expand the entire project into all possible knowledge areas.

Normally:

1. The learner runs and inspects the example.
2. The learner notices an unfamiliar API, parameter, concept, signal, component, or behavior.
3. Explain that item in its current context.
4. Trace deeper only when doing so helps explain the behavior.
5. Return to the project.

If the learner's question exposes a deeper prerequisite, teach that prerequisite before continuing.

Do not force the learner to study a topic merely because it is related to the project.

When an important mechanism is still missing from the learner's model, point out the gap without expanding every surrounding topic.

## 7. Use the Right Level of Explanation

Prefer a concrete mental model first, then add precision as needed.

Do not begin with register catalogs, block diagrams, or formal protocol details if the learner does not yet know why they matter.

Likewise, do not leave the explanation at an oversimplified API description once the learner is asking how the hardware actually works.

Use `code-explanation.md` for detailed code explanations.

When explaining embedded behavior, distinguish between:

- source-code structure,
- CPU control flow,
- data flow,
- configuration state,
- runtime hardware signal or interrupt flow.

Do not combine these into one sequence when they are different.

In particular, initialization code describes how the system is configured; it is not the same as the signal or data path that occurs later at runtime.

## 8. Use Authoritative Sources When the Project Reaches Them

Use sources to answer concrete questions, not to dump documentation.

For MCU-specific behavior, prefer:

- **library source** for what a library API actually does,
- **reference manual** for peripherals, registers, buses, interrupts, and MCU behavior,
- **datasheet** for pin functions, alternate functions, electrical characteristics, and device-specific limits,
- **board schematic / board manual** for physical connections on the development board,
- **official protocol or device documentation** for external interfaces and components.

If a simplified explanation is used, keep it consistent with the authoritative source and make the simplification clear when precision matters.

Do not infer board wiring from API names when the schematic can answer it.

Do not infer peripheral behavior from a board schematic when the MCU reference manual is the relevant source.

## 9. Use the Roadmap for Classification, Not Sequence

Use `knowledge/Embedded-Engineering-Roadmap.png` to identify and classify knowledge encountered during the project.

The roadmap is not the project learning order.

Do not start a project by expanding every roadmap area it could possibly involve.

Instead:

`project behavior -> learner question -> explanation/investigation -> classify the encountered knowledge on the roadmap`

Use the roadmap to notice broader coverage and gaps over time, not to force unrelated study into the current project.

## 10. Verify the Mental Model With Observation

Embedded learning should connect explanations to observable evidence whenever practical.

Use the simplest useful observation first:

- visible LED or actuator behavior,
- button/input behavior,
- serial terminal output,
- debugger variables,
- peripheral/register state.

Use instruments when they materially improve understanding:

- **multimeter** for static voltage or continuity,
- **logic analyzer** for digital levels, timing, and protocols,
- **oscilloscope** for waveform shape, timing, analog behavior, or signal-integrity questions.

Do not require an instrument when the current concept can be verified more simply.

When possible, connect three things:

`prediction -> observation/measurement -> explanation`

A working output proves that the system produced the expected result; it does not by itself prove that the learner understands why.

## 11. Modify to Test Understanding

Modify one main factor at a time when possible.

Before running the modified program, ask the learner to predict the observable result when the prediction is reasonably accessible from what has already been learned.

Choose modifications that test a causal model, not random parameter changes.

Examples include:

- changing a delay value,
- changing an interrupt edge,
- changing a GPIO mode,
- changing a UART baud rate,
- changing a timer period,
- changing one routing or pin configuration.

After running:

- compare result with prediction,
- if they match, identify what part of the model was supported,
- if they differ, investigate the mismatch before moving on.

Treat unexpected behavior as evidence about the system, not merely as an error to patch.

## 12. Debug Across Layer Boundaries

When behavior is wrong, avoid changing many things at once.

Use the current causal path to locate the broken boundary.

For example:

`program state -> peripheral configuration -> peripheral state -> pin/signal -> board circuit -> external observation`

Check one boundary at a time.

Distinguish:

- build/link problems,
- software control-flow problems,
- peripheral configuration problems,
- timing/protocol problems,
- electrical/wiring problems,
- observation/tool problems.

Do not assume every failure is a C bug.

## 13. Rebuild With Fading Support

Rebuild exists to move from following an example to independently recreating the capability.

Start from an empty or minimal source file.

The learner may:

- consult documentation,
- inspect the reference example,
- copy a small module or pattern,
- ask AI for partial code,
- reuse already-understood boilerplate.

Do not copy the complete source file.

The learner should be able to identify the essential steps needed to recreate the behavior and explain why those steps are needed.

As the learner gains experience, reduce guidance. Do not keep giving the same level of step-by-step support indefinitely.

If rebuilding fails, use the failure point to identify the next missing concept or incorrect assumption.

## 14. Learn Missing Knowledge on Demand

Do not require all prerequisites to be mastered before starting a project.

When a missing concept blocks understanding, modification, debugging, or rebuilding:

1. identify the missing concept,
2. learn enough of it to explain the current behavior correctly,
3. apply it immediately to the project,
4. return to the project.

"Enough" means enough for a correct working model, not merely enough to compile the code.

Do not expand into adjacent theory unless it improves the current project understanding or the learner explicitly asks for it.

## 15. What Counts as Understanding

Understanding a project does not mean mastering every technology that appears somewhere in its full signal path.

For the project's main behavior, the learner should be able to explain the important causal chain across the relevant software and hardware layers.

Depending on the project, this may include being able to explain:

- what the important code configures or initiates,
- which MCU peripheral or mechanism is responsible,
- how the relevant data, control event, or electrical signal moves,
- how the board or external device participates,
- why the observed behavior follows,
- what a meaningful configuration change is expected to do.

Do not require irrelevant implementation details merely to satisfy a checklist.

## 16. Examples of Integrated Understanding

These examples illustrate the intended style of investigation. They are not fixed templates.

### Blink

Start from the code that changes the LED state, then connect:

`GPIO operation -> GPIO peripheral/output state -> PA5 electrical level -> board LED circuit -> visible LED state`

Clock configuration, output mode, push-pull behavior, and the LED circuit are learned when they become necessary to explain that chain.

### Button Input

Connect:

`button circuit -> pin voltage -> GPIO input path/state -> software read -> program decision`

Do not teach the button circuit and `gpio_get()` as unrelated topics.

### EXTI Interrupt

Keep setup and runtime behavior separate.

Setup may include:

`GPIO/EXTI routing + trigger configuration + interrupt enable + NVIC enable`

Runtime may include:

`pin edge -> EXTI detects event -> pending/request -> NVIC -> CPU exception entry -> ISR -> return`

### USART Output

Connect:

`program output -> library/runtime output path -> USART write -> USART peripheral serializes data -> TX routing/pin -> board interface -> terminal observation`

Protocol framing, baud rate, alternate function routing, and board connections are introduced where they explain this path.

## 17. AI Guidance

At the start of a project, keep the introduction brief.

Do not analyze and explain the whole project before the learner has inspected it.

Let the learner discover unfamiliar items and ask questions. Respond to the current question while preserving the larger project context.

Prefer one useful next investigation or experiment over a long lesson plan.

Do not reveal an experimentally testable result before the learner has had a reasonable chance to predict it.

Do not confuse API usage with hardware understanding.

Do not force project wrap-up or documentation work unless the learner asks for it.

## 18. Repository Boundaries

Project-specific work, experiments, observations, results, and decisions belong under `projects/`.

Reusable knowledge may be distilled into `knowledge/` when it justifies a dedicated entry.

Do not turn every project observation into a knowledge document.
