# ai-utility

A shared AI agent that many workflows call. It keeps the prompt, the JSON parsing and the
retry logic in one place, and only the model provider changes. Callers pass a model name,
so no other pipeline has to keep its own copy of the agent.

## Workflows

| File | Workflow name | ID | Active | Nodes |
| --- | --- | --- | --- | --- |
| `AI Json Agent.json` | AI Json Agent | `ex2acZkiUgT8vPZM` | yes | 17 |
| `Claude Json AI Agent.json` | Claude Json AI Agent | `OwVoiLFyN1nRd09N` | no | 5 |

## How it works

`AI Json Agent` takes a system prompt and a user prompt plus a provider selector. A switch
node routes the request to the Anthropic, OpenAI or DeepSeek agent node and returns a JSON
object. `Claude Json AI Agent` is the older version and only talks to Anthropic.

Because the provider is just a parameter, the same agent can run on a cheap model for simple
steps and on a stronger model for hard steps without any change in the calling workflow.

## Dependencies

- Workflows: none.
- Credentials: Anthropic API, OpenAI API and DeepSeek API. `Claude Json AI Agent` needs
  Anthropic and OpenAI.
- External services: none.

## Notes

Called by `firefly-import/Get Best Match` and `job-application/Job Application Pipeline`. The
pinned data in the export only held sample model responses, so it was emptied before
publishing.
