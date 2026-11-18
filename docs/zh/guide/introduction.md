---
description: 了解 flash-tunnel 的用途、预览工作流程，以及何时使用 flash-nexus。
---
# 认识 flash-tunnel

**把本地 origin 变成可分享的公网预览。** `flash-tunnel` 启动或连接本地服务，通过 Cloudflare Tunnel 暴露公网，并负责 Tunnel 生命周期。

它是 [Flash Nexus 体系](./ecosystem)中的独立 Tunnel 层。DNS 自动化和完整会话编排分别由 `flash-dns` 与 `flash-nexus` 提供。

::: info 早期预览
flash-tunnel 目前处于 `0.x` 阶段，接口和行为可能调整。本文档以[项目仓库](https://github.com/flash-nexus/flash-tunnel)中的实现为依据。
:::

## 两种分享方式

| | Quick 模式 | Named 模式 |
| --- | --- | --- |
| 公网地址 | 随机 `*.trycloudflare.com` 地址 | 已经解析到 Tunnel 的域名 |
| Cloudflare API 凭据 | 不需要 | 需要 |
| DNS 管理 | 无 | 由独立的 DNS 工作流管理 |
| 适用场景 | 临时演示和联调 | 由其他流程管理的稳定域名 |
| 启动命令 | `npx flash-tunnel --mode quick` | `npx flash-tunnel --mode named --hostname preview.example.com` |

独立使用时默认是 **quick** 模式。已有域名时使用 `--mode named`。

## 一次运行会做什么？

1. 识别本地 origin，或连接已有地址。
2. 按选择的模式启动 `cloudflared`。
3. 等待公网地址可访问。
4. 打印公网地址和会话信息。
5. 退出时停止本次会话创建的 Tunnel 和本地进程。

`flash-tunnel` 不会创建或删除 DNS 记录。需要同时创建和清理 DNS 与 Tunnel 时，使用 `flash-nexus`。

## 环境要求

- Node.js **22.14 或更新版本**。
- `cloudflared` 可执行文件，位于 `PATH` 中，或通过配置显式指定。
- 一个本地项目、构建好的静态目录，或正在运行的 HTTP 服务。
- named 模式还需要 Cloudflare API Token，以及已经由 DNS 准备好的域名。

## 从这里开始

[创建第一个预览](./quick-start)、[了解整体架构](./ecosystem)，或在[命令行参考](../reference/cli)中查找参数。
