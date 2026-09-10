# Projects

This module contains independent embedded systems projects and experiments.

## Principles

1. One project, one directory.
2. Name project directories as `NNN-name` in creation order, for example `001-blink`. Do not renumber existing projects.
3. Each project is self-contained. Its code, configuration, and project-specific records stay inside its own directory.
4. Changes in `projects/` must not modify other top-level modules unless explicitly requested.
5. Decide each project's internal structure only when needed. Do not add structure preemptively.
