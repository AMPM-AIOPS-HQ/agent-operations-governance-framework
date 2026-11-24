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
