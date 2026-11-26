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

## Three review seats

The eight gates above say *what* to check. This section says *who* checks, because a check performed by the party being checked is not a check.

| Seat | When | Responsibility | Gates it owns |
| --- | --- | --- | --- |
| Plan review | Before execution | Is the plan complete, and are the acceptance criteria testable? Does the list include at least one counter-example — a check that should fail if the work is wrong? | Task completeness; identity and authorization |
| Result verification | After execution | Re-run every acceptance check and counter-example independently. The actor's own report is input, not evidence. | Result acceptance |
| Release and closure | After verification | Confirm the first two seats left their records, then perform the release itself, observe the result in the live environment, and issue the receipt. | High-impact actions; closure record |

The remaining gates (data handling, tool restrictions, execution safety) are enforced continuously by the runtime rather than by a seat.

All three seats are automated procedures, not roles held by the actor, and every verdict leaves a record that can be looked up. Both plan review and result verification rely on counter-examples: the first confirms the list contains one, the second actually runs it. Work by people and AI agents, and every run of an automation, pass through the same three seats (see [the complete flow at a glance](work-order-lifecycle.md#the-complete-flow-at-a-glance)).

Three properties make the seats worth having:

- **Separation**: no seat is held by the actor whose work it reviews, and a seat does not review another seat's verdict.
- **Release by the releaser**: the seat that approves a release is the one that carries it out, one work order at a time. If approval and release are separate steps performed by different parties, unreviewed work finds its way into the gap between them. Releasing one work order at a time also means one faulty item cannot hold up everything queued behind it.
- **No receipt, no "done"**: until the release seat issues a receipt, nobody may describe the work as complete.

## Gates must be reliable too

A gate is software and process, and it fails like everything else. Design for that.

- **Say what would pass.** When a gate rejects something, it states which evidence would change the verdict. A gate must not demand something that cannot be supplied.
- **Escalate when stuck.** If a gate stops on the same work order repeatedly, or cannot run at all, it notifies a human. Writing the same line to a log every minute is not escalation.
- **Contain bad input.** One malformed work order must not stop the gate from processing the others. Skip it, flag it, continue.
- **Publish the rules.** Every rule a gate enforces is written down where actors can read it before they submit. Rules discovered only by being rejected waste everyone's time and train actors to guess.
- **One rule set.** If two gates check the same thing, they share one definition. Two implementations of one rule will drift apart and contradict each other.

## Using an AI model as a reviewer

A model can read a submission and notice what a fixed rule would miss. It can also be inconsistent, and persuaded. Use it accordingly.

- Pass or fail is decided by deterministic checks. A model may raise **doubts**; it does not hold the verdict.
- Every doubt must be answerable: it names the evidence that would resolve it.
- Bound the number of rounds, and send anything unresolved to a human rather than leaving it blocked.
- Evaluate the reviewer too. Track doubts that were justified and doubts that were not, and adjust its instructions.

## Format checks versus fact checks

It is easy to add checks on how a report is worded — a required header, a mandatory phrase, a citation format. Each one is cheap alone. Together they can cost more effort than verifying the facts, and actors learn to satisfy the format instead of the substance. Prefer checks that verify something true about the work, and review the format checks periodically to remove those that no longer earn their cost.
