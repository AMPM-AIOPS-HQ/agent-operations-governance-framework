# Quality gates

> 繁體中文：[zh-TW/quality-gates.md](zh-TW/quality-gates.md)

Quality gates are runtime controls, not advisory text for the agent to read. Each gate should record its result, the policy version and how a failure was handled.

| Gate | Minimum check | On failure |
| --- | --- | --- |
| Task completeness | Purpose, owner, scope, stop conditions, acceptance criteria and deadline are present | Reject the submission |
| Identity and authorization | Requester, approver and executing identity are valid; permissions match the work order | Reject or escalate |
| Data handling | Data source, sensitivity and permitted use are clear; secrets never enter general records | Isolate the data and stop |
| Tool restrictions | Only explicitly allowed tools and operations can be called; side effects and resource use are limited | Block the call |
| High-impact actions | Deletes, external publishing, payments, permission changes and similar actions get human approval per policy | Pause until approved |
| Execution safety | Timeouts, retries, rate limits and abort mechanisms are configured | Terminate or hand to a human |
| Result acceptance | Results are checked against predefined criteria by a check independent of the agent's own report | Mark as not accepted |
| Closure record | A minimal receipt is produced, marking failures, partial completion and approval status | Do not mark as complete |

## Minimum testing

Before a pilot, test at least:

- Missing fields, invalid identities, expired approvals, and old approvals reused after the work order changed.
- Out-of-scope tool requests, rejections, timeouts, duplicate requests and partial failures.
- External content carrying instructions, risk of sensitive-data leakage, and situations that must escalate to a human.
- Human cancellation, permission revocation, retry limits and recovery procedures.

Test results apply only to the explicitly recorded environment, versions, data and scenarios. Simulated tests do not represent production behavior, an error-rate guarantee or a certification.
