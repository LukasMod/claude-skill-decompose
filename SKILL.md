---
name: decompose
description: Decompose React or React Native component architecture for cleaner boundaries, less mixed logic, fewer effects, and fewer re-renders.
---

# Decompose

Use when user wants to improve React or React Native component structure, not patch symptom.

Leading idea: split mixed logic into smaller components and custom hooks with clean ownership, explicit interfaces, and Rules of React-friendly structure. Prefer simple boundaries that follow SOLID over clever abstractions. Good JSX structure gives good performance.

## Steps

1. Name target.
   Restate component or screen, what feels wrong, and what must stay same.
   Completion criterion: target and invariant clear.

2. Read current tree.
   Identify:
   - component boundaries
   - hooks at each level
   - props passed through
   - state location
   - effects location
   - context usage
   - subscriptions or selectors
   - similar local patterns worth matching or intentionally improving
     Completion criterion: current ownership map clear.

3. Find mixed concerns.
   Look for:
   - side effects mixed with render logic
   - parent owning data only children use
   - broad subscriptions high in tree
   - derived state stored instead of derived in render
   - sync Effects
   - giant components with unrelated branches
   - prop drilling through neutral layers
   - controlled inputs on hot path
     Completion criterion: each structural smell named.

4. Design ideal tree.
   From first principles, define cleaner component boundaries. Existing names not sacred.
   Each component should:
   - have one clear job
   - own exactly data it needs
   - expose minimal interface
   - narrow with simple props
   - avoid work for branches not active
   - live in a separate file when it is a meaningful decomposed boundary
     Completion criterion: target tree simpler than current tree.

5. Choose decomposition pattern.
   Prefer one or more:
   - abstract logic into custom hooks
   - specialized component extraction
   - data ownership push-down
   - selector narrowing
   - interface tightening
   - shared component extraction
   - variant split by use case
   - side-effect isolation via renderless component when a hook or focused component is not enough
   - specialized component swap like FlatList or FlashList
     Completion criterion: chosen pattern matches actual smell.

6. Verify React shape.
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
     Completion criterion: no structural fix replaced by memo band-aid.

7. Check Compiler friendliness.
   Assume React Compiler should do most memo work if code follows Rules of React.
   Prefer code that compiler can optimize naturally:
   - pure render logic
   - early returns OK
   - calculations inside component OK when tied to props or state
   - if React Compiler works, do not add manual memoization
   - do not cargo-cult move every function or object out of render
   - inline functions or objects are problem only when they defeat memoized child boundaries or measured hot paths
     Completion criterion: plan avoids unnecessary manual memoization and only flags unstable refs when they matter.

8. Emit architecture plan.
   Output:
   - current state
   - target state
   - proposed component tree
   - ownership moves
   - expected rerender reduction
   - risks
   - optional implementation order
     Completion criterion: user can see why new tree better.

## Heuristics

- Never reach for local band-aid if tree reshape solves root cause.
- Big parent with many hooks usually too high-level.
- If child only receives data to pass farther, ownership wrong.
- If effect exists only to sync state to props or other state, shape wrong.
- If one component has multiple unrelated branches, split variants.
- If list built with ScrollView and many items, use specialized list.
- If broad store or context change hits unrelated leaves, narrow ownership.
- If component name vague, boundary probably vague too.
- If a new boundary has a real name and single job, give it its own file.
- If a child does not need the full object, narrow with simple props.
- Prefer a custom hook before inventing a clever component API.
- Check nearby code for patterns worth matching before introducing a new abstraction.

## React Native focus

Watch extra hard for:

- TextInput controlled on fast path
- JS-heavy work near interaction
- list virtualization missing
- app-wide or screen-wide subscriptions
- expensive headers, footers, side panels hanging off changing parent

## Output style

Lead with architecture, validate with render consequences.
Say what owns too much now, what should own it instead, and which components stop re-rendering after move.

See [GLOSSARY.md](GLOSSARY.md).

