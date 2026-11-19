---
description: What flash-nexus does and when to use temporary or named previews.
---

# What is flash-nexus?

**flash-nexus** gives a service on your machine a public URL. Use it to show a work in progress, test a webhook, or reach a local API from another device. The service continues to run on your machine.

## Choose a preview mode

| Mode | Public URL | What you need |
| --- | --- | --- |
| `quick` | A temporary `trycloudflare.com` URL | Node.js and `cloudflared` |
| `named` | A hostname in your Cloudflare zone | The same tools, plus a Cloudflare zone and API credentials |

Use `quick` for the first run. Use `named` when clients need a hostname you control or you want a recognizable URL for each session.

## What Nexus manages

Nexus loads the project's configuration, starts a local server or attaches to one, and opens the tunnel. In named mode it creates the DNS record pointing to that tunnel. On shutdown it cleans up the resources it owns.

[flash-tunnel] and [flash-dns] are the libraries that provide those operations. You can use either separately when you only need its part of the workflow.

[Start a temporary preview](./quick-start) · [See the dependency graph](./ecosystem)

[flash-tunnel]: ./ecosystem#flash-tunnel
[flash-dns]: ./ecosystem#flash-dns
