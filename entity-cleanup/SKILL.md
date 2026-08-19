---
name: entity-cleanup
description: Audit code, modules, files, generated artifacts, or mechanisms and decide whether to keep, warn, archive, or remove them based on real consumers, source-of-truth role, unique value, and maintenance. Use for cleanup and entropy-reduction requests, not ordinary refactoring.
---

# Entity cleanup

Treat cleanup as a decision about responsibility, not a search for files that
look old.

## Workflow

1. Inventory the candidate and find its actual callers, readers, writers, and
   trigger. Use read-only evidence first.
2. Ask whether it has a named consumer and a concrete time when it is used.
   If neither exists, archive or remove it unless retention is required.
3. Classify it as an authority/source of truth or a projection derived from
   another authority.
4. Keep an authority while it still owns a required decision.
5. For a projection, ask whether it adds unique value relative to its source.
   Formatting or duplication alone is not unique value.
6. If the projection adds value, verify an explicit maintenance mechanism:
   generation, synchronization, ownership, validation, or expiry.

## Outcomes

- **Keep:** required authority, or valuable projection with maintenance.
- **Warn:** valuable projection with no reliable maintenance mechanism.
- **Archive:** no active consumer, but historical or audit value remains.
- **Remove:** no consumer, no authority role, and no unique maintained value.

Before removing anything material, resolve the exact targets, inspect callers,
prefer a recoverable operation, and report what changed. Do not infer permission
to delete unrelated or broad directories from a general cleanup request.

## Decision model

```text
Candidate
→ real consumer and trigger?
→ authority or projection?
→ unique value beyond its authority?
→ reliable maintenance mechanism?
→ keep / warn / archive / remove
```
