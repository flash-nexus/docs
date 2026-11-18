---
description: What flash-tunnel does, how previews work, and when to use flash-nexus.
---
# Meet flash-tunnel

**A local origin becomes a shareable public preview.** `flash-tunnel` starts or attaches to a local server, connects it through Cloudflare Tunnel, and manages the Tunnel lifecycle.

It is the standalone Tunnel layer in the [Flash Nexus ecosystem](./ecosystem). DNS automation and complete session orchestration are provided by `flash-dns` and `flash-nexus`.

::: info Early preview
flash-tunnel is currently in the `0.x` stage. APIs and behavior may change. These docs describe the implementation in the [project repository](https://github.com/flash-nexus/flash-tunnel).
:::

## Two ways to share

| | Quick mode | Named mode |
| --- | --- | --- |
| Public address | Random `*.trycloudflare.com` URL | A hostname that already resolves to the Tunnel |
| Cloudflare API credentials | Not required | Required |
| DNS management | None | Managed outside standalone flash-tunnel |
| Best for | Temporary demos and collaboration | A stable hostname managed by another workflow |
| Start | `npx flash-tunnel --mode quick` | `npx flash-tunnel --mode named --hostname preview.example.com` |

The default standalone mode is **quick**. Use `--mode named` when a hostname already exists.

## What happens when you run it?

1. Detect or attach to the local origin.
2. Start `cloudflared` with the selected Tunnel mode.
3. Wait for the public URL to become ready.
4. Print the URL and session information.
5. Stop the Tunnel and the local processes owned by this session on exit.

`flash-tunnel` does not create or delete DNS records. Use `flash-nexus` when DNS and Tunnel need to be created and cleaned up together.

## What you need

- Node.js **22.14 or newer**.
- The `cloudflared` executable, available on your `PATH` or configured explicitly.
- A local project, a built static directory, or an already running HTTP server.
- For named mode: a Cloudflare API token and a hostname already prepared by DNS.

## Start here

[Create your first preview](./quick-start), [understand the ecosystem](./ecosystem), or look up a flag in the [CLI reference](../reference/cli).
