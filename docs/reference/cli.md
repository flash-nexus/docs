---
description: Supported flash-nexus CLI flags for starting a preview or inspecting its configuration.
---

# CLI reference

~~~sh
npx flash-nexus [options]
~~~

Use `pnpm exec flash-nexus` when the CLI is installed as a project dependency. See [the config reference](./config) for settings such as the startup command and session timeout.

| Option | Purpose |
| --- | --- |
| `--cwd <dir>` | Project directory. Defaults to the current working directory. |
| `--env-file <path>` | Select an environment file. Relative paths use the project directory. |
| `--url <url>` | Attach to an already running local HTTP service. |
| `--mode <mode>` | `quick` for a temporary URL or `named` for your own Cloudflare zone. The default is `named`. |
| `--zone <domain>` | DNS zone name for a named preview, e.g. `example.com`. |
| `--hostname <fqdn>` | Full public hostname for a named preview. |
| `--subdomain <pattern>` | A single label, or a template such as `preview-{rand:4}`, combined with the zone. |
| `--dry-run` | Print the config object loaded from disk without starting a session. |
| `-h`, `--help` | List options. |
| `-v`, `--version` | Print the installed CLI version. |

## Examples

~~~sh
npx flash-nexus --mode quick --url http://127.0.0.1:5173
npx flash-nexus --cwd apps/web --env-file .env.preview
npx flash-nexus --cwd apps/web --dry-run
~~~

After startup the CLI prints lines prefixed with `local` and `public`. It stays alive while the preview is running. In a terminal, press **Ctrl+C** to close it. For machine-readable access to the URLs and shutdown, use [the SDK](../guide/automation).

::: info About dry-run
The output is the loaded config object, not a resolved runtime plan. CLI and environment overrides may not appear in it.
:::

[Preview with a custom domain](../guide/custom-domains) · [Environment variables](./environment)
