---
description: 了解独立 Tunnel 的清理，以及 flash-nexus 如何协调 DNS 清理。
---
# 清理与恢复

`flash-tunnel` 负责自己创建的 Tunnel 和本地进程，不负责 DNS 记录。当 DNS 与 Tunnel 由 `flash-nexus` 共同管理时，Nexus 会把 `flash-dns` 记录纳入同一个会话生命周期。

## 独立使用 flash-tunnel

独立 `flash-tunnel` 正常退出时会：

1. 停止 `cloudflared`。
2. 等待 Tunnel 会话完成清理钩子。
3. 停止由 flash-tunnel 启动的本地服务进程。
4. 删除本地会话状态并释放项目锁。

Quick 模式没有账号内的 DNS 记录。通过 `--url` 连接的已有服务仍由你自己停止。

## flash-nexus 的清理

由 `flash-nexus` 启动的 named 会话会跨两个 provider 清理：

1. 停止会话拥有的客户端或本地服务进程。
2. 通过 `flash-dns` 删除会话归属的 DNS 记录。
3. 通过 `flash-tunnel` 停止并删除 named Tunnel。
4. 删除会话状态。

DNS 记录中会保存会话归属信息，因此清理时可以拒绝删除其他工作流拥有的记录。

## 意外中断

| 情况 | 行为 |
| --- | --- |
| `Ctrl+C` | 清理后以 `130` 退出 |
| `SIGTERM` / `SIGHUP` | 清理后以 `143` 退出 |
| 清理时再次按 `Ctrl+C` | 立即退出，保留状态供恢复 |
| 主进程被杀或终端关闭 | 独立 watchdog 尝试回收 |
| 清理失败 | 保留状态以便重试 |

云端清理需要网络和有效凭据。意外中断后，请检查记录的会话状态。

## 手动清理

```sh
npx flash-tunnel clean
npx flash-tunnel clean --stale --dry-run
npx flash-tunnel clean --stale
```

对于由 `flash-dns` 管理的 DNS 记录，删除前先查看 plan 或记录归属。确认剩余云资源之前，不要直接删除本地状态。

## 保留云端资源

独立 Tunnel 和 Nexus 会话都可以配置保留云端资源。保留 Tunnel 或 DNS 记录不会让本地服务或 `cloudflared` 进程持续运行。

Cloudflare 凭据只从环境变量读取，不会保存到项目配置、会话状态或命令参数中。
