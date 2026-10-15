# Event Log — Source of Truth

Append-only JSONL. One object per line. No nesting, no migration path.

## Contract

| Field | Type | Rule |
|---|---|---|
| `ts` | string | ISO 8601 with offset — never drop timezone |
| `crew` | string | Crew ID only. No names. Name mapping lives in a local file that never deploys |
| `type` | string | Fixed enum: `no-show`, `late`, `present`, `left early`. Loose by convention — old readers skip unknown types, new readers handle them. No version bump |
| `sev` | number | 1–5. 1 is informational, 5 is drop-everything. Dashboard sorts descending |
| `note` | string | Optional free text for context the enum can't carry |

## Example

```json
{"ts":"2026-10-15T07:42:00-05:00","crew":"jdoe","type":"no-show","sev":5,"note":"no call no show"}
```

## Path

iPhone (Working Copy) → SFTP → daily file on server

Hand-edit one file per day to start. Prove the dashboard reads it before any automation writes to it.
