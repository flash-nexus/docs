---
description: Save a project configuration and run flash-nexus without repeating flags.
---

# Set up a project configuration

If you run the same preview regularly, save the local server and preview mode in `flash-nexus.config.json` or `flash-nexus.config.ts` in the project directory. You can then run `npx flash-nexus` without extra flags.

## Pick a file format

This example asks Nexus to start an app with `pnpm dev` and share port 5173 through a temporary URL.

::: code-group

~~~json [flash-nexus.config.json]
{
  "server": {
    "command": "pnpm dev",
    "port": 5173
  },
  "tunnel": {
    "mode": "quick"
  }
}
~~~

~~~ts [flash-nexus.config.ts]
import { defineConfig } from 'flash-nexus'

export default defineConfig({
  server: { command: 'pnpm dev', port: 5173 },
  tunnel: { mode: 'quick' },
})
~~~

:::

Use JSON for plain data or TypeScript for editor completion. To reuse a running service, replace the server block with `{ "url": "http://127.0.0.1:5173" }`.

## Keep runtime values in .env

Put Cloudflare credentials and machine-specific values in `.env`. Use `--env-file .env.preview` to select another file from the project directory.

For settings that support overrides, the general order is **CLI arguments → Nexus environment variables → supported lower-level fallbacks → config file → defaults**. See [the environment reference](../reference/environment) for the exact names and [the config reference](../reference/config) for structured options.

::: tip Check the config file
`npx flash-nexus --dry-run` prints the configuration loaded by the config loader without opening a tunnel. It does not show the final runtime result after every CLI or environment override, nor does it validate Cloudflare credentials.
:::

[Configure a named preview](./custom-domains) · [See how local servers are selected](./local-servers)
