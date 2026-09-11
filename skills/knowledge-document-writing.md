# Knowledge Document Writing Rules

Applies to AI-authored documents under `knowledge/`.

## 1. Keep One Primary Concept

One entry owns one concept or one tightly coupled concept set.

For out-of-scope knowledge, include only the minimum statement required for correctness and link to its primary entry. Do not duplicate a full explanation.

## 2. Write Definition First

Start directly with the concept definition.

Keep only:

- the accurate definition,
- core rules and relationships,
- necessary engineering constraints,
- necessary related links,
- references.

Do not add background, motivation, learning objectives, historical context, tutorial narration, or a summary unless one of them is required for technical correctness.

Engineering constraints belong next to the concept they constrain. Distinguish specification requirements from engineering conventions.

## 3. Text, Necessary Symbols, and Minimal Examples

AI-authored knowledge content may contain:

- concise factual prose,
- established terminology,
- necessary operators, symbols, equations, or notation,
- compact tables when they reduce repeated prose,
- minimal examples that materially clarify a core rule or distinction,
- links and references.

Examples must be the smallest set necessary to clarify the concept. Do not add several examples that demonstrate the same rule.

Do not generate flowcharts, process diagrams, teaching diagrams, or illustrative diagrams. Do not use arrows, boxes, indentation, or other notation to simulate a diagram.

## 4. Use Authoritative Sources

Use authoritative sources as the factual, terminological, and symbolic baseline.

Source priority:

1. Language standard or official specification.
2. Official implementation or platform documentation.
3. Recognized engineering standards or institutional guidance.
4. Highly recognized technical references or websites.

For C, typical sources include ISO C, WG14 material, compiler documentation, SEI CERT C, MISRA C, BARR-C, and cppreference.

Do not invent terminology, notation, rules, or explanatory models when an established authoritative formulation exists.

Verbatim wording may be used when the source license, public-domain status, or other applicable permission allows it. Otherwise use concise quotation or accurate paraphrase and keep the source in `References`.

## 5. Keep the Entry Minimal

Every sentence or example must materially contribute to at least one of these:

- defining the primary concept,
- stating a core rule or relationship,
- preventing an incorrect interpretation,
- demonstrating a necessary distinction,
- stating a necessary engineering constraint,
- establishing the minimum context required by another rule.

If not, delete it or replace it with a link.

Do not repeat the same conclusion in multiple sections.

## 6. Default README Shape

Use only the sections that have necessary content:

```markdown
# Topic

## Definition

## Core Rules

## Related Knowledge

## References
```

Additional headings are allowed only when the concept requires them.

Do not create separate example-oriented files such as `examples.c` or `exercises.md` unless explicitly requested.
