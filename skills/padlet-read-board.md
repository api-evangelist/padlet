---
name: Read a Padlet board and its posts
description: Fetch a Padlet board with its posts, sections, and comments using the Padlet API.
api: openapi/padlet-openapi.yml
operations: [get-board-by-id, get-post-attachment-data]
generated: '2026-07-20'
method: generated
source: https://docs.padlet.dev/reference/get-board-by-id
---

# Read a Padlet board and its posts

Use the Padlet API (JSON:API, base `https://api.padlet.dev/v1`) to read a board and everything on it.

## Prerequisites
- A Padlet API key from https://padlet.com/dashboard/settings/developers (paying user only).
- Send it on every request in the `x-api-key` header.
- You must be an **admin** of the board (`NOT_ADMIN` / 401 otherwise).

## Steps
1. Find the `board_id` — the 16-character hashid at the end of the padlet URL (or the board's `...` → Developer menu).
2. Call `get-board-by-id`: `GET /boards/{board_id}`. Add `?include=posts,sections,comments` to pull the full compound document instead of just the board attributes.
3. Read the JSON:API response: the board is under `data` (`type: board`); included posts/sections/comments arrive in the `included[]` array, each linked back via `relationships`.
4. For any post attachment payload, call `get-post-attachment-data`: `GET /posts/{post_id}/attachmentData`.

## Rules
- Handle errors from the `errors[]` envelope by `code`: `PADLET_ARCHIVED` (unarchive first), `NOT_ADMIN`, `NOT_FOUND`, `RATE_LIMIT_EXCEEDED`.
- Stay under 250 requests/minute per IP; back off for a minute on a 429.
- Timestamps (`createdAt`/`updatedAt`) are ISO 8601.
