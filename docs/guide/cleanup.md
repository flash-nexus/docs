---
description: What flash-nexus removes when a session ends and what to check if cleanup is interrupted.
---

# Stop a preview and clean up

Press **Ctrl+C** in the terminal or call `await preview.stop()` from the [SDK](./automation). Keep the process running until cleanup finishes.

## What gets removed

| Resource | On normal shutdown |
| --- | --- |
| `cloudflared` process | Stops with the preview. |
| DNS record in named mode | The session-owned CNAME is removed before the tunnel resource. |
| Cloudflare Tunnel in named mode | The tunnel created for the session is deprovisioned. |
| Local server started by Nexus | Stops with the preview. |
| Local server supplied through `--url` or `server.url` | Continues running. |

Quick mode has no account-owned DNS record. If you set [`tunnel.keep`](../reference/config#tunnel), cloud resources are retained, but the local tunnel process still stops. Keeping the resources does not keep the URL serving.

## If shutdown was interrupted

With state persistence enabled, the Tunnel core keeps session state and logs under `~/.flash-tunnel/` by default. If Nexus cannot complete cleanup, use those details and the terminal output to identify the session. In the Cloudflare dashboard, check for the matching tunnel and CNAME before removing any leftover resources.

::: info Record ownership
A named preview owns only its own DNS record. A matching hostname can also belong to another session or workflow. Check ownership before editing the record.
:::

For automated use, [stop the SDK session in a finally block](./automation) and keep the Cloudflare credentials available until shutdown finishes.
