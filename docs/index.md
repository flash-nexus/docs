---
layout: home
title: Share a local service
hero:
  name: flash-nexus
  text: Share a local service<br>in one command.
  tagline: Start or connect to a local server, then give it a temporary Cloudflare URL or a hostname on your own domain.
  actions:
    - theme: brand
      text: Quick start
      link: /guide/quick-start
    - theme: alt
      text: How it fits together
      link: /guide/ecosystem
features:
  - title: Temporary previews
    details: Share a running app or API on a temporary trycloudflare.com URL without setting up your own DNS.
    link: /guide/quick-start
  - title: Your own hostname
    details: Use a fixed hostname or a subdomain pattern. Nexus manages both the Cloudflare Tunnel and its DNS record.
    link: /guide/custom-domains
  - title: CLI and SDK
    details: Run the same setup from a project config or embed the session in your own development tool.
    link: /guide/automation
---

## Try it with a running app

With `cloudflared` installed and a service listening on port 5173:

~~~sh
npx flash-nexus --mode quick --url http://127.0.0.1:5173
~~~

Open the `public` URL printed in the terminal. When you are done, press **Ctrl+C**. Nexus closes the preview and leaves your existing server running.
