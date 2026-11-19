---
description: flash-nexus 会话结束时清理哪些资源，以及退出被中断后如何排查。
---

# 停止预览与清理

在终端按 **Ctrl+C**，或通过 [SDK](./automation) 调用 `await preview.stop()`。等待清理完成再关闭终端。

## 会清理什么

| 资源 | 正常关闭时的行为 |
| --- | --- |
| `cloudflared` 进程 | 随预览一起停止。 |
| 命名模式的 DNS 记录 | 先移除归当前会话管理的 CNAME。 |
| 命名模式的 Cloudflare Tunnel | 回收本次预览创建的隧道。 |
| Nexus 启动的本地服务 | 随预览一起停止。 |
| 通过 `--url` 或 `server.url` 接入的本地服务 | 继续运行。 |

临时模式不创建账号下的 DNS 记录。如果设置了 [`tunnel.keep`](../reference/config#tunnel)，云端资源会保留，但本地隧道进程仍然停止，公网地址不会继续提供服务。

## 如果关闭过程被中断

启用状态持久化时，Tunnel 核心默认把会话状态和日志保存在 `~/.flash-tunnel/`。清理未完成时，先从状态和终端输出确认是哪次会话，再去 Cloudflare 控制台查找对应的隧道和 CNAME，确认后再处理残留资源。

::: info 确认 DNS 归属
一次命名预览只管理自己的 DNS 记录。同一个主机名也可能被其他会话或流程使用，修改前先确认记录的归属。
:::

自动化脚本应当[在 finally 中关闭 SDK 会话](./automation)，并让 Cloudflare 凭据保持可用，直到清理完成。
