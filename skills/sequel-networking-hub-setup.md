---
name: Set up and embed a Sequel networking hub
description: Create a networking hub, confirm it, retrieve its embed code, and list its participants.
api: openapi/sequel-openapi-original.yml
operations: [addNetworkingHub, getNetworkingHubById, getEmbedNetworkingHubById, returnNetworkingHubParticipants]
---

# Set up and embed a Sequel networking hub

Provision a networking hub (circles, chat, participants) and embed it on your own domain.

## Auth
Obtain a JWT via the client-credentials flow (see `authentication/sequel-authentication.yml`) and send `Authorization: Bearer <token>`. Base URL `https://api.introvoke.com/api/v1`.

## Steps
1. **Create the hub** — `addNetworkingHub` (POST `/networking`). Capture the returned hub `id`.
2. **Confirm** — `getNetworkingHubById` (GET `/networking/{id}`) to read back its configuration.
3. **Embed** — `getEmbedNetworkingHubById` (POST `/networking/{id}/embedCode`) to get the embeddable component code.
4. **Monitor** — `returnNetworkingHubParticipants` (GET `/networking/{id}/participantsList`) to list current participants.

## Rules
- Error envelope: `{ "hasError": true, "errorMessage": "..." }`.
- `401` -> refresh token; `403` -> the client cannot access that hub's company; `404` -> unknown hub `id`.
- No idempotency support — verify before retrying create operations.
