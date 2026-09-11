# Knowledge Document Writing Rules

Applies to AI-authored documents under `knowledge/`.

## 1. Learning First

The primary goal is to help the reader understand and learn the topic correctly.

Documents should also remain concise, structured, and easy to revisit as references.

Prioritize content in this order:

1. concepts required to understand the topic,
2. common rules and relationships,
3. practical distinctions and failure-prone points,
4. engineering constraints that affect correct use,
5. edge cases, historical details, or rarely used features only when they materially improve understanding or application.

Do not let formal taxonomy, terminology, historical compatibility, or uncommon features dominate a chapter unless they are central to the topic.

## 2. Keep One Primary Topic

One entry should teach one concept or one tightly coupled concept set.

Include prerequisite or neighboring knowledge only when it is needed to understand the current topic. Otherwise link to its primary entry instead of duplicating the full explanation.

Build the explanation in a useful learning order rather than mirroring the structure of a standard, specification, manual, or source document.

## 3. Explain for Understanding

Use the simplest wording that preserves the correct technical meaning.

Introduce technical terminology when it improves precision, but explain it before relying on it. Do not replace a clear explanation with specialist vocabulary merely because an authoritative source uses that vocabulary.

Actively use examples when they make an abstract rule, distinction, or practical consequence easier to understand.

Prefer placing an example close to the concept it explains rather than collecting examples far away from the explanation.

Use as many examples as are useful for learning. Multiple examples are encouraged when they reveal different cases, boundaries, common mistakes, or practical consequences.

Prefer concrete examples over abstract restatement. When useful, include contrasting examples or counterexamples to show why a distinction matters.

Examples should still be relevant to the primary topic and should not become unrelated demonstrations.

Do not add background, history, motivation, or broad theory unless it helps explain the current topic.

## 4. Use Authoritative Sources for Correctness

Use authoritative sources to establish factual correctness, technical boundaries, and accepted terminology.

Source priority:

1. Official standard or specification.
2. Official implementation, platform, or vendor documentation.
3. Recognized engineering standards or institutional guidance.
4. Highly recognized technical references.

Authoritative sources are the factual baseline, not a required writing style or chapter structure.

Do not copy the source's terminology density, taxonomy, or presentation when a simpler explanation preserves the same meaning.

Do not invent technical rules, terminology, notation, or constraints when an established authoritative formulation exists.

Verbatim wording may be used only when the source license, public-domain status, or other applicable permission allows it. Otherwise use accurate paraphrase or a concise quotation and keep the source in `References`.

## 5. Connect Theory to Use

When a concept has an important practical consequence, state it near the concept.

Prioritize distinctions that help the reader:

- interpret code or systems correctly,
- choose between related concepts,
- avoid common misunderstandings,
- recognize implementation-dependent behavior,
- understand when a rule matters in practice.

Distinguish specification requirements from implementation details, engineering conventions, and recommendations.

Do not turn the document into a generic best-practices list.

## 6. Keep the Document Focused

Every section, paragraph, table, or example should contribute to at least one of these:

- understanding the primary topic,
- explaining a core rule or relationship,
- clarifying a necessary distinction,
- preventing a likely misunderstanding,
- showing a practical consequence,
- establishing required context,
- supporting later lookup.

Remove or link out content that is merely related, technically interesting, repetitive, or too advanced for the current topic.

Conciseness must not remove explanation that is necessary for learning.

## 7. Content Forms

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

## 8. Default Document Shape

Use only the sections that help the topic.

A common learning-oriented shape is:

```markdown
# Topic

## Overview

## Core Concepts

## Important Distinctions

## Related Knowledge

## References
```

`Overview` should establish what the reader needs to understand, not provide general background.

`Important Distinctions` is optional. Use it only when the topic has concepts that are easy to confuse or misuse.

For a narrow term or rule, `Definition` and `Core Rules` may be more appropriate than `Overview` and `Core Concepts`.

Do not force every document into the same headings.

## 9. Default Layout

Use numbered headings for major sections and hierarchical numbering for subsections when needed.

For major sections, place `---` immediately before and after the `##` heading.

Between `###` subsections, use one `---`.

Do not add separators inside ordinary paragraph groups.
