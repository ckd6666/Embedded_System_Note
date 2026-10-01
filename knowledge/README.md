# Knowledge

This module contains independent, reusable embedded-systems knowledge organized by subject.

## 1. Organization

Organize knowledge by subject.

Use `knowledge/Embedded-Engineering-Roadmap.png` as a classification reference, not as a fixed directory tree.

Create directories only when they are needed.

Use:

- conventional names for established language or technology names when clearer, such as `C`;
- lowercase kebab-case for ordinary subject and subtopic directories, such as `microcontrollers`, `gpio`, and `clock-management`.

Use nested directories when a topic naturally belongs under a broader subject.

Example:

```text
knowledge/
├── C/
└── microcontrollers/
    ├── gpio/
    ├── interrupts/
    └── timers/
```

Each knowledge entry is a directory named `NNN-name`, using the next available number within its immediate subject directory.

Existing entries are never renumbered.

## 2. Writing Knowledge

Before creating or substantially rewriting a knowledge entry, follow:

`skills/knowledge-document-writing.md`

This skill defines how knowledge content should be taught, structured, scoped, and supported by sources.

## 3. Reviewing Knowledge

When reviewing or correcting an existing knowledge entry, follow:

`skills/knowledge-document-review.md`

Use the review skill to check correctness, scope, clarity, structure, and consistency with the knowledge-writing rules.

## 4. Scope

Knowledge entries should contain reusable knowledge, not project-specific experiments, decisions, or debugging history.

Changes under `knowledge/` must not modify other top-level modules unless explicitly requested.
