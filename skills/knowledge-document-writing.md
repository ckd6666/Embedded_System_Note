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

Prefer:

```text
concept
    ↓
accurate definition
    ↓
core rules / relationships
    ↓
necessary symbols / notation
```

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

```text
1. Determine the primary concept
        ↓
2. Write an accurate, direct definition
        ↓
3. Extract the core rules and relationships
        ↓
4. Add only necessary symbols or notation
        ↓
5. Define the concept boundary
        ↓
6. Replace out-of-scope detail with links
        ↓
7. Place engineering constraints next to the concept they constrain
        ↓
8. Add Related Knowledge
        ↓
9. Add References
```

Do not add sections merely to satisfy a template.

## 4. Default Information Order

When applicable, prefer this order:

```text
Definition
    ↓
Core Rules / Relationships
    ↓
Related Knowledge
    ↓
References
```

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

Examples:

```text
definition ⊂ declaration
```

```text
identifier ≠ object
```

```text
declaration specifiers + declarator
→ determine the complete declared type
```

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

## 8. No Examples or Diagrams

When writing documents under `knowledge/`, do not generate:

- examples,
- sample code,
- worked examples,
- flowcharts,
- process diagrams,
- teaching diagrams.

Use only:

- concise prose,
- necessary terminology,
- necessary operators, symbols, equations, syntax fragments, or notation.

Symbols and notation should appear only when they are required to state the knowledge accurately or compactly.

## 9. Prefer Compact Contrast When a Distinction Is the Core Knowledge

When two concepts must be distinguished, prefer concise prose or compact symbolic relationships over repeated explanation.

For instance, relationships such as `definition ⊂ declaration` or `definition ≠ initialization` are acceptable when they state the distinction precisely.

Use tables only when they reduce repeated prose and contain no invented examples.

## 10. Engineering Constraints Stay Local

Engineering rules should be placed next to the language concept they constrain.

Do not collect unrelated rules into a generic `Best Practices` section.

Prefer:

```text
reserved identifiers
    ↓
language rule
    ↓
local engineering convention
```

rather than repeating the same rule again at the end of the document.

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

```text
language standard / official specification
        ↓
official implementation or platform documentation
        ↓
recognized engineering standards and guidance
        ↓
high-quality technical references
```

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

Prefer:

```text
standard / official specification
        ↓
official implementation or platform documentation
        ↓
recognized engineering standard or institutional guidance
        ↓
high-quality technical reference
```

When authoritative wording is already clear and compact, preserve its technical meaning closely.

Short quotations or minor wording adjustments are acceptable when appropriate, but do not reproduce long copyrighted passages verbatim. Prefer concise quotation, close technical paraphrase, or reorganization while preserving the authoritative meaning.

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
