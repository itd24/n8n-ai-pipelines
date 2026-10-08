# json-loader

A sub-workflow that downloads one named JSON file from a Google Drive folder and returns its
parsed content.

## Workflows

| File | Workflow name | ID | Active | Nodes |
| --- | --- | --- | --- | --- |
| `Load Json from drive.json` | Load Json from drive | `5OEZbpYMy7HYq7ZN` | no | 4 |

## Interface

- Trigger: an Execute Sub-workflow trigger that takes a folder ID and a file name.
- Output: the JSON payload of the requested file.

## Dependencies

- Workflows: none.
- Credentials: Google Drive OAuth2.
- External services: none.

## Notes

The workflow is inactive because it only runs as a sub-workflow. Call it from any workflow
that needs a JSON file from Drive.
