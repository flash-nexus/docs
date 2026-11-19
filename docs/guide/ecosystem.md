---
description: See how the Nexus CLI and SDK depend on flash-tunnel and flash-dns.
---

# The Flash Nexus ecosystem

Use **flash-nexus** to start a preview. It composes two libraries with separate responsibilities.

~~~mermaid
flowchart TD
    accTitle: Flash Nexus package dependencies
    accDescr: Nexus uses flash-tunnel for traffic and flash-dns for named DNS records.
    Nexus["flash-nexus"] -->|Tunnel lifecycle| Tunnel["flash-tunnel"]
    Nexus -->|DNS in named mode| DNS["flash-dns"]
~~~

| Project | Responsibility |
| --- | --- |
| [flash-nexus](https://github.com/flash-nexus/flash-nexus) | Runs the complete preview from the [CLI](./quick-start) or [SDK](./automation). |
| [flash-tunnel](https://github.com/flash-nexus/flash-tunnel) | Starts and stops Cloudflare Tunnel connections and manages `cloudflared`. |
| [flash-dns](https://github.com/flash-nexus/flash-dns) | Resolves hostnames and manages DNS records with ownership checks. |

## Named preview lifecycle

1. Nexus resolves the local server, DNS zone, and public hostname.
2. It provisions a Cloudflare Tunnel and gets the target hostname from `flash-tunnel`.
3. It uses `flash-dns` to create a session-owned CNAME pointing to the tunnel.
4. When the preview is ready, Nexus prints the public URL. On shutdown, it removes the DNS record before deprovisioning the tunnel.

Quick mode uses a Cloudflare-assigned URL and skips the DNS step. See [custom domains](./custom-domains) and [session cleanup](./cleanup).

## flash-tunnel

Use this CLI if you only need the connection layer:

~~~sh
npx flash-tunnel --mode quick --url http://127.0.0.1:5173
~~~

Its named mode accepts a full hostname. You manage that hostname's DNS separately. See the [standalone Tunnel README](https://github.com/flash-nexus/flash-tunnel#readme).

## flash-dns

Use `flash-dns plan` to inspect a configured change before writing. `flash-dns ensure` applies the change, subject to record ownership. See the [DNS README](https://github.com/flash-nexus/flash-dns#readme) for examples and provider configuration.
