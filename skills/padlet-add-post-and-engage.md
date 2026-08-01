---
name: Add a post to a board and engage with it
description: Create a post on a Padlet board, then add a comment and a reaction to a post.
api: openapi/padlet-openapi.yml
operations: [add-post, create-comment, create-reaction]
generated: '2026-07-20'
method: generated
source: https://docs.padlet.dev/reference/add-post
---

# Add a post to a board and engage with it

Create content on a Padlet board and interact with posts. Base `https://api.padlet.dev/v1`, JSON:API, `x-api-key` header, admin access required.

## Steps
1. Create a post: `add-post` → `POST /boards/{board_id}/posts` with a JSON:API `data` body (`type: post`, `attributes.content` with `subject`/`bodyHtml`/optional `attachment`). The response returns the new `post_` id.
2. Comment on a post: `create-comment` → `POST /posts/{post_hashid}/comments` with `attributes.content`.
3. React to a post: `create-reaction` → `POST /posts/{post_id}/reactions` with `attributes.reactionType` (`like`, `star`, `grade`, or `vote`) and a `value`.

## Rules
- Every write body follows JSON:API (`data.type`, `data.attributes`, camelCase fields).
- Only admins of the board may write (`NOT_ADMIN` / 401 otherwise); non-paying users get `NOT_PAYING_USER` / 403.
- `NOT_UNIQUE` (422) means a duplicate already exists.
- No idempotency-key is supported — do not blindly retry a POST after an ambiguous failure; re-read the board to check state first.
- Respect the 250 req/min per-IP rate limit (`RATE_LIMIT_EXCEEDED` / 429).
