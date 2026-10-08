# notion

Building blocks for working with Notion from n8n. Each workflow stands on its own and can be
called as a sub-workflow or wired into a larger pipeline.

## Workflows

| File | Workflow name | ID | Active | Nodes | Purpose |
| --- | --- | --- | --- | --- | --- |
| `Notion Article Upsert.json` | Notion Article Upsert | `X9k3L86DzKHZDqrc` | no | 8 | Create or update an article page from its metadata and blocks. |
| `Recursive Notion Block Loader.json` | Recursive Notion Block Loader | `ZPbbyJA7lDE7JjcZ` | no | 7 | Walk a page and all of its child blocks recursively. |
| `Change article status.json` | Change article status | `gfFR0wR9NDmvTJnr` | no | 4 | Move a page to another status and swap its icon. |
| `Get Page Content.json` | Get Page Content | `Qw2KwyuVg94oVqaS` | no | 6 | Fetch a page and return its blocks as text. |

## Dependencies

- Workflows: `Recursive Notion Block Loader` calls itself and uses the name lookup mode, so
  it keeps working after a re-import.
- Credentials: the Notion API. `Change article status` also uses an HTTP bearer credential
  for its direct REST call.
- External services: `api.notion.com`.

## Notes

Database and page IDs stay in the workflows because the calls need them, and Notion
permissions are what protect the data. Pinned data was emptied before publishing. It held
real page URLs and page content.
