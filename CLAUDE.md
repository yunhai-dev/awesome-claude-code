Here is the English version of your refined **Development Guidelines**, incorporating the "Plan-on-Demand" strategy, the `docs/plan` structure, and the incremental segment-based implementation.

-----

# Development Guidelines (Final Collaborative Version)

## Philosophy

### Core Beliefs

  - **Tiered Planning**: Small tasks/single-module fixes can be implemented directly; complex tasks involving **multiple modules, cross-components, or architectural changes** must be planned first.
  - **Incremental Progress**: Prohibit full-file overwrites. Use segment-based modifications (Chunks) to ensure each change is within a controllable scope.
  - **Pragmatic Validation**: No complex test suites required. Ensure logical closure through manual verification, log observation, and negative triggering (testing for failure).
  - **Documentation as Index**: Use the `docs` structure for organized thinking, but keep the document lifecycle lightweight.

## Process

### 1\. Task Classification & Planning

Before writing any code, determine the task type:

  * **Simple Task**: Single file changes, local UI adjustments, or simple logic fixes.
      * **Action**: Proceed directly to the [Implementation Flow](https://www.google.com/search?q=%232-implementation-flow).
  * **Complex Task**: Involves multi-module coordination, data structure changes, or integration of new libraries.
      * **Action**:
        1.  Register the feature entry in `docs/IMPLEMENTATION_PLAN.md`.
        2.  Create a detailed 3-5 stage plan in `docs/plan/[feature-name].md`.

**Detailed Plan Template (`docs/plan/xxx.md`):**

```markdown
## Feature: [Name]
**Status**: [In Progress | Complete]

### Stages
1. **Stage 1**: [Specific code block/logic A]
   - **Validation**: [How to verify logic A works]
2. **Stage 2**: [Specific code block/logic B]
   - **Validation**: [How to verify logic B works]
```

### 2\. Implementation Flow

1.  **Segment Identification**: Even for simple tasks, mentally (or via comments) break the task into independent logical segments.
2.  **Segmented Modification**: **Strictly avoid overwriting large chunks of existing files.** Precisely modify only the affected lines, functions, or components.
3.  **Real-time Validation**:
      * **Log Observation**: Run the code and confirm expected log outputs.
      * **Negative Triggering**: Intentionally provide invalid input to ensure "fail-fast" logic works.
4.  **Incremental Commits**: Commit after completing each logical segment to ensure granular and revertible version history.

### 3\. Plan Maintenance (Complex Tasks Only)

  * **During Implementation**: Update the status in `docs/plan/[feature-name].md` in real-time.
  * **Post-Implementation**: Once the feature is stable, summarize the plan into the history of `docs/IMPLEMENTATION_PLAN.md` and archive/delete the original plan file.

## Technical Standards

### Modification Quality

  - **Principle of Locality**: Modifications must be restricted to the smallest necessary scope without touching unrelated code.
  - **Explicit Dependencies**: New logic must keep data flow clear, avoiding obscure global variables or side effects.
  - **Fail Fast**: Validation should confirm not just "success," but also that clear errors occur when something goes wrong.

## Decision Framework

When choosing between multiple approaches:

1.  **Simplicity**: Is this the most direct path to the goal?
2.  **Splittability**: Does this solution allow for small, incremental commits?
3.  **Consistency**: Does it follow the existing patterns in the codebase?
4.  **Necessity**: Is a detailed plan truly required? (If it takes 10 minutes and won't break other modules, skip the plan).

## Quality Gates

### Definition of Done (DoD)

  - [ ] Code was written in precise segments with no redundant changes.
  - [ ] Simple validation (logs, logic checks, exception triggers) passed.
  - [ ] (Complex Tasks) `docs/plan/` status is closed and the index is updated.
  - [ ] Commit messages explain "what" was done and "why."

-----

**NEVER**:

  - Use `--no-verify` to bypass hooks, even for small tasks.
  - Perform full-file replacements; always use segmented modifications.

**ALWAYS**:

  - Choose the "boring" but robust solution over a clever one.
  - **STOP** after 3 failed attempts and document the reason in the plan or code comments before reassessing.
