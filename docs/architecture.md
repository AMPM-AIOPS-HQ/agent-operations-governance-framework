# Architecture and governance principles

> 繁體中文：[zh-TW/architecture.md](zh-TW/architecture.md)

## Purpose

AOGF treats an agent workflow as an operational process with a clear owner, authorization boundaries, risk controls and a record of outcomes. Governance must match the actual runtime environment; it cannot rely on prompt text or on the model's own statements.

## Logical components

1. **Requester and owner**: states the goal, confirms the purpose, and is accountable for approval and for handling the outcome.
2. **Work order**: describes the task, scope, tool permissions, data restrictions, acceptance criteria and stop conditions.
3. **Policy and quality gates**: check authorization, risk and output before, during or after execution.
4. **Agent and controlled tools**: act only within granted permissions; the runtime should technically restrict which tools and data are available.
5. **Human reviewer**: approves or rejects actions that are high-impact, irreversible or outside the pre-authorized scope.
6. **Receipts and audit records**: capture the minimum information needed to reproduce what happened, and support investigation, correction and review.

These are logical responsibilities; they do not have to be deployed as separate services. The controls can be implemented with a workflow engine, a permission system, policy checks and storage.

## Trust boundaries

- User input, external files, tool responses and model output are all treated as untrusted data.
- A prompt, a work order or model output cannot grant itself additional permissions.
- Credentials, personal data and confidential content belong in dedicated secure storage, not in general logs or receipts.
- Tool side effects are controlled with explicit permissions, approvals and verifiable results; a written record after the fact is not enough to stop an unauthorized action.

## Core principles

- **Least privilege**: grant only the tools, data and time window the task needs.
- **Explicit authorization**: write what is allowed and what is prohibited into the work order, and have the runtime enforce it.
- **Reversible first**: take low-impact, recoverable steps first; irreversible actions need additional approval.
- **Humans are accountable**: name a reachable owner and an escalation path; the agent is not the party that bears the risk.
- **Data minimization**: keep only the records operations need, with defined access and retention.
- **Reviewable, not overstated**: integrity checks help detect changes but are not a tamper-proof guarantee or a third-party audit.

## Deployment considerations

Deployers should record the runtime environment, policy version, model and tool versions, permission settings, data flows and retention policy. Configure isolation, rate limits, timeouts, retry limits and a human stop mechanism according to risk. This architecture document does not by itself prove that any deployment has implemented these controls.

## The control plane is human-change-only

Distinguish the work an agent does from the mechanisms that constrain that work. The second group is the control plane: policies, gates and their rules, review procedures, security mechanisms, and anything that spends money.

- Agents may **read** the control plane. Being able to inspect a rule is how an actor avoids breaking it, and how problems in the rule get noticed.
- Agents may **not change** it. When a change is needed, an agent prepares a candidate version with tests and an explanation of what becomes stricter or looser; a human applies it.
- After a change, synchronize every copy of the control in the same operation, and treat the change as incomplete until they match. Divergent copies mean the same action is blocked in one place and allowed in another.

The reason is structural, not a judgment about any particular agent: an actor that can adjust the checks on its own work is not being checked. Pay particular attention to changes that loosen a control — those are the ones to slow down.
