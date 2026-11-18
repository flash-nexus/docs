---
description: 了解 flash-nexus、flash-dns 和 flash-tunnel 的职责关系。
---
# Flash Nexus 整体架构

Flash Nexus 将预览编排与底层 provider 操作拆开。根据需要选择对应的层：

```text
flash-nexus       编排一次完整的预览会话
├── flash-dns     管理 DNS 记录和资源归属
└── flash-tunnel  管理 Tunnel 适配器和 cloudflared
```

## flash-tunnel

`flash-tunnel` 是独立的 Tunnel 包。它接收本地 origin，启动 `cloudflared`，等待公网地址，并停止本次会话创建的 Tunnel。

适合以下场景：

- 只需要临时的 `trycloudflare.com` 地址；
- 域名和 DNS 记录已经存在；
- DNS 由其他系统管理，不希望 Tunnel 工具接管。

独立使用 named 模式时，`flash-tunnel` 要求域名已经解析到对应 Tunnel。它不会创建或删除 DNS 记录。

## flash-dns

`flash-dns` 是 DNS 包。它负责解析 Zone、生成主机名、协调记录，并记录资源归属，让会话只删除自己创建的记录。

如果只需要部署预览的 DNS 自动化，或者不需要 Tunnel，可以单独使用它。

## flash-nexus

`flash-nexus` 是集成编排层。它加载项目配置和环境变量，启动或连接本地服务，调用 `flash-tunnel` 创建 Tunnel，再调用 `flash-dns` 为 named 会话创建和清理 DNS 记录。

需要一条命令接管完整生命周期时使用它：

1. 加载 `flash-nexus.config.ts` 和 `.env`。
2. 启动或连接本地 origin。
3. 通过 `flash-tunnel` 创建 named Tunnel。
4. 通过 `flash-dns` 创建 DNS 记录。
5. 输出公网地址。
6. 退出时清理 DNS 记录和 Tunnel。

Cloudflare 凭据只放在环境变量中。请不要把 `CLOUDFLARE_API_TOKEN`、`CLOUDFLARE_ACCOUNT_ID` 和 `CLOUDFLARE_ZONE_ID` 写入项目配置或提交到代码库。

## 应该使用哪个命令？

| 需求 | 使用 |
| --- | --- |
| 临时公网地址 | `npx flash-tunnel --mode quick` |
| 已有域名的 Tunnel | `npx flash-tunnel --mode named --hostname preview.example.com` |
| 只做 DNS 记录自动化 | `npx flash-dns plan` / `npx flash-dns ensure` |
| 一条命令管理本地服务 DNS Tunnel 和清理 | `npx flash-nexus` |

移动端和设备调试场景可以使用 `flash-debug`。它基于 `flash-nexus` 增加 Inspector、日志上传 API、Flutter SDK，以及 ADB 或 HDC 客户端。
