---
description: Use an existing hostname with standalone flash-tunnel or let flash-nexus manage DNS.
---
# Custom domains

There are two ways to use your own domain:

- **Standalone flash-tunnel:** prepare the DNS record separately, then pass the existing hostname.
- **flash-nexus:** configure the zone and hostname rules once; Nexus uses `flash-dns` and `flash-tunnel` together and cleans up the resources it owns.

## Standalone flash-tunnel

The standalone Tunnel command does not create or delete DNS records. Prepare a hostname with `flash-dns` or another DNS workflow, then start the Tunnel:

```sh
npx flash-tunnel --mode named --hostname preview.example.com --url http://127.0.0.1:4317
```

Set the credentials in the environment:

```sh
export CLOUDFLARE_API_TOKEN='your_token_here'
export CLOUDFLARE_ACCOUNT_ID='your_account_id'
```

The token is read from the environment. Keep it out of project configuration and Git history.

## Managed DNS with flash-nexus

Use `flash-nexus.config.ts` when the same session should create the DNS record and Tunnel:

```ts
import { defineConfig } from 'flash-nexus'

export default defineConfig({
  tunnel: {
    mode: 'named',
    hostname: 'preview.example.com'
  },
  dns: {
    zone: 'example.com'
  }
})
```

Then run:

```sh
npx flash-nexus
```

Nexus loads `.env`, creates the Tunnel through `flash-tunnel`, creates the DNS record through `flash-dns`, and removes both resources during cleanup.

## DNS permissions

For the managed flow, create a Cloudflare Custom token with the permissions required by both providers:

| Permission | Used for |
| --- | --- |
| Account · Cloudflare Tunnel · Edit | Create and delete named Tunnels and configure ingress |
| Zone · DNS · Edit | Create and delete the session-owned CNAME record |
| Zone · Zone · Read | Resolve and validate the DNS zone |

The exact permissions depend on the provider workflow you choose. Keep the token scoped to the account and zones used by the project.

## Subdomain templates

For a standalone Tunnel, pass the complete hostname with `--hostname`. For a managed Nexus workflow, put the zone and naming rules in `flash-nexus.config.ts`; `flash-dns` renders and validates the hostname before the Tunnel starts.

```ts
export default defineConfig({
  dns: {
    zone: 'example.com',
    naming: { template: 'fd-{project}.example.com' }
  }
})
```

## Naming and conflicts

Put `zone`, `hostname`, or naming rules in `flash-nexus.config.ts`. Keep Cloudflare credentials in `.env` or the environment. `flash-dns` records ownership metadata so a session can detect conflicts and remove only records it owns.
