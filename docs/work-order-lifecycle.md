# Work-order lifecycle

> 繁體中文：[zh-TW/work-order-lifecycle.md](zh-TW/work-order-lifecycle.md)

A work order is a human-approved task boundary, not a source of authority for the model. The runtime must verify identity and permissions separately.

## States

`draft → submitted → approved → running → completed`

Before execution starts, a work order can be marked `rejected` or `cancelled`. After execution starts it can move to `failed`, `cancelled` or `needs_review`. Every state transition keeps the time, the operator or service identity, and the reason.

## Flow

1. **Create**: the requester fills in purpose, scope, input data, allowed tools, prohibited actions, acceptance criteria, expiry and owner.
2. **Pre-check**: the system validates required fields, identity, permissions, policy version and risk classification. Execution must not start until this passes.
3. **Approve**: a person with the appropriate responsibility reviews according to risk. Approval is bound to the work-order version; any material change requires re-approval.
4. **Execute**: the runtime provides restricted permissions within the approved scope. Every tool call is verified by the runtime, never accepted on the agent's word alone.
5. **Stop or escalate**: when the work goes out of scope, data is insufficient, policies conflict, risk rises, approval lapses or a safety concern appears, stop side effects and hand over to the owner.
6. **Accept and close**: check results against the predefined criteria; record success, partial completion and failure separately, and never use the agent's own report as the only evidence. Who performs this check, and who releases the result, is described in [quality-gates.md](quality-gates.md#three-review-seats).
7. **Review and improve**: sample receipts and outcomes according to workflow risk; when needed, pause the workflow, revoke permissions or update the policy.

## Approval and revocation

An approval has a scope, an expiry and an approver. It becomes invalid when the work order is modified, when identity or permissions change, when a policy update is incompatible with what was approved, or when the risk assumptions no longer hold. A policy update that does not change what the work order is allowed to do should not strand work in progress: annotate the work order with the new policy version and let it continue (see "Policy updates and work in progress"). After revocation the runtime blocks new actions; actions already in progress follow the predefined stop and recovery procedure.

## Failure handling

A failure must never be presented as a success. Record completed and uncompleted steps, possible side effects, remediation status and the owner. Retries are bounded by count and idempotency; when a safe retry is not possible, hand over to a human.

## Scope triage: when a work order is required

Decide this before anything else, because it determines how much process a request deserves.

- **No work order needed**: reading, searching, answering a question, or proposing a plan that stays in the conversation. These have no external effect, so reviewing them adds delay without reducing risk.
- **Work order required**: anything that uses a tool to change a file, a setting, a deployment or an external system, or that sends something to another party. Writing a plan down as a stored document counts as a change.

Why draw the line here: review effort is limited. Spending it on harmless reads teaches people to treat review as noise; reserving it for actions with consequences keeps it meaningful. When a request is ambiguous, treat it as requiring a work order.

## Automations follow the same lifecycle

A scheduled job or pipeline that changes production is an actor like any other. It should not have a private path around review.

- Give each automation a **pre-approved task list**: what it does, which resources it may touch, and what a normal result looks like. The list is reviewed once and re-reviewed when it changes.
- After each run, compare the result with the task list. A result that matches proceeds to release. A result that does not match — a value outside the expected range, an unexpected resource, a different structure — is an **anomaly**: stop, and hand it to a named coordinator before anything is released.
- An automation must not be able to queue its own output for release.

## No orphan work orders

A work order whose actor has stopped — session closed, credential expired, waiting at a gate with nobody watching — is still a commitment someone made.

- Every open work order has exactly one current owner at all times, and that owner can be looked up.
- Set a deadline measured from plan approval. When a work order stalls or passes its deadline, reassign it to a coordinator, who either hands it to another actor or finishes it. Leaving it is not an option.
- Track the count of open work orders with no reachable owner. The target is zero, and a non-zero count is itself an alert.

## Cancellation leaves a record

Cancelling is a decision, and decisions need evidence. A `cancelled` work order gets a closure record just as a `completed` one does: who cancelled it, why, what had already been done, and what was replaced or abandoned. If the cancellation was caused by a mistake, record that mistake (see [receipts-and-audit.md](receipts-and-audit.md#incident-notes-and-repeat-prevention)). Route cancellations through the same release seat that closes completed work, so that "cancel" cannot become a way to escape review.

## Policy updates and work in progress

Policies change while work is under way. Decide in advance what a policy update does to open work orders, and make the runtime behave that way.

- If the update does not change what a given work order may do, annotate the work order with the new policy version and continue.
- If it does, revoke the approval and require re-approval.
- Never let an update silently invalidate every open approval at once. That turns a routine policy change into an outage of the review process itself.
