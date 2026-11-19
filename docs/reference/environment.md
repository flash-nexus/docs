---
description: Shared Nexus environment variables, Cloudflare credentials, and lower-level fallbacks.
---

# Environment variables

Nexus reads `.env` in the project directory. Use `--env-file <path>` to select a different file. The same settings can be supplied through the process environment.

## Nexus settings

Values in the Nexus column take precedence over the fallback column. If neither is set, Nexus uses the project config or defaults. CLI flags and explicit SDK options override the corresponding environment setting.

| Purpose | Nexus variable | Lower-level fallback |
| --- | --- | --- |
| Preview mode | `FLASH_NEXUS_MODE` | `FLASH_TUNNEL_MODE` |
| DNS zone name | `FLASH_NEXUS_ZONE` | `FLASH_DNS_ZONE` |
| Full hostname | `FLASH_NEXUS_HOSTNAME` | `FLASH_TUNNEL_HOSTNAME`, then `FLASH_DNS_HOSTNAME` |
| Subdomain pattern | `FLASH_NEXUS_SUBDOMAIN` | `FLASH_TUNNEL_SUBDOMAIN` |
| Path to `cloudflared` | `FLASH_NEXUS_CLOUDFLARED_PATH` | `FLASH_TUNNEL_CLOUDFLARED_PATH` |

The Cloudflare Tunnel adapter also accepts `FLASH_TUNNEL_CLOUDFLARED` when no executable path has been supplied to it. Prefer `FLASH_NEXUS_CLOUDFLARED_PATH` for Nexus.

## Cloudflare credentials

| Variable | Used for |
| --- | --- |
| `CLOUDFLARE_API_TOKEN` | API authentication for named-mode tunnel and DNS changes. |
| `CLOUDFLARE_ACCOUNT_ID` | Select the Cloudflare account. Set it explicitly when you have more than one account. |
| `CLOUDFLARE_ZONE_ID` | Optional zone identifier for the DNS adapter. It does not replace the zone name required by Nexus. |

See [token permissions and setup](../guide/custom-domains). Quick mode does not require these credentials.
