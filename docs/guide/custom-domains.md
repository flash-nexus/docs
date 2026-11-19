---
description: Use a hostname in your Cloudflare zone and let flash-nexus manage its DNS record.
---

# Use your own domain

In named mode, Nexus creates a Cloudflare Tunnel and a CNAME in your zone. The CNAME is tied to the preview session and is removed during normal cleanup.

## 1. Provide Cloudflare credentials

Add a `.env` file in the working directory. In CI, inject the same values through the job's secret store.

~~~dotenv
CLOUDFLARE_API_TOKEN=your-token
CLOUDFLARE_ACCOUNT_ID=your-account-id
# Optional: skip the zone-name lookup
# CLOUDFLARE_ZONE_ID=your-zone-id
~~~

Scope the token to the account and zone you use. Grant **Cloudflare Tunnel Write** on the account, **DNS Write** on the zone, and **Zone Read** if Nexus needs to look up the zone by name. See [Cloudflare's API setup](https://developers.cloudflare.com/tunnel/get-started/). Keep the token out of version control.

::: info Zone and Zone ID
The zone is a domain such as `example.com`. `CLOUDFLARE_ZONE_ID` is Cloudflare's identifier for that zone. Even when you supply an ID, set the zone name through `--zone`, `FLASH_NEXUS_ZONE`, or the config file.
:::

## 2. Choose a hostname

::: code-group

~~~sh [Fixed hostname]
npx flash-nexus --mode named --url http://127.0.0.1:5173 --zone example.com --hostname preview.example.com
~~~

~~~sh [Random suffix]
npx flash-nexus --mode named --url http://127.0.0.1:5173 --zone example.com --subdomain "preview-{rand:4}"
~~~

:::

Choose one naming method. `--hostname` takes a full name inside the zone. `--subdomain` takes a single label such as `demo` or `preview-{rand:4}`. The template above generates a name like `preview-k8p3.example.com`. If you set another full hostname in the environment, remove it when you want to use the subdomain option.

For repeatable naming by project, use [`dns.naming.template`](../reference/config#dns) with a pattern such as `preview-{project}.example.com`.

## 3. Save it for next time

Put `FLASH_NEXUS_MODE=named`, `FLASH_NEXUS_ZONE`, and either `FLASH_NEXUS_HOSTNAME` or `FLASH_NEXUS_SUBDOMAIN` in `.env`. Save the startup command or local URL in [`flash-nexus.config.json`](./configuration). After that, run `npx flash-nexus` from the project directory.

::: tip Choose an unused hostname
If an existing DNS record is unmanaged or belongs to another workflow, Nexus reports a conflict. Choose a different label, or review that record with its owner before changing it.
:::

See [environment variables](../reference/environment) and [session cleanup](./cleanup).
