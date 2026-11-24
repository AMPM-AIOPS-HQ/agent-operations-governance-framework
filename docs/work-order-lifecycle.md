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
6. **Accept and close**: check results against the predefined criteria; record success, partial completion and failure separately, and never use the agent's own report as the only evidence.
7. **Review and improve**: sample receipts and outcomes according to workflow risk; when needed, pause the workflow, revoke permissions or update the policy.

## Approval and revocation

An approval has a scope, an expiry and an approver. It becomes invalid when the work order is modified, when identity or permissions change, when the policy version is incompatible, or when the risk assumptions no longer hold. After revocation the runtime blocks new actions; actions already in progress follow the predefined stop and recovery procedure.

## Failure handling

A failure must never be presented as a success. Record completed and uncompleted steps, possible side effects, remediation status and the owner. Retries are bounded by count and idempotency; when a safe retry is not possible, hand over to a human.
