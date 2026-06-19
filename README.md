# claude-skill-decompose

A Claude skill for React and React Native decomposition work where the goal is to split mixed concerns into cleaner boundaries **without changing behavior**.

## Files

- `SKILL.md` — main skill instructions
- `GLOSSARY.md` — supporting terminology used by the skill
- `README.md` — overview, usage, and examples

## What it is for

Use this skill when a component or screen feels structurally wrong and needs a cleaner shape, for example:
- a large Expo Router screen mixing routing, store wiring, effects, and rendering
- a React component with broad props and too many branches
- a container that owns data only its children use
- a JSX file hiding reusable logic that should become a hook or helper
- a component tree that re-renders too broadly because ownership is too high

It focuses on:
- component and hook boundaries
- harness/orchestration vs domain logic vs presentation
- ownership of state, selectors, and side effects
- regression-safe extraction order
- simpler interfaces and narrower props
- React Compiler-friendly structure

## What it is not for

Do not use this skill for:
- styling-only cleanup
- one-line bug fixes
- generic performance tuning without a structural problem
- adding memoization as the primary fix
- broad architecture redesign outside the local React tree

## Core approach

The skill works in phases:
1. name the target and invariants
2. map the current tree and ownership
3. classify concerns into harness, domain logic, and presentation
4. identify structural smells
5. design a better target tree
6. choose decomposition patterns
7. propose a regression-safe cut order
8. verify the React shape and output a plan

It prefers small safe cuts over clever rewrites.

## Patterns it favors

- custom hook extraction
- specialized component extraction
- selector narrowing
- data ownership push-down
- interface tightening
- side-effect isolation
- variant splitting
- helper extraction for pure domain logic

## Examples

### Example 1 — Screen decomposition

Use when a screen mixes route params, store selectors, submit handlers, and many UI branches.

Goal:
- keep router/store wiring in the screen harness
- move reusable derivation into hooks/helpers
- extract focused UI sections into separate files

### Example 2 — Presentation split

Use when a child receives a large domain object but only renders three small fields.

Goal:
- narrow props
- remove pass-through plumbing
- make the leaf component purely presentational

### Example 3 — Hook extraction

Use when JSX is cluttered with formatting, derived flags, and event preparation.

Goal:
- move reusable non-UI logic into a focused hook or pure helper
- keep the render path readable
- preserve callback behavior and side effects

## Local installation

Copy this skill directory into your local Claude skills directory, for example:

```bash
cp -R claude-skill-decompose ~/.claude/skills/decompose
```

Then invoke it from Claude Code with:

```text
/decompose
```

## Companion skills

This skill can work well alongside:
- tree discovery / component inventory
- regression or UI invariant checking
- focused hook extraction

But it should still stand on its own and not depend on those skills existing.
