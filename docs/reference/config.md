---
description: Supported flash-tunnel.ts fields and the boundary between Tunnel and DNS configuration.
---
# Configuration reference

Import `defineConfig` from `flash-tunnel` for editor completion. Use `flash-tunnel.ts` for standalone local-server and Tunnel settings. Use `flash-nexus.config.ts` when DNS orchestration is part of the same workflow.

## Server

| Field | Type | Default / behavior |
| --- | --- | --- |
| `server.command` | `string` | Auto-detected startup script |
| `server.cwd` | `string` | Config directory when a file is loaded |
| `server.port` | `number` | Detected from script, framework, or server mode |
| `server.url` | `string` | Existing server to attach to |
| `server.dir` | `string` | Static directory to serve |
| `server.timeout` | `number` | `60000` milliseconds |

Choose one of `command`, `url`, or `dir`. Relative paths resolve from the configuration directory.

## Tunnel

| Field | Type | Default / behavior |
| --- | --- | --- |
| `tunnel.provider` | `string` | `'cloudflare'` |
| `tunnel.mode` | `'quick' \| 'named'` | `'quick'` for standalone flash-tunnel |
| `tunnel.name` | `string` | Derived from the project directory |
| `tunnel.hostname` | `string` | Existing hostname for named mode |

Quick mode returns a random `trycloudflare.com` URL. Named mode requires a hostname that DNS already points to the Tunnel.

## Cloudflare options

Non-sensitive Tunnel options remain under the provider configuration:

| Field | Type | Default / behavior |
| --- | --- | --- |
| `providers.cloudflare.cloudflaredPath` | `string` | `'cloudflared'` |
| `providers.cloudflare.originRequest.httpHostHeader` | `string` | Optional origin Host header override |
| `providers.cloudflare.originRequest.noTLSVerify` | `boolean` | Optional origin TLS verification override |

Cloudflare credentials are not configuration fields. The adapter reads them from the environment:

```env
CLOUDFLARE_API_TOKEN=...
CLOUDFLARE_ACCOUNT_ID=...
```

`CLOUDFLARE_ZONE_ID` is consumed by the DNS adapter when a DNS workflow needs to skip zone-name lookup. It belongs to `flash-dns` or `flash-nexus`, not to `flash-tunnel.ts`.

## Example

```ts
import { defineConfig } from 'flash-tunnel'

export default defineConfig({
  server: { url: 'http://127.0.0.1:3000' },
  tunnel: { mode: 'quick' }
})
```

For a complete named workflow, see the [Flash Nexus ecosystem guide](../guide/ecosystem) and [custom domains](../guide/custom-domains).

Definitions are maintained in the [flash-tunnel source repository](https://github.com/flash-nexus/flash-tunnel/tree/main/packages/config) and the [flash-nexus configuration](https://github.com/flash-nexus/flash-nexus).
