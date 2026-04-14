# Development Guidelines

## Core Principles

- **Think Before Coding**: Don't assume. Don't hide confusion. Surface tradeoffs. Before implementing, state assumptions explicitly. If multiple interpretations exist, present them. Simpler approaches first.
- **Simplicity First**: Minimum code that solves the problem. No features beyond what was asked. No abstractions for single-use code. No error handling for impossible scenarios. If you write 200 lines and it could be 50, rewrite it.
- **Surgical Changes**: Touch only what you must. Clean up only your own mess. Don't "improve" adjacent code, comments, or formatting. Remove imports/variables that YOUR changes made unused, but don't remove pre-existing dead code unless asked.
- **Goal-Driven Execution**: Transform tasks into verifiable goals. For multi-step tasks, state a brief plan with verification for each step.

## Task Classification & Planning

| Task Type | Definition | Action |
|-----------|------------|--------|
| **Simple Task** | Single-file changes, local UI adjustments, simple logic fixes | Proceed directly to Implementation Flow |
| **Complex Task** | Multi-module coordination, data structure changes, new library integration, architectural changes | 1. Register a **secondary task list** (just titles/stage names) in `docs/IMPLEMENTATION_PLAN.md`<br>2. Write a **complete detailed design-implementation-test plan** in `docs/plan/[feature-name].md` |

### Document Responsibilities

- **`docs/IMPLEMENTATION_PLAN.md`** – High-level index only. Contains:
  - Feature name
  - Secondary task list (e.g., `1. Database migration`, `2. API layer refactor`, `3. Frontend adaptation`)
  - Status for each task (`[ ]` / `[x]`)
  - Link to detailed plan: `See docs/plan/xxx.md`

- **`docs/plan/[feature-name].md`** – Complete technical design document. Must include the following sections (not limited to 3-5 stages):

```markdown
# [Feature Name] Design Document

## Background & Goals
- Problem to solve
- Success criteria

## High-Level Design
- Modules/components involved
- Data flow or architecture (text description acceptable)

## Implementation Plan

### Stage 1: [Name]
- **Files modified**: `path/to/file1`, `path/to/file2`
- **Specific logic**: Describe functions to add, conditions to change, etc.
- **Validation**: How to manually verify / log observation / negative testing

### Stage 2: [Name]
- ...

### Stage N: [Name]
- ...

## Testing Strategy
- Happy path tests
- Error path tests (negative triggers)
- Regression scope (which existing features need confirmation)

## Risks & Mitigation
- Possible side effects or uncertainties
- Rollback plan
```

> Note: Number of stages depends on actual complexity; no forced 3-5 stages.

## Implementation Flow

1. **Segment Identification**: Break the task into independent logical segments (use comments if helpful).
2. **Segmented Modification**: **Strictly avoid full-file overwrites.** Precisely modify only affected lines, functions, or components.
3. **Real-time Validation**:
   - Run code and confirm expected log output.
   - Intentionally provide invalid input to ensure fail-fast logic works.
4. **Incremental Commits**: Commit after each logical segment to ensure revertible history.

## Quality Requirements

### Modification Quality
- **Principle of Locality**: Changes restricted to the smallest necessary scope.
- **Explicit Dependencies**: New logic keeps data flow clear; avoid implicit globals or side effects.
- **Fail Fast**: Validate not just success, but also clear errors on failure.

### Definition of Done (DoD)
- [ ] Code written in precise segments with no redundant changes.
- [ ] Simple validation (logs, logic checks, exception triggers) passed.
- [ ] (Complex tasks) Corresponding task in `docs/IMPLEMENTATION_PLAN.md` is checked; detailed plan document kept up-to-date.
- [ ] Commit messages explain "what" and "why".

## Prohibited & Required

**NEVER**:
- Use `--no-verify` to bypass hooks, even for small tasks.
- Perform full-file replacements; always use segmented modifications.
- Write code beyond requirements, unnecessary abstractions, or speculative flexibility.

**ALWAYS**:
- Choose the "boring" but robust solution over a clever one.
- **STOP after 3 failed attempts** and document the reason in the plan or code comments before reassessing.
- Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## Decision Framework

When choosing between multiple approaches, evaluate in order:
1. **Simplicity**: Is this the most direct path?
2. **Splittability**: Does it allow small, incremental commits?
3. **Consistency**: Does it follow existing patterns in the codebase?
4. **Necessity**: Is a detailed plan truly required? (If it takes 10 minutes and won't break other modules, skip the plan.)
