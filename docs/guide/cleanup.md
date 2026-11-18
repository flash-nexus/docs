---
description: Understand standalone Tunnel cleanup and how flash-nexus coordinates DNS cleanup.
---
# Cleanup and recovery

`flash-tunnel` owns the Tunnel and the local processes it starts. It does not own DNS records. When DNS and Tunnel are managed together, `flash-nexus` adds the `flash-dns` record to the same session lifecycle.

## Standalone flash-tunnel

On normal exit, standalone `flash-tunnel`:

1. Stops `cloudflared`.
2. Waits for the Tunnel session to finish its cleanup hooks.
3. Stops the local server process started by flash-tunnel.
4. Removes local session state and releases the project lock.

Quick mode has no account-owned DNS record. A server attached through `--url` remains yours to stop.

## flash-nexus cleanup

When a named session is started by `flash-nexus`, cleanup runs across both providers:

1. Stop the client or local server processes owned by the session.
2. Remove the session-owned DNS record through `flash-dns`.
3. Stop and deprovision the named Tunnel through `flash-tunnel`.
4. Remove session state.

The session owner is recorded in the DNS record metadata, so cleanup can refuse to remove a record owned by another workflow.

## Interrupted sessions

| Situation | Behavior |
| --- | --- |
| `Ctrl+C` | Clean up, then exit with `130` |
| `SIGTERM` / `SIGHUP` | Clean up, then exit with `143` |
| Second `Ctrl+C` during shutdown | Exit immediately; retain state for recovery |
| Main process killed or terminal closed | A detached watchdog attempts recovery |
| Cleanup failure | Retain state so cleanup can be retried |

Cloud cleanup needs network access and valid credentials. Check the recorded session after an interrupted run.

## Manual cleanup

```sh
npx flash-tunnel clean
npx flash-tunnel clean --stale --dry-run
npx flash-tunnel clean --stale
```

For DNS records managed by `flash-dns`, inspect the plan or record ownership before removing anything. Avoid deleting local state until you understand which cloud resources remain.

## Keeping cloud resources

Standalone Tunnel sessions and Nexus sessions can be configured to keep their cloud resources. A retained Tunnel or DNS record does not keep your local server or `cloudflared` process running.

Cloudflare credentials are read from the environment and are not stored in project configuration, session state, or command arguments.
