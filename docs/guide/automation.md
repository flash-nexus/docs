---
description: Use the flash-nexus SDK to obtain a public URL and stop the preview from a script.
---

# SDK and automation

Use `@flash-nexus/sdk` when another program needs the preview URL or must stop the session itself. It uses the same tunnel and DNS workflow as the CLI.

## Run a check against the public URL

~~~sh
pnpm add -D @flash-nexus/sdk
~~~

The script below assumes your app is already listening on port 5173 and `cloudflared` is available. Save it as `check-preview.mjs`:

~~~js
import { startFlashNexus } from '@flash-nexus/sdk'

const preview = await startFlashNexus({
  localUrl: 'http://127.0.0.1:5173',
  mode: 'quick',
})

try {
  console.log(preview.publicUrl)
  const response = await fetch(preview.publicUrl, {
    signal: AbortSignal.timeout(10_000),
  })
  if (!response.ok)
    throw new Error('Preview returned HTTP ' + response.status)
} finally {
  await preview.stop()
}
~~~

~~~sh
node check-preview.mjs
~~~

## Session controls

| API | Use |
| --- | --- |
| `preview.localUrl` and `preview.publicUrl` | Read the origin and public addresses after startup. |
| `preview.sessionId` | Correlate logs or diagnostic output with the session. |
| `preview.exited` | Wait for the running service or tunnel to exit. |
| `await preview.stop()` | Close the preview and await cleanup. |

Pass `cwd` to select a project and `envFile` to select another environment file. You can also pass an explicit `config` object in place of loading the project config file. For example, use `config.tunnel.autoStopAfterMs` as a session time limit and still stop the session in `finally`.

In named mode, make the [Cloudflare credentials](./custom-domains) available through shutdown so the session can remove the resources it created. See [cleanup behavior](./cleanup).
