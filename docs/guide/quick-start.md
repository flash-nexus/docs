---
description: Share an existing local service with flash-nexus and Cloudflare Quick Tunnel.
---

# Quick start

Start here with a temporary URL. You can use it without a Cloudflare account, API token, or DNS configuration.

## 1. Check your tools

Use Node.js **22.14 or later** and install [cloudflared](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/).

~~~sh
node --version
cloudflared --version
~~~

If `cloudflared` is outside your PATH, set [`FLASH_NEXUS_CLOUDFLARED_PATH`](../reference/environment) to the executable path.

## 2. Open a preview

Run this while your app or API is listening locally. Replace the port with the one your app uses.

~~~sh
npx flash-nexus --mode quick --url http://127.0.0.1:5173
~~~

The terminal prints a `local` URL and a `public` URL. Open the public `trycloudflare.com` address from a browser or send it to a teammate. Keep Nexus running while the link is in use.

::: tip Let Nexus start the server
If you're in a project with a runnable dev script, try `npx flash-nexus --mode quick`. You can specify a startup command in [the project configuration](./configuration).
:::

## 3. End the preview

Press **Ctrl+C** and let cleanup finish. A server you supplied through `--url` continues running. A new quick preview gets a new public address.

[Use your own domain](./custom-domains) · [Set up a project command](./local-servers)
