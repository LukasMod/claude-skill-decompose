# Glossary — Decompose

## Ownership

Component that should directly hold state, subscription, derivation, or effect.

## Mixed concern

One component doing multiple unrelated jobs: render, side effects, state orchestration, prop plumbing, branching.

## Harness / orchestration

App wiring layer. Owns route params, selectors, mutations, effect coordination, screen-level composition, and imperative integration with app APIs.

## Domain logic

Reusable non-UI logic such as derivation, mapping, validation, formatting, and decision rules.

## Presentational component

UI-focused component with a narrow interface. Renders data and emits explicit events without owning broad app wiring.

## Renderless component

Component with no visible UI. Owns side effects or subscriptions only.

## Push-down

Move state, subscription, hook, or logic from parent to narrowest child that uses it.

## Selector narrowing

Subscribe to the smallest possible value, not a broad object or collection.

## Interface tightening

Reduce prop surface. Child derives or subscribes to what it needs.

## Variant split

Split one component into focused variants when branches represent different use cases.

## Structural smell

Architecture sign that tree shape, ownership, or boundaries are wrong.

## Boundary extraction

Creating a new focused component, hook, or helper around one clear responsibility.

## Regression-safe cut

A decomposition step that preserves behavior, keeps interfaces understandable, and can be verified before the next extraction.

## Invariant

Behavior that must not change during decomposition: render states, callback behavior, side effects, navigation, persistence, analytics, and store updates.
