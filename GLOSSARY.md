# Glossary — Decompose

## Ownership

Component that should directly hold state, subscription, derivation, or effect.

## Mixed concern

One component doing multiple unrelated jobs: render, side effects, state orchestration, prop plumbing, branching.

## Renderless component

Component with no visible UI. Owns side effects or subscriptions only.

## Push-down

Move state, subscription, hook, or logic from parent to narrowest child that uses it.

## Selector narrowing

Subscribe to smallest possible value, not broad object or collection.

## Interface tightening

Reduce prop surface. Child derives or subscribes to what it needs.

## Variant split

Split one component into focused variants when branches represent different use cases.

## Structural smell

Architecture sign that tree shape, ownership, or boundaries are wrong.
