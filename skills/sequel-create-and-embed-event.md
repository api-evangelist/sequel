---
name: Create and embed a Sequel webinar event
description: Authenticate, create a webinar event under a company, verify it, and retrieve the embed code to render it on your own site.
api: openapi/sequel-openapi-original.yml
operations: [addEvent, geteventById, getEmbedEventById]
---

# Create and embed a Sequel webinar event

Use the Sequel (Introvoke) API to programmatically create a webinar event and get an embeddable player for your own domain.

## Auth
1. POST `https://api.introvoke.com/api/oauth/token` with `client_id`, `client_secret`, `audience: https://www.introvoke.com/api`, `grant_type: client_credentials` (get credentials from Admin Dashboard > Integrations > APIs).
2. Use the returned `access_token` as `Authorization: Bearer <token>` on every call.
3. Base URL: `https://api.introvoke.com/api/v1`.

## Steps
1. **Create the event** — `addEvent` (POST `/event/create`). Provide the event body (title, start time, company, etc.). Capture the returned `eventId`.
2. **Verify** — `geteventById` (GET `/event/{eventId}`) to confirm the event was created and read back its configuration.
3. **Get the embed** — `getEmbedEventById` (POST `/event/{eventId}/embedCode`) to retrieve the embed code, then drop it into your CMS/page (or render via `@sequel.io/api`).

## Rules
- On failure the API returns `{ "hasError": true, "errorMessage": "..." }` — surface `errorMessage`.
- `401` means the JWT is missing/expired: re-run the token call. `404` means the `eventId`/`companyId` is wrong or not in your account.
- No idempotency key is supported; do not blindly retry `addEvent` on a network timeout — verify with a list/get first to avoid duplicate events.
