# Knowledge Document Writing Rules

Rules for creating new documents under `knowledge/`.

These rules define how a knowledge entry should be structured before it is reviewed by `knowledge-document-review.md`.

The purpose of a `knowledge/` entry is:

> **Accurately record one knowledge topic in a compact, reusable, reference-oriented form.**

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
minimum necessary examples
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
4. Add the minimum necessary examples
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
        ↓
10. Run knowledge-document-review.md
```

Do not add sections merely to satisfy a template.

## 4. Default Information Order

When applicable, prefer this order:

```text
Definition
    ↓
Core Rules / Relationships
    ↓
Examples
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

## 8. Minimum Example Set

Examples are optional.

Add an example only when it materially clarifies a rule, relationship, syntax shape, or boundary.

The goal is not:

```text
one example per subsection
```

The goal is:

```text
the smallest set of examples that makes the concept distinguishable
```

For example, explaining declarators may require both:

```c
int value;
int *p;
```

because they demonstrate two different declarator shapes.

Do not add several examples that demonstrate the same relationship.

## 9. Prefer Contrast When a Distinction Is the Core Knowledge

When two concepts are commonly confused, a compact contrast is preferred over two long explanations.

For example:

| Concept | Meaning |
| --- | --- |
| identifier | source-level name |
| object | data-storage entity |

Or:

```text
definition ⊂ declaration
definition ≠ initialization
```

Use tables only when they reduce repeated prose.

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

## 12. Avoid Duplicate Ownership

A concept should have one primary home.

When the same concept appears elsewhere:

- keep only the locally necessary fact,
- link to the primary entry,
- do not maintain parallel full explanations.

This prevents multiple versions of the same knowledge from diverging.

## 13. Default Entry Files

The default entry structure is:

```text
NNN-topic/
├── README.md
├── examples.c
└── exercises.md
```

Rules:

- `README.md` — required.
- `examples.c` — optional; create only when runnable or compiler-observable examples add value.
- `exercises.md` — optional; create only when explicitly useful for the knowledge entry.

Do not create empty optional files.

## 14. Default README Shape

A useful default shape is:

```markdown
# Topic

## Definition

...

## Core Rules

...

## Examples

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
- small tables,
- minimal code,
- stable terminology,
- relative links.

Avoid:

- tutorial narration,
- motivational language,
- repeated summaries,
- unnecessary transitions,
- large code samples when a smaller one proves the same point,
- broad background material.

## 16. Final Step: Mandatory Review

After drafting, apply:

```text
skills/knowledge-document-review.md
```

The writing process creates the smallest complete draft.

The review process must then verify:

```text
correctness
necessity
boundary
duplication
compression
references
```

A knowledge entry is not complete until it passes that review.
