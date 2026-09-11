# Knowledge Document Writing Rules

Rules for creating new documents under `knowledge/`.

These rules define how a knowledge entry should be structured when it is created.

The purpose of a `knowledge/` entry is:

> **Accurately record one knowledge topic in a compact, reusable, reference-oriented form using authoritative wording and necessary symbols only.**

It is not a tutorial, course chapter, essay, or motivational explanation.

## 1. Primary Principle: Definition First

Start with the concept itself.

Do not begin with:

- background,
- motivation,
- why the topic is important,
- learning objectives,
- historical context,
- general introductions.

Prefer this content order: concept definition, core rules or relationships, then only the symbols or notation required to state them precisely.

The first useful section should make the topic identifiable immediately.

## 2. One Primary Concept

Each entry must have one primary concept or one tightly coupled concept set.

Before writing, determine:

```text
What is the primary concept owned by this entry?
```

Do not allow the entry to grow into neighboring topics merely because they are related.

If another entry owns a concept:

- include only the minimum context required for correctness,
- link to the owning entry,
- do not duplicate its full explanation.

## 3. Core Writing Flow

Use this process when creating a new knowledge document:

1. Determine the primary concept.
2. Write an accurate, direct definition.
3. Extract the core rules and relationships.
4. Add only necessary symbols or notation.
5. Define the concept boundary.
6. Replace out-of-scope detail with links.
7. Place engineering constraints next to the concept they constrain.
8. Add Related Knowledge.
9. Add References.

Do not add sections merely to satisfy a template.

## 4. Default Information Order

When applicable, prefer this order: Definition; Core Rules / Relationships; Related Knowledge; References.

This is an information order, not a mandatory heading structure.

A short entry may need only:

```text
definition
core rule
reference
```

A complex entry may require several concept-specific subsections.

## 5. Definition Rules

A definition should:

- state what the concept is,
- distinguish it from closely related concepts when necessary,
- use standard terminology,
- avoid unnecessary analogy,
- avoid explaining neighboring topics unless required for correctness.

When an informal explanation is useful, keep the formal meaning clear.

Do not replace a precise definition with only an analogy.

## 6. Core Rules and Relationships

After the definition, record only the relationships required to use or distinguish the concept correctly.

Necessary symbolic relationships are allowed when they state the knowledge more precisely or compactly, such as `definition ⊂ declaration`, `identifier ≠ object`, or `declaration specifiers + declarator → complete declared type`.

Prefer compact relationships over repeated prose when both express the same fact.

## 7. Minimum Necessary Context

Related knowledge may be mentioned only when omitting it would make the current explanation incorrect or misleading.

Use this rule:

```text
If omitting the related concept makes the current statement wrong:
    include the minimum necessary explanation + link

Otherwise:
    link only, or omit it
```

Do not pre-teach later chapters.

## 8. Text and Necessary Symbols Only

When writing documents under `knowledge/`, AI must not generate:

- examples,
- sample code,
- worked examples,
- hypothetical cases,
- flowcharts,
- process diagrams,
- teaching diagrams,
- illustrative diagrams.

AI-authored content must consist only of:

- concise factual prose,
- established terminology,
- necessary operators, symbols, equations, syntax fragments, or notation,
- references and links.

Symbols and notation may appear only when they are required to state the knowledge accurately or more compactly. Do not use arrows, boxes, indentation, or other notation to imitate a flowchart.

## 9. Prefer Compact Contrast When a Distinction Is the Core Knowledge

When two concepts must be distinguished, prefer concise prose or compact symbolic relationships over repeated explanation.

For instance, relationships such as `definition ⊂ declaration` or `definition ≠ initialization` are acceptable when they state the distinction precisely.

Use tables only when they reduce repeated prose and contain no invented examples.

## 10. Engineering Constraints Stay Local

Engineering rules should be placed next to the language concept they constrain.

Do not collect unrelated rules into a generic `Best Practices` section.

Place the language rule and any local engineering convention together under the relevant concept rather than repeating the same rule again at the end of the document.

Recognized guidance such as SEI CERT C, MISRA C, BARR-C, compiler documentation, or platform documentation may supplement the language rule.

Always distinguish:

```text
language / specification requirement
```

from:

```text
engineering convention / project rule
```

## 11. Source Hierarchy

Prefer sources in this order:

1. Language standard or official specification.
2. Official implementation or platform documentation.
3. Recognized engineering standards and institutional guidance.
4. Highly recognized technical references or websites.

For C, typical sources include:

- ISO C
- WG14 material
- compiler documentation
- SEI CERT C
- MISRA C
- BARR-C
- cppreference

The document must remain understandable without opening the references.

References exist for verification, traceability, and deeper lookup.

## 11.1 Authority and Wording Rule

The factual content, terminology, and symbols used in a knowledge document should be grounded primarily in highly recognized authoritative sources.

Use authoritative sources as the factual and terminological baseline. Prefer standards, official specifications, official documentation, recognized engineering standards, institutional guidance, and highly recognized technical references or websites.

Established wording may be copied verbatim when the source license, public-domain status, or other applicable permission allows it. Otherwise, preserve the technical meaning with concise quotation or accurate paraphrase and keep the source in References.

Do not invent terminology, notation, rules, or explanatory models when an established authoritative formulation already exists.

## 12. Avoid Duplicate Ownership

A concept should have one primary home.

When the same concept appears elsewhere:

- keep only the locally necessary fact,
- link to the primary entry,
- do not maintain parallel full explanations.

This prevents multiple versions of the same knowledge from diverging.

## 13. Default Entry Files

The default AI-authored entry structure is:

```text
NNN-topic/
└── README.md
```

AI should not create `examples.c`, `exercises.md`, or other example-oriented files as part of normal knowledge-document writing.

## 14. Default README Shape

A useful default shape is:

```markdown
# Topic

## Definition

...

## Core Rules

...

## Related Knowledge

...

## References

...
```

This is not mandatory.

Rules:

- remove empty sections,
- omit sections with no real value,
- introduce additional headings only when the concept requires them,
- do not add a summary merely because a template normally has one.

## 15. Reference-Oriented Writing

Write for long-term lookup.

Prefer:

- direct definitions,
- compact relationships,
- small tables when they reduce prose,
- necessary symbols or syntax fragments,
- stable terminology,
- relative links.

Avoid:

- tutorial narration,
- motivational language,
- repeated summaries,
- unnecessary transitions,
- code samples,
- examples,
- flowcharts or teaching diagrams,
- broad background material.
