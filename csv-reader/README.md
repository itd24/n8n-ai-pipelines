# csv-reader

A sub-workflow that reads every CSV file in a Google Drive folder and returns the parsed
rows.

## Workflows

| File | Workflow name | ID | Active | Nodes |
| --- | --- | --- | --- | --- |
| `Read CSV Files from Google Drive Folder (Subworkflow).json` | Read CSV Files from Google Drive Folder (Subworkflow) | `Z5f5YMUT4TtBXlpA` | yes | 4 |

## Interface

- Trigger: an Execute Sub-workflow trigger that takes a folder ID.
- Output: one item per CSV row, with the source file name attached so the caller can group
  rows per file.

## Dependencies

- Workflows: none.
- Credentials: Google Drive OAuth2.
- External services: none.

## Notes

Pinned data was emptied before publishing. The old sample held real bank export rows. This
workflow is called by `firefly-import/Read And Structure NLB Main`, `Read And Structure NLB
Savings` and `Read And Structure Revolut`. Those calls point at this workflow by ID, so
import both folders into the same n8n instance.
