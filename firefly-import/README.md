# firefly-import

Imports bank data into Firefly III. Bank exports are read from Google Drive, every
transaction is labelled by an AI matcher that reuses what it learned before, and the final
list is pushed into Firefly through its API.

## Workflows

| File | Workflow name | ID | Active | Nodes | Role |
| --- | --- | --- | --- | --- | --- |
| `MAIN - Import Data To Firefly.json` | MAIN - Import Data To Firefly | `sMxeFl2CuSMSykhX` | yes | 16 | Orchestrator. Runs the four account readers and calls the subset importer once per account. |
| `SUBSET - Import Data to Firefly.json` | SUBSET - Import Data to Firefly | `0uWPJg8m7HT9PyLT` | yes | 19 | Labels a batch of transactions and writes them to Firefly. Creates any category or budget that is missing. |
| `Get Best Match.json` | Get Best Match | `oszZ1Ud4sQlpltZ3` | yes | 13 | The AI matcher. Loads candidates and past examples, asks the JSON agent for the best match and stores the new example. |
| `Read And Structure NLB Main.json` | Read And Structure NLB Main | `lZbuzq0isCbt7rVq` | yes | 4 | Reads and normalises the NLB main account export. |
| `Read And Structure NLB Savings.json` | Read And Structure NLB Savings | `qzaGpoJsxkrpf1ny` | yes | 4 | Reads and normalises the NLB savings export. |
| `Read And Structure Revolut.json` | Read And Structure Revolut | `zc62SIi0Evmqxtza` | yes | 4 | Reads and normalises the Revolut export. |
| `Read And Structure VISA.json` | Read And Structure VISA | `5DvRXRtfBuQ2Nbn0` | yes | 7 | Reads and normalises the VISA export. Downloads the file itself instead of using the shared reader. |
| `Calculate initial Amount.json` | Calculate initial Amount | `p3M2xTVkwyt6smsx` | no | 14 | Utility that runs the four readers and reports the opening amounts. |

## Dependency graph

```
MAIN - Import Data To Firefly
  -> Read And Structure NLB Main / NLB Savings / Revolut / VISA
       -> Read CSV Files from Google Drive Folder (Subworkflow)   [csv-reader]
  -> SUBSET - Import Data to Firefly
       -> Get Best Match
            -> AI Json Agent                                     [ai-utility]
Calculate initial Amount -> Read And Structure (all four)
```

Cross folder calls mean the pipelines must be imported into the same n8n instance, or the
`Execute Sub-workflow` nodes must be pointed at their target again after import.

## How the AI matcher works

`Get Best Match` is the part that makes the import smart. For each unknown transaction it
loads the merchants seen before and the decisions taken on them, sends the transaction and
the examples to `AI Json Agent` and asks for the single best match. When the answer comes
back it writes the pair of transaction and decision to the example store, so the next run
has more to learn from. Over time the matcher needs fewer guesses and the categories stay
consistent.

## Dependencies

- Credentials: Google Drive OAuth2 (used by `MAIN` and `Read And Structure VISA`) and HTTP
  bearer auth (used by `SUBSET`). The other readers use the Drive credential held by the CSV
  reader sub-workflow.
- Environment: `MATCHING_API_KEY`.
- External services: `api.example.com` for the matching endpoints and the Firefly
  category and budget helpers, and `firefly.example.com` for the Firefly III transaction
  API. Both are placeholders, so repoint them at your own services. Requests carry
  `x-api-user: system.user@local.local` and `x-api-key: ={{ $env.MATCHING_API_KEY }}`.

## Notes

Pinned data was emptied before publishing. It held real transactions with merchant names,
amounts and account holder names. The earlier files also carried a hardcoded API key, which
is now the `MATCHING_API_KEY` environment expression.
