---
description: Reference for the server, tunnel, and DNS fields in flash-nexus.config.
---

# Configuration reference

Save `flash-nexus.config.json` or `flash-nexus.config.ts` in the project directory. For TypeScript, import `defineConfig` from `flash-nexus`. See the [copyable example](../guide/configuration).

## Server

| Field | Purpose |
| --- | --- |
| `server.url` | Attach to an origin that is already running. |
| `server.command` | Start a command such as `pnpm dev`. |
| `server.dir` | Serve a static directory containing `index.html`. |
| `server.cwd` | Working directory for the local server. |
| `server.port` | Port to serve or expect. Does not change the app's own configuration. |
| `server.timeout` | How long to wait for the local server, in milliseconds. |

Choose one of `url`, `command` and `dir`. If none is set, the Tunnel core can detect a project script or static output. `--url` overrides the configured origin.

## Tunnel

| Field | Purpose |
| --- | --- |
| `tunnel.mode` | `quick` or `named`. Defaults to `named`. |
| `tunnel.name` | Cloudflare Tunnel resource name. The public hostname is configured separately. |
| `tunnel.hostname` | Full public hostname. Use it if you do not set `dns.hostname`. |
| `tunnel.subdomain` | Single label or random template used together with the zone. |
| `tunnel.cloudflaredPath` | Path to the `cloudflared` executable if it is outside PATH. |
| `tunnel.originRequest.httpHostHeader` | Override the Host header sent to the origin. |
| `tunnel.originRequest.noTLSVerify` | Disable verification of the HTTPS origin's certificate when explicitly required. |
| `tunnel.autoStopAfterMs` | Stop the session after this many milliseconds. |
| `tunnel.verifyTimeoutMs` | Time allowed for the public URL readiness check. |
| `tunnel.keep` | Retain cloud resources after the local session closes. |
| `tunnel.persist` | Save local session state. Defaults to `true`. |

## DNS

These fields apply in named mode. Nexus creates a CNAME pointing to the tunnel target.

| Field | Purpose |
| --- | --- |
| `dns.zone` | DNS zone name, such as `example.com`. `tunnel.zone` is also accepted. |
| `dns.hostname` | Complete public hostname. Preferred over `tunnel.hostname` in the file. |
| `dns.naming.template` | Hostname template used when a hostname or subdomain is not selected, e.g. `preview-{project}.example.com`. |
| `dns.variables` | Values for placeholders in a DNS naming template. |
| `project.name` | Optional project name used by `{project}`. When omitted, DNS naming can use the package or directory name. |

For a single random label, use `tunnel.subdomain` with a pattern such as `preview-{rand:4}`. For a stable name based on the project, use `dns.naming.template`. [Custom domains](../guide/custom-domains) explains how to choose.

Cloudflare credentials are supplied through [environment variables](./environment). The [configuration types](https://github.com/flash-nexus/flash-nexus/blob/main/packages/types/src/index.ts) define the TypeScript shape.
