---
description: Diagnose flash-nexus startup, Cloudflare credentials, DNS conflicts, and local origin problems.
---

# Troubleshooting

Start by checking the installed tools and the config loaded from the intended directory:

~~~sh
npx flash-nexus --version
cloudflared --version
npx flash-nexus --dry-run
~~~

`--dry-run` does not call Cloudflare. See [what it prints](../reference/cli).

## Nexus cannot find cloudflared

Install [cloudflared](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/) and put it on PATH, or set `FLASH_NEXUS_CLOUDFLARED_PATH` to the executable. The npm package does not install that executable.

## Named preview fails

| Symptom | What to check |
| --- | --- |
| `zone is required` | Set `--zone`, `FLASH_NEXUS_ZONE` or `dns.zone` to a domain such as `example.com`. The zone ID alone is not enough. |
| Cloudflare returns 401 or 403 | Check the API token, its account and zone scope, and [the required permissions](./custom-domains). |
| Account or zone cannot be resolved | Set `CLOUDFLARE_ACCOUNT_ID` explicitly. Check that the zone is active and in that account. |
| Hostname is outside the zone | Use a hostname under the zone set in Nexus. If you meant to generate a subdomain, check for a full hostname in `.env`. |
| DNS record conflict | Choose another hostname or inspect ownership of the existing record. |

If you only need a temporary URL, pass `--mode quick`. The Nexus default is `named`.

## The local server or public URL is not ready

- Open the local address from the same machine. Then use `--url` to attach to that running service.
- If auto-detection picks the wrong project in a monorepo, pass `--cwd apps/web` or configure [`server.command` and `server.port`](./local-servers).
- If the origin rejects the public hostname, configure the allowed host in the app. See [the local server guide](./local-servers).
- If the public readiness check times out, inspect the `cloudflared` output and confirm the local app is still running.

For leftovers after an interrupted exit, see [cleanup](./cleanup). For a bug report, include the Nexus version, operating system, command and error message. Remove credentials before sharing the output. [Open an issue](https://github.com/flash-nexus/flash-nexus/issues).
