# Knowledge Document Review Rules

Rules for writing and reviewing documents under `knowledge/`.

These rules are mandatory. Apply them while drafting and again before considering a knowledge document complete.

## 1. Primary Rule: Deletion Test

For every paragraph and every sentence, ask:

> **If this paragraph or sentence were deleted, could the reader still correctly learn the core knowledge of this chapter?**

If the answer is **yes**, the content should usually be:

- deleted,
- merged into a shorter explanation,
- moved to a more appropriate chapter, or
- converted into a concise reference/link.

The goal of a `knowledge/` document is not to be comprehensive for its own sake.

The goal is:

> **Use the minimum amount of text required to build a complete and correct mental model.**

## 2. A Sentence or Paragraph May Remain Only If It Serves a Necessary Function

Content should normally remain only when it does at least one of the following:

1. Defines a core concept.
2. Explains a core mechanism or relationship.
3. Prevents a likely or dangerous misunderstanding.
4. Provides the minimum example necessary to understand the concept.
5. States an engineering constraint that materially affects correct usage.
6. Establishes prerequisite knowledge required by a later chapter.

If it serves none of these purposes, remove it.

## 3. Four Mandatory Review Tests

Every section must pass all four tests.

### 3.1 Correctness Test

Ask:

> Is this technically accurate?

Requirements:

- Prefer the language standard or another authoritative specification as the semantic baseline.
- Distinguish language rules from engineering conventions.
- Do not simplify a rule in a way that creates a false mental model.
- If an important rule depends on scope, storage duration, linkage, implementation, standard version, or other context, state that context.

### 3.2 Necessity Test

Ask:

> If this content is removed, is the reader's understanding damaged?

Remove:

- repeated conclusions,
- motivational filler,
- statements that merely say a concept is "important",
- multiple examples that demonstrate exactly the same thing,
- background knowledge not required by the chapter.

Keep only the smallest amount needed for correct understanding.

### 3.3 Boundary Test

Ask:

> Does this knowledge actually belong in this chapter?

If another module or chapter owns the concept:

- explain only the minimum prerequisite needed here,
- link to the owning chapter,
- do not duplicate the full explanation.

A chapter should have one clear responsibility.

### 3.4 Compression Test

Ask:

> Can this be expressed more directly without losing precision?

Prefer:

- one precise sentence over three similar sentences,
- one decisive example over several redundant examples,
- a small table when it replaces repetitive prose,
- a simple diagram when relationships are otherwise difficult to express.

Compression must never sacrifice correctness.

## 4. Example Selection Rule

Examples exist to reduce cognitive load, not to make the document look complete.

Before adding an example, ask:

> What misunderstanding does this example eliminate?

If there is no clear answer, do not add it.

When several examples teach the same idea, keep the smallest or clearest one.

Add another example only when it introduces a genuinely different case.

For example:

```c
int value;
int *p;
```

may both be necessary when explaining declarators because they demonstrate two different declaration shapes.

Three additional pointer examples that demonstrate the same relationship probably are not necessary.

## 5. Distinguish Core Knowledge from Detail

Use three levels consciously.

### Core

The reader must understand this to understand the chapter.

Keep and explain clearly.

### Supporting

The reader needs this to avoid a likely misconception or connect the core concepts.

Keep concise.

### Detail

The information is true but not required for the chapter's learning objective.

Usually remove it or link to the chapter that owns it.

Do not keep detail merely because it is technically interesting.

## 6. Avoid Premature Knowledge

Do not introduce advanced concepts merely because they are related.

A brief preview is acceptable only when required to prevent an incorrect model.

For example, an introductory declaration chapter may show:

```c
int *p;
```

to demonstrate that a declarator can contain `*`.

It should not teach pointer arithmetic, pointer lifetime, or complex function-pointer syntax there.

## 7. Engineering Rules Must Stay Near the Concept They Constrain

Do not create a separate collection of generic "best practices" when a rule belongs to a specific concept.

Prefer:

```text
initialization
└── explain the initialization rule and its failure mode here
```

rather than:

```text
language knowledge/
best-practices/
```

Mature guidance such as SEI CERT C, MISRA C, and BARR-C should be integrated next to the relevant language concept and referenced there.

## 8. Avoid Duplicate Explanations

A concept should have one primary home.

Elsewhere:

- summarize only what is required locally,
- use a relative link to the primary explanation.

Do not maintain multiple full explanations of the same concept.

## 9. Required Final Review Process

Before completing or committing a `knowledge/` document, review it in this order:

```text
1. Correctness
   ↓
2. Deletion test, sentence by sentence
   ↓
3. Chapter-boundary check
   ↓
4. Remove duplicate explanations
   ↓
5. Remove redundant examples
   ↓
6. Compress remaining wording
   ↓
7. Verify links and references
```

Then perform one final question:

> **Is there anything left in this document that is true but not necessary for correctly learning or later looking up this chapter?**

If yes, remove or relocate it.

## 10. Definition of a Good Knowledge Document

A good `knowledge/` document is not the longest or most comprehensive document.

It should be:

- technically correct,
- conceptually complete,
- minimal,
- easy to scan,
- easy to revisit during engineering work,
- explicit about important failure modes,
- free of unrelated detail.

The target is:

> **No missing core knowledge. No unnecessary knowledge.**
