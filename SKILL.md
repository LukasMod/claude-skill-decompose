---
name: decompose
description: Decompose React or React Native component architecture into cleaner boundaries with explicit ownership, harness-vs-logic separation, and regression-safe cuts.
---

# Decompose

Use when user wants to improve React or React Native component structure, not patch a symptom. This skill is for splitting mixed concerns into focused components, custom hooks, selectors, and helpers **without changing behavior**.

Leading idea: good React architecture comes from clear ownership, explicit interfaces, and simple boundaries. Separate harness/orchestration from reusable domain logic and presentation. Prefer a safe sequence of small cuts over a clever rewrite. Good JSX structure gives good performance.

You work in phases and do not skip ahead.

## Phase 1 — Name target and invariants

Restate:
- the target screen/component/hook boundary
- what feels wrong structurally
- what must stay the same
- how deep this session should go

Capture invariants before suggesting a split:
- rendered behavior
- public props and callback behavior
- navigation and side-effect behavior
- loading, empty, error, and disabled states
- analytics, persistence, store writes, and subscriptions that must still fire

Depth options:
- Level 0 — target only
- Level 1 — target + direct children
- Full local subtree

Completion criterion: target, depth, and no-regression invariants are explicit.

## Phase 2 — Read the current tree

Identify:
- component boundaries
- hooks at each level
- props passed through
- state location
- effects location
- context usage
- subscriptions or selectors
- derived values
- similar local patterns worth matching or intentionally improving

Map current ownership clearly:
- what owns data
- what only passes data through
- what triggers side effects
- what is reusable logic versus app wiring

Completion criterion: current ownership map is clear enough to explain the tree to another engineer.

## Phase 3 — Classify each concern

For each meaningful block of code, decide whether it belongs to:

### Harness / orchestration
Owns app wiring such as:
- route params
- store subscriptions and selectors
- mutations and action dispatch
- effect coordination
- screen-level composition
- feature gating
- imperative integration with app/runtime APIs

### Domain logic
Reusable non-UI logic such as:
- derivation
- mapping
- formatting
- validation
- policy or decision logic
- state-machine-like behavior
- reusable hook logic that is not tied to one screen shell

### Presentation
Focused rendering such as:
- visual structure
- styling
- copy display
- narrow event surfaces
- local UI-only state

Rules:
- do not bury harness inside generic presentational components
- do not leave reusable domain logic trapped inside large JSX files
- do not invent fake reuse by extracting app-specific orchestration into “shared” utilities

Completion criterion: each concern has an appropriate layer.

## Phase 4 — Find structural smells

Look for:
- side effects mixed with render logic
- parent owning data only children use
- broad subscriptions high in tree
- derived state stored instead of derived in render
- sync Effects
- giant components with unrelated branches
- prop drilling through neutral layers
- controlled inputs on hot path
- harness mixed with domain logic and presentation in one file
- helpers that secretly depend on screen wiring
- generic components that know too much about app state

Completion criterion: each structural smell is named and tied to a concrete location.

## Phase 5 — Design the target tree

From first principles, define cleaner boundaries. Existing names are inputs, not constraints.

Each target boundary should:
- have one clear job
- own exactly the data it needs
- expose a minimal interface
- accept narrow, simple props
- keep harness visible at the right level
- keep reusable logic out of screen shells when possible
- avoid work for inactive branches
- live in a separate file when it is a meaningful decomposed boundary

Prefer target trees where:
- presentation is easy to read without tracing app wiring
- hooks encapsulate reusable behavior cleanly
- selectors are as narrow as practical
- side effects live near the narrowest owner that truly needs them

Completion criterion: target tree is simpler, clearer, and safer than the current tree.

## Phase 6 — Choose decomposition patterns

Prefer one or more patterns based on the actual smell:
- custom hook extraction
- specialized component extraction
- data ownership push-down
- selector narrowing
- interface tightening
- shared component extraction
- variant split by use case
- side-effect isolation via renderless component when a hook or focused component is not enough
- specialized component swap like FlatList or FlashList
- helper extraction for pure domain logic

