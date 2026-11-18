---
description: Understand how flash-nexus, flash-dns, and flash-tunnel fit together.
---
# The Flash Nexus ecosystem

Flash Nexus separates preview orchestration from the provider operations that make it work. Choose the layer that matches the job:

```text
flash-nexus       coordinates one complete preview session
├── flash-dns     manages DNS records and ownership
└── flash-tunnel  manages Tunnel adapters and cloudflared
```

## flash-tunnel

`flash-tunnel` is the standalone Tunnel package. It accepts a local origin, starts `cloudflared`, waits for a public URL, and stops the Tunnel it owns.

Use it when:

- you only need a temporary `trycloudflare.com` URL;
- a hostname and DNS record already exist;
- another system owns DNS and should remain responsible for it.

In named mode, standalone `flash-tunnel` expects the hostname to already resolve to the Tunnel. It does not create or delete DNS records.

## flash-dns

`flash-dns` is the DNS package. It resolves zones, renders hostnames, reconciles records, and tracks ownership so a session can safely remove only what it created.

Use it independently for deployment previews, record automation, or any workflow that does not need a Tunnel.

## flash-nexus

`flash-nexus` is the integration layer. It loads project configuration and environment variables, starts or attaches to the local server, asks `flash-tunnel` for a Tunnel, and uses `flash-dns` to create and clean up the DNS record for named sessions.

Use it when one command should own the complete lifecycle:

1. Load `flash-nexus.config.ts` and `.env`.
2. Start or attach to the local origin.
3. Create the named Tunnel through `flash-tunnel`.
4. Create the DNS record through `flash-dns`.
5. Print the public URL.
6. Remove the DNS record and Tunnel during cleanup.

Cloudflare credentials stay in the environment. Keep `CLOUDFLARE_API_TOKEN`, `CLOUDFLARE_ACCOUNT_ID`, and `CLOUDFLARE_ZONE_ID` out of project configuration and source control.

## Which command should I use?

| Need | Use |
| --- | --- |
| Temporary public URL | `npx flash-tunnel --mode quick` |
| Tunnel for an existing hostname | `npx flash-tunnel --mode named --hostname preview.example.com` |
| DNS record automation only | `npx flash-dns plan` / `npx flash-dns ensure` |
| One command for local server DNS Tunnel and cleanup | `npx flash-nexus` |

For mobile and device debugging, `flash-debug` builds on `flash-nexus` and adds the Inspector, log upload APIs, Flutter SDK integration, and ADB or HDC clients.
