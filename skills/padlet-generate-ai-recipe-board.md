---
name: Generate an AI recipe board and poll its status
description: Kick off an AI-generated Padlet board and poll the async creation status until ready.
api: openapi/padlet-openapi.yml
operations: [create-ai-recipe-board, get-ai-recipe-board-status]
generated: '2026-07-20'
method: generated
source: https://docs.padlet.dev/reference/create-ai-recipe-board
---

# Generate an AI recipe board and poll its status

Padlet can generate a board from an AI "recipe". Creation is asynchronous. Base `https://api.padlet.dev/v1`, JSON:API, `x-api-key` header.

## Steps
1. Start generation: `create-ai-recipe-board` → `POST /ai-recipe-boards` with the recipe attributes. The response returns a status key / status URL rather than a finished board.
2. Poll status: `get-ai-recipe-board-status` → `GET /ai-recipe-boards/status/{status_key}`. Repeat until the AI recipe board creation status object reports completion, then read the resulting board with the read-board skill (`get-board-by-id`).

## Rules
- Poll politely — space status calls out and stay under 250 req/min per IP (`RATE_LIMIT_EXCEEDED` / 429).
- Requires a paying user (`NOT_PAYING_USER` / 403 otherwise).
- Handle `errors[]` by `code`; a `NOT_FOUND` on the status key means the job expired or the key is wrong.
