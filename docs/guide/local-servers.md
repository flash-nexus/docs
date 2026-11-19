---
description: Connect flash-nexus to an existing service, run a command, or serve static output.
---

# Connect a local service

Choose how Nexus should obtain the origin. You can connect to a process you already started, let Nexus run the project command, or serve static output directly.

## Connect to a running server

~~~sh
npx flash-nexus --mode quick --url http://127.0.0.1:3000
~~~

Use `server.url` in the config file to save that address. Closing the preview leaves the server running. If another process already owns the port, use this method only when it is the intended origin.

## Start a command

Save this as `flash-nexus.config.json`:

~~~json
{
  "server": { "command": "pnpm dev", "port": 5173 },
  "tunnel": { "mode": "quick" }
}
~~~

`server.port` tells Nexus where to expect the app. Set the actual listening port in the app's configuration or in its command. When Nexus starts a child process, it stops that process when the session ends.

Without an explicit origin, Nexus looks for a runnable project script or built static output using the Tunnel core. In a monorepo, point it at the app directory:

~~~sh
npx flash-nexus --cwd apps/web --mode quick
~~~

## Serve a static directory

Build the app first, then set `server.dir` to a directory containing `index.html`. Set only one of `server.command`, `server.url`, or `server.dir`.

~~~json
{
  "server": { "dir": "dist", "port": 4173 },
  "tunnel": { "mode": "quick" }
}
~~~

## If the origin rejects the public hostname

For supported Vite processes it starts, Nexus can pass an additional allowed host through the Tunnel core. If you run the server yourself, configure the preview hostname in [Vite's allowed hosts](https://vite.dev/config/server-options#server-allowedhosts) or the equivalent framework setting.

See [startup troubleshooting](./troubleshooting) and [server fields](../reference/config#server).