Pattern rules:
- prefer a custom hook before inventing a clever component API
- prefer specialized leaf components over broad configurable abstractions
- prefer extracting pure logic before rearranging many UI nodes
- only create a shared abstraction when at least the interface is genuinely shared

Completion criterion: each chosen pattern directly answers a named smell.

## Phase 7 — Choose a regression-safe cut order

Do not propose a giant rewrite by default. Prefer a cut sequence that preserves behavior after each step.

Typical order:
1. extract pure helpers or derivations
2. extract presentational leaves with narrow props
3. relocate hooks or logic to the narrowest real owner
4. isolate side effects
5. split variants or larger subtree boundaries
6. tighten interfaces and delete old pass-through paths

For each cut, state:
- what moves
- what stays put
- which interface changes
- why the cut is behavior-safe
- what should be checked afterward

Completion criterion: implementation order is incremental and safe.

## Phase 8 — Verify React shape

Prefer:
- derive in render, not Effect
- less lifted state
- IDs over duplicated objects
- flatter state
- children/composition before prop drilling
- context only when many distant consumers truly need same value
- early returns to cut inactive branches fast
- small explicit interfaces between components and hooks
- if React Compiler works, do not use memo, useMemo, or useCallback

Avoid:
- memo band-aids for bad ownership
- extracting components only to reduce file length
- hiding mutations or selectors inside “dumb” UI components
- moving logic into hooks that still leak broad screen concerns

Completion criterion: structural fix is real, not disguised optimization.

## Phase 9 — Check Compiler friendliness

Assume React Compiler should do most memo work if code follows Rules of React.
Prefer code that compiler can optimize naturally:
- pure render logic
- early returns OK
- calculations inside component OK when tied to props or state
- if React Compiler works, do not add manual memoization
- do not cargo-cult move every function or object out of render
- inline functions or objects are a problem only when they defeat memoized child boundaries or measured hot paths

Completion criterion: plan avoids unnecessary manual memoization and only flags unstable refs when they matter.

## Phase 10 — Emit the decomposition plan

Output:
- current state
- invariants that must hold
- target state
- proposed component tree
- layer classification summary
- ownership moves
- chosen decomposition depth
- recommended cut order
- expected rerender reduction or ownership simplification
- risks
- verification checklist
- optional implementation order by file

When useful, include:
- ASCII tree
- before/after ownership notes
- prop-surface before/after
- hook extraction candidates

Completion criterion: user can see why the new tree is better and how to get there safely.

## Heuristics

- Never reach for a local band-aid if tree reshape solves the root cause.
- Big parent with many hooks is usually too high-level.
- If child only receives data to pass farther, ownership is probably wrong.
- If an effect exists only to sync state to props or other state, shape is probably wrong.
- If one component has multiple unrelated branches, split variants.
- If list is built with ScrollView and many items, use a specialized list.
- If broad store or context change hits unrelated leaves, narrow ownership.
- If component name is vague, boundary is probably vague.
- If a new boundary has a real name and single job, give it its own file.
- If a child does not need the full object, narrow with simple props.
- Check nearby code for patterns worth matching before introducing a new abstraction.
- Keep harness thin but visible; do not pretend app wiring is presentation.
- Prefer extracting stable seams first: pure helpers, pure UI, then orchestration shifts.
- A decomposition is not done until critical states and side effects are accounted for.

## React Native focus

Watch extra hard for:
- TextInput controlled on fast path
- JS-heavy work near interaction
- list virtualization missing
- app-wide or screen-wide subscriptions
- expensive headers, footers, side panels hanging off changing parent
- screen components that mix router params, storage, and rendering in one file

## Output style

Lead with architecture, validate with render consequences.
Say what owns too much now, what should own it instead, what belongs to harness versus reusable logic, and which components stop re-rendering after the move.

See [GLOSSARY.md](GLOSSARY.md).
