# Deployment port registry

This application publishes host **8014** to container **80** in PROD,
TEST, STAGING, and future environments. The host port is fixed in Compose.
Use the domain for normal access. Internal listening ports and Docker DNS stay unchanged.

`BIND_ADDRESS` defaults to `127.0.0.1` for a proxy on the same host. Set it in
the untracked `.env` to a LAN address or `0.0.0.0` only when LAN clients or a
proxy on another machine need access. Never change the host port to resolve a conflict.
Verify the existing owner of the registry port first.

Validate with `docker compose config -q`, then recreate only the application
service with `docker compose up -d --no-deps --no-build SERVICE`. Confirm
`docker compose ps`, container logs, the new host endpoint and domain access.
Update all nginx/Cloudflare origins after the new endpoint is healthy. Preserve
the old route temporarily if a remote proxy cannot be updated yet.
Keep databases, Redis, queues, workers and backup containers unpublished.

## Global registry

```text
8001 SUAR Services
8002 Nuqta Pro
8003 Nuqta ID
8004 Nuqta Health
8005 Nuqta IT
8006 Nuqta SY
8007 Nuqta Sale / eBazar
8008 Zanist
8009 Students Platform
8010 Files
8011 Girocode
8012 Immobilienrechner
8013 Buchhaltung
8014 Meraki Dashboard Frontend
8015 Meraki Dashboard Backend/API
8016 Telegram Calendar Bot
8017 SpiderFoot
```

Reserved ports remain unused when that application is absent or internal-only.
Meraki's API remains internal when frontend nginx is its only consumer; 8015 is
reserved for deployments that demonstrably need a host-facing API.
