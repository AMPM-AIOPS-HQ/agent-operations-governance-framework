# Usage guide

> 繁體中文：[zh-TW/usage-guide.md](zh-TW/usage-guide.md)

This guide walks through adopting the framework for one workflow. It assumes you have read the [README](../README.md). Every example here is fictional.

## What you are setting up

Four things, in this order:

1. A rule for **when a work order is required** ([scope triage](work-order-lifecycle.md#scope-triage-when-a-work-order-is-required)).
2. A **work order** format your team fills in ([blank template](../examples/work-order.example.yaml)).
3. **Three review seats** and who or what holds each ([quality gates](quality-gates.md#three-review-seats)).
4. A **receipt** produced at the end of every run ([blank template](../examples/receipt.example.json)).

You do not need new software to start. A shared folder, a checklist and two people who are not the actor are enough for a first pilot.

## Step 1 — Pick one workflow

Choose something that changes an external system but can be undone: opening pull requests, updating documentation, triaging tickets. Avoid starting with anything irreversible or anything that spends money.

*Fictional example: a team lets an agent update the "supported versions" table in its product documentation.*

## Step 2 — Write the triage rule

One sentence is enough: *"Reading and proposing need no work order; anything that edits a published page does."* Post it where actors see it.

## Step 3 — Fill in a work order

Copy the blank template and fill every field. The three fields teams most often leave vague are the ones that matter most:

| Field | Weak | Strong |
| --- | --- | --- |
| `scope.prohibited_actions` | "Don't break anything" | "Do not edit any page other than the versions page; do not change navigation" |
| `acceptance` | "Table is updated" | "Every row matches the release list; a row for a version that does not exist must cause the check to fail" |
| `stop_conditions` | "If something goes wrong" | "Stop if the release list cannot be read, or if more than five rows would change" |

The second row shows a **counter-example**: an acceptance check that is expected to fail when the work is wrong. Without one, a check often proves only that a file exists.

## Step 4 — Assign the three seats

| Seat | In a first pilot this can be |
| --- | --- |
| Plan review | A script that rejects work orders with empty fields, plus a person who reads the acceptance list |
| Result verification | A different person or process that re-runs the acceptance checks |
| Release and closure | The person who publishes the change, looks at the published page, and writes the receipt |

The one rule you may not relax: **the actor does not hold any seat for its own work.**

## Step 5 — Run it once and write the receipt

Run the workflow. Whatever happens, fill in the receipt — including when the run is stopped or rejected. Record what each seat decided and anything that blocked the work.

## Step 6 — Read the receipts before widening scope

After ten or so runs, read the receipts together. Look for:

- Checks that never fail. They may be checking nothing.
- Blocks with the same cause. That cause should become a rule the plan-review seat enforces.
- Runs where the actor reported success and verification disagreed. These are the reason the seats exist.

Only then add a second workflow or a riskier action.

## Common mistakes when adopting

- **Putting the rules in the prompt.** An instruction the runtime does not enforce is a request, not a control.
- **Letting the actor verify itself** "to save time". It saves time until the first false "done".
- **Adding gates without publishing their rules.** Actors then learn the rules by being rejected.
- **Treating the receipt as paperwork.** If nobody reads receipts, stop producing fields nobody uses, and keep the ones that answer real questions.
- **Letting an agent adjust its own controls.** See [the control plane is human-change-only](architecture.md#the-control-plane-is-human-change-only).

## Where to go next

- How we apply this ourselves, and what went wrong: [case study](case-study.md).
- The full list of gates and how to test them: [quality gates](quality-gates.md).
