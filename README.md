# flowise-hs1

Flowise production-style stack for hs1 using Docker Compose, dedicated PostgreSQL, and Cloudflare Tunnel ingress.

## Services
- flowise (`127.0.0.1:3000`)
- dedicated postgres (`flowise-postgres`)
- cloudflared tunnel connector

## Setup
1. Copy env template:
   cp .env.example .env
2. Set secrets and hostname values.
3. Start:
   docker compose --env-file .env -f compose.yaml up -d

## Verify
- docker compose --env-file .env -f compose.yaml ps
- docker compose --env-file .env -f compose.yaml logs --tail 100 flowise flowise-postgres cloudflared
- curl -I http://127.0.0.1:3000/

## Cloudflare
In Tunnel Public Hostnames, add:
- hostname: flowise-hs1.parkerbrown.dev
- service: http://flowise:3000

## Cloudflare Access (recommended)
To avoid login/session issues when Access is enabled:
1. Create one Access `Self-hosted` application for `flowise-hs1.parkerbrown.dev` (path `*`).
2. In the Access app CORS settings, include origin `https://flowise-hs1.parkerbrown.dev` and allow methods including `OPTIONS`.
3. Keep one DNS/tunnel route for this hostname (no duplicate records to different origins).
4. Use a normal browser profile for testing app login (Flowise warns against Incognito/private mode for auth flows).

References:
- https://developers.cloudflare.com/cloudflare-one/access-controls/applications/http-apps/
- https://developers.cloudflare.com/cloudflare-one/access-controls/applications/http-apps/authorization-cookie/cors/
- https://docs.flowiseai.com/configuration/environment-variables
