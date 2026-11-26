# Case study: a website operated day to day by AI agents

> 繁體中文：[zh-TW/case-study.md](zh-TW/case-study.md)

This document describes how the authors of AOGF apply it, and what failed along the way. It covers practices and trade-offs only. It does not contain our rule text, thresholds or internal procedures, and it is not evidence that the approach is complete or certified.

## Background

This framework was not designed and then adopted. It was forced into existence by operations. Our website is run by several AI agents and automated jobs that verify data, write content, review one another's work and publish, every day. People assign the work and make the decisions.

The number and speed of the agents soon exceeded what a person could watch. The question changed from *"can the agent finish the task?"* to *"on what grounds do we believe it finished?"*

## How we operate

**1. Separate work that has consequences from work that does not.**
Reading, searching and proposing are answered directly. Anything that changes a file, a setting or published content first becomes a work order. This keeps review effort on actions that can cause harm, without slowing down simple questions.

**2. Define "done" before starting.**
Each work order states the request in the requester's own words, the deliverables, the acceptance checks, and at least one counter-example: a check that should fail if the work is wrong. An acceptance list without a counter-example tends to confirm that something exists rather than that it is correct.

**3. Three review seats, each independent.**
- *Plan review* checks the plan and the acceptance list before execution; only then is a pass issued.
- *Result verification* re-runs every acceptance check and counter-example from outside the actor. The actor's own report is not accepted as evidence.
- *Release and closure* confirms that the first two seats left their records, performs the release, inspects the live result, and issues a receipt.

Without a receipt, no agent may claim completion.

**4. The control plane is changed only by people.**
Guards, review procedures, security mechanisms and anything involving spending are read-only to agents. When a change is needed, an agent prepares a candidate version and tests; a person applies it and synchronizes every copy. The tools that constrain agents are not adjusted by agents.

**5. Errors are recorded, and records must prevent repetition.**
An agent that is blocked by a gate, or makes an error, writes down what happened, the root cause and the correction at that moment. This is not a letter of apology. Each record should map to a signature a machine can detect, which, once approved by a person, becomes a new check.

## What happened, and what the framework now requires

| What we encountered | What the framework requires as a result |
| --- | --- |
| An agent reported "done" when the deliverable was missing or wrong | Acceptance is independent of the actor and includes a counter-example |
| Several pieces of work were released as one batch; one problem delayed all of them | Release is per work order |
| A review procedure, through its own defect, blocked a valid work order permanently | A gate must state what evidence would pass, and must not demand what cannot be supplied |
| One malformed work order stopped the entire review line | A single bad input must not interrupt review of other work |
| A review procedure logged a stuck item repeatedly while no person was notified | A stalled gate escalates to a human; a log entry is not escalation |
| A release was halted by an automatic safety pause and stayed halted, because nobody was alerted and nobody owned the resume | A safety pause names an owner, raises an alert, and has an approved way to resume |
| A governance rule update invalidated the passes of all work in progress at once | A policy update defines what happens to work in progress |
| Agents spent more effort wording reports to satisfy format checks than verifying facts | Checks favour verifiable facts; format requirements are kept in proportion |
| The same mistake was repeated by different agents that had never seen the earlier record | Incident notes carry a detectable signature and become candidate checks |

## Trade-offs we accepted

- **Slower small changes.** A trivial change still passes three seats. We accept this because the costly failures were never the difficult tasks; they were small changes nobody reviewed.
- **Human time on control changes.** Requiring a person to apply every change to a guard is a bottleneck. We keep it because the alternative is an actor loosening its own checks.
- **More records than feel necessary.** Receipts for failed and cancelled work look like noise until the day they explain an outage.

## Our position

Every row in the table above is something that actually happened to us. Some of them we are still fixing.

We publish them because the hard part of governing agents is not writing rules. It is that the rules themselves fail. A governance framework that does not admit it can be wrong is not one to rely on.
