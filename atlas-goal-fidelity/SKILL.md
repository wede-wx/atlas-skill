---
name: atlas-goal-fidelity
description: Use for implementation, build, refactor, optimization, full-stack, and reference-UI tasks where Codex must preserve the user's original goal. Extract a finite Goal Contract from the request and prevent silent goal drift, requirement downgrade, feature hiding, mock substitution, behavior breakage, or reference layout drift. Triggers include implement, build, fix, refactor, optimize, complete, full, end-to-end, preserve, keep, do not change, match this design, reference image, 完整实现, 保留, 不要改, 按参考图.
---

# Atlas Goal Fidelity

Prevent silent changes to the user's goal during execution.

Core rule:

> You may challenge the Goal Contract, but you must not silently modify, narrow, hide, remove, disable, stub, mock, or substitute it.

## Step 1 - Extract The Goal Contract

Before planning or editing, extract a compact Goal Contract:

```json
{
  "goal": "",
  "must_do": [],
  "must_not_do": [],
  "preserve": []
}
```

Each item in `must_do`, `must_not_do`, and `preserve` must include:

- `text`: the constraint in plain language
- `hard`: true when the user used concrete, imperative, complete, exact, or non-negotiable language
- `source_quote`: the closest user phrase that supports the item
- `verify_by`: how to check the item before reporting completion

Keep it finite: no more than 7 combined `must_do` and `must_not_do` items, and no more than 5 `preserve` items.

## Step 2 - Add Default Anti-Drift Rules

Apply these defaults unless the user explicitly says otherwise:

- If the user asks to implement a feature, add `must_not_do`: do not hide, remove, disable, comment out, stub, or mock that feature and call it done.
- If the user asks for complete, full, end-to-end, or backend-included work, add `must_not_do`: do not deliver only a subset without disclosure.
- If the user asks to match a reference UI, add `preserve`: preserve layout structure, element hierarchy, and interaction affordances before style polish.
- If the user asks to refactor, optimize, clean up, or improve, add `preserve`: preserve existing behavior, public APIs, data semantics, and user-visible flows.
- If the user says do not change, keep, preserve, retain, 保留, 不要改, or 沿用, treat the named thing as hard unless clearly hedged.

## Step 3 - Ask Only Blocking Questions

Ask a question only when a missing detail is load-bearing and continuing would likely violate a hard item.

For non-blocking gaps, state an assumption and continue.

## Step 4 - Self-Check Before Risky Actions

Before any action that deletes, disables, narrows scope, weakens tests, swaps layout, changes public behavior, or replaces real behavior with mock data, check:

- Does this violate `must_not_do`?
- Does this change a `preserve` item?
- Does this turn a `must_do` into a weaker substitute?

If yes, do not proceed silently. Use Step 5.

## Step 5 - Disclose Deviations

For hard items, stop and tell the user before acting:

```text
DEVIATION
WHAT: <contract id and item text>
WHY: <specific reason this conflicts with the current path>
IMPACT: <how this changes the original goal>
OPTIONS:
A. Continue with the original goal: <cost or next step>
B. Accept the proposed deviation: <tradeoff>
```

For soft items, disclose the deviation and log the assumption before proceeding.

## Step 6 - Verify Before Reporting Done

Before saying the task is done:

- Check every `must_do` by its `verify_by`.
- Confirm no `must_not_do` happened without deviation disclosure.
- Confirm every `preserve` item still holds.
- State any unmet item as partial, blocked, or intentionally deferred.

Never report success for a subset of the contract without an explicit deviation disclosure.
