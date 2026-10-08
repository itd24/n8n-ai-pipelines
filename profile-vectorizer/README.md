# profile-vectorizer

Builds a profile vector from the CV data in Google Sheets, writes it back to the sheet,
uploads it to Google Drive and reports over Telegram. The job application pipeline matches
against these vectors.

## Workflows

| File | Workflow name | ID | Active | Nodes |
| --- | --- | --- | --- | --- |
| `Profile Vectorizer.json` | Profile Vectorizer | `oQOL10ZVVKVor07P` | yes | 50 |

## Interface

- Triggers: a webhook and a manual trigger.
- Input: CV rows from the configured Google Sheets workbook.
- Output: the vector written to the sheet, a copy uploaded to Google Drive and a Telegram
  notification.

## How the AI part works

Four Anthropic agents run over the CV data before the vector is stored. `Strength Signal
Calculator` rates each entry, `Role Estimator` guesses the role it points at and
`Experience Achievement Evaluator` scores the achievements. Their output is folded into one
compact profile vector that the job pipeline compares against a posting.

## Dependencies

- Workflows: none. The workflow is standalone.
- Credentials: Anthropic API, Google Sheets OAuth2, Google Drive OAuth2, Telegram API and
  HTTP header auth for the vector storage call.
- Environment: `TELEGRAM_CHAT_ID` for the Telegram notification.
- External services: Anthropic for the agent calls that rate and enrich the CV entries.

## Notes

Sheet, drive and folder IDs inside the workflow are required for it to run and are protected
by the Google account permissions rather than by the file itself.
