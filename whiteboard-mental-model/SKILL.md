---
name: whiteboard-mental-model
description: Explain a complex technical system, architecture, state machine, or concept as a causal, repeatable mental model. Distinguish authority, projections, runtime, evidence, and consumers while preserving versions, history, multiplicity, and important failure states.
---

# Whiteboard mental model

Explain the system so the reader can repeat the causal model to another person.
Do not produce a glossary or mirror the source document's table of contents.

## Workflow

1. Read the current authoritative material and the minimum implementation
   evidence needed. Separate documented intent, observed state, and inference.
2. Identify the objects. For each one, state in plain language:
   - what it is;
   - what it owns and does not own;
   - what it reads and who reads it;
   - who wins when facts conflict.
3. Organize the explanation around the real causal path, adapting this chain
   without inventing missing layers:

   ```text
   External facts
   → authority / source of truth
   → compiled projection or manifest
   → runtime execution
   → evidence, lineage, or outcome
   → consumer interpretation
   ```

4. Preserve direction and cardinality: references, constraints, one-to-many,
   many-to-many, versions, and effective periods.
5. Keep “what should happen” separate from “what actually happened.” A log,
   observation, projection, or outcome is not automatically an authority.
6. Use one concrete example throughout. A metaphor may help, but return to the
   actual objects when it stops matching the system.
7. Check zero/one/many matches, unresolved conflicts, current versus historical
   facts, exact version references, missing evidence, drift, and legacy limits.

## Output

1. Lead with the most important relationship in one or two sentences.
2. Show one compact causal chain.
3. Give 6–12 numbered statements the reader can copy and repeat.
4. End with a one-sentence governing idea.
5. List 3–5 long-lived invariants that must not be blurred by simplification.

Use plain language first and formal terms in parentheses. Simplify vocabulary,
not the boundaries that determine the conclusion.
