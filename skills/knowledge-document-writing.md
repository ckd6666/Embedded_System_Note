# Knowledge Document Writing Rules

Applies to AI-authored documents under `knowledge/`.

## 1. Learning First

The primary goal is to help the reader learn the topic clearly and correctly.

Documents should remain concise, structured, and easy to revisit.

Prioritize content in this order:

1. what the concept is,
2. what it is used for,
3. how it is normally used,
4. examples that make the concept concrete,
5. core rules and relationships required to understand it.

Do not let formal taxonomy, historical details, uncommon features, or secondary technical detail dominate the topic.

## 2. Teach the Concept Before Expanding It

When introducing a concept or a set of related concepts, explain each one before discussing relationships between them.

A concept is not considered explained merely because its name appears in a list or table.

For each new concept, cover the parts that are useful for learning:

- basic meaning,
- primary role or purpose,
- normal usage,
- a concrete example when useful.

Only after the concepts themselves are clear should the document explain necessary relationships between them.

Do not expand into edge cases, pitfalls, failure modes, project-specific concerns, or engineering advice unless the user explicitly asks for them.

If an implementation-dependent property or special rule is part of the concept's core semantics, explain it as part of the concept itself rather than presenting it as a warning or pitfall.

## 3. Keep One Primary Topic

One entry should teach one concept or one tightly coupled concept set.

Include prerequisite or neighboring knowledge only when it is needed to understand the current topic. Otherwise link to its primary entry instead of duplicating the full explanation.

Build the explanation in a useful learning order rather than mirroring the structure of a standard, specification, manual, or source document.

## 4. Explain for Understanding

Use the simplest wording that preserves the correct technical meaning.

Introduce technical terminology when it improves precision, but explain it before relying on it.

Do not replace a clear explanation with specialist vocabulary merely because an authoritative source uses that vocabulary.

Actively use examples when they make a concept easier to understand.

Prefer placing an example close to the concept it explains.

Use as many examples as are useful for learning, provided each example contributes to understanding the current topic.

Prefer concrete examples over abstract restatement.

Do not add background, history, motivation, or broad theory unless it directly helps explain the current topic.

## 5. Use Authoritative Sources for Correctness

Use authoritative sources to establish factual correctness, semantic scope, and accepted terminology.

Source priority:

1. Official standard or specification.
2. Official implementation, platform, or vendor documentation.
3. Recognized engineering standards or institutional guidance.
4. Highly recognized technical references.

Authoritative sources are the factual baseline, not a required writing style or chapter structure.

Do not copy the source's terminology density, taxonomy, or presentation when a simpler explanation preserves the same meaning.

Do not invent technical rules, terminology, notation, or constraints when an established authoritative formulation exists.

Verbatim wording may be used only when the source license, public-domain status, or other applicable permission allows it. Otherwise use accurate paraphrase or a concise quotation and keep the source in `References`.

## 6. Keep Knowledge in Knowledge

A `knowledge/` document should teach the topic itself.

Do not expand the chapter into:

- project decisions,
- architecture choices,
- debugging guidance,
- deployment concerns,
- generic best practices,
- risk catalogs,
- failure-mode collections,
- engineering checklists.

Those concerns belong in the relevant project or operational context unless explicitly requested.

Core language, protocol, hardware, or system semantics still belong in `knowledge/`, including implementation-dependent behavior when understanding it is necessary to understand the concept correctly.

## 7. Keep the Document Focused

Every section, paragraph, table, or example should contribute to at least one of these:

- explaining what the topic is,
- explaining what it is used for,
- showing how it is normally used,
- explaining a core rule or relationship,
- making the concept easier to understand,
- establishing required context,
- supporting later lookup.

Remove or link out content that is merely related, repetitive, too advanced, or outside the topic's teaching responsibility.

Conciseness must not remove explanation that is necessary for learning.

## 8. Content Forms

Use whichever of these best explains the topic:

- concise prose,
- established terminology,
- code or concrete examples,
- operators, symbols, equations, or notation,
- compact tables,
- links and references.

Prefer prose for explanation, examples for understanding, and tables for compact comparison.

When a concept is difficult to grasp from prose alone, add an example instead of making the explanation increasingly abstract or terminology-heavy.

Do not generate flowcharts, process diagrams, teaching diagrams, or illustrative diagrams unless explicitly requested.

Do not use arrows, boxes, indentation, or other notation to simulate a diagram.

Do not create separate example-oriented files such as `examples.c` or `exercises.md` unless explicitly requested.

## 9. Default Document Shape

Use only the sections that help teach the topic.

A common learning-oriented shape is:

```markdown
# Topic

## Overview

## Core Concepts

## Related Knowledge

## References
```

`Overview` should establish what the reader is about to learn.

`Core Concepts` should contain the actual teaching content and may be divided into subsections for each concept.

For a narrow term or rule, `Definition` and `Core Rules` may be more appropriate than `Overview` and `Core Concepts`.

Do not force every document into the same headings.

## 10. Default Layout

Use numbered headings for major sections and hierarchical numbering for subsections when needed.

For major sections, place `---` immediately before and after the `##` heading.

Between `###` subsections, use one `---`.

Do not add separators inside ordinary paragraph groups.
