# Receipts and audit records

> 繁體中文：[zh-TW/receipts-and-audit.md](zh-TW/receipts-and-audit.md)

A receipt summarizes the reviewable events of a piece of work. It is not a complete security control, and it must not become a copy of secrets or sensitive output.

## Recommended fields

- Work-order identifier and version, status, start and end time.
- Internal identifiers for the requesting, approving and executing identities (minimized as needed).
- Policy, model, tool and runtime versions.
- Summary of tool calls, approval events, gate results and error categories.
- Acceptance status, human reviewer, open issues and follow-up actions.
- If hashes are used, the algorithm and exactly what content was hashed.

Never record plaintext credentials, private keys, full sensitive prompts, unnecessary personal data or customer content that has not been approved. Access, retention, deletion and export of logs follow the applicable policy.

## Integrity and limitations

A SHA-256 hash can show that specific bytes match a known digest. A hash is not a digital signature: it cannot prove authorship, and it cannot stop someone with access from changing both the data and the digest. Digital signatures require a controlled private key and a verification process. Whether a log is "append-only" depends on implementation, permissions and storage configuration; it is not an absolute guarantee against tampering.

Important records can additionally use restricted write access, separated administration, backups, timestamps and periodic reconciliation. If you adopt these measures, describe the concrete implementation and the remaining risk instead of claiming the records are "absolutely tamper-proof".

## Example

[`examples/receipt.example.json`](../examples/receipt.example.json) is a blank field template. It contains no real identifiers, execution records or signatures.

## Every run leaves a receipt

A receipt is not a reward for success. Work that failed, was stopped, was cancelled or never passed a gate also ends with a record, stating where it stopped and why. Otherwise the only runs you can study afterwards are the ones that went well.

## The journey record

Besides the outcome, a receipt should let a reviewer reconstruct the path the work took:

- When each review seat acted, and what it decided.
- Each time the work was blocked or rejected: by which gate, for what reason, and what was done about it.
- Anomalies raised, who handled them and how.
- Escalations to a human, and the decision that came back.
- For a release: what was released, when, and what was observed afterwards.

Keep this as short structured entries, not narrative. Its purpose is to answer "what happened on the way?" months later without relying on anyone's memory.

## Incident notes and repeat prevention

When an actor is blocked by a gate, or makes an error, it writes an incident note at that moment: what it did, which rule or gate was involved, the consequence, the root cause and the correction. Do this even when the actor believes the gate was wrong — say so in the note. A false block is information about the gate.

A note that is only stored has little effect: the same mistake recurs in the next session, by an actor that never saw it. To make notes work:

1. **Attach a detectable signature.** For each note, ask what a machine could look for to catch the same mistake earlier — a pattern in a submission, a missing step, an unexpected state.
2. **Turn it into a candidate check.** The signature becomes a proposed gate rule, with a case that should be caught and a case that should not.
3. **A human approves it.** Agents propose; they do not install rules that govern themselves (see [architecture.md](architecture.md#the-control-plane-is-human-change-only)).
4. **Be honest about the rest.** Some mistakes have no reliable signature. Mark those as "reminder only" instead of pretending they are prevented.

Measured this way, the value of your incident notes is the number of mistakes that can no longer happen twice.
