---
description: 了解 flash-nexus 的用途，以及临时预览和命名预览的区别。
---

# 什么是 flash-nexus？

**flash-nexus** 为你电脑上的服务提供一个公网地址，可以用来分享开发中的页面、联调 webhook，或者从其他设备访问本地 API。服务仍然运行在你的电脑上。

## 选择预览模式

| 模式 | 公网地址 | 所需条件 |
| --- | --- | --- |
| `quick` | 临时 `trycloudflare.com` 地址 | Node.js 和 `cloudflared` |
| `named` | 自有 Cloudflare zone 下的主机名 | 以上工具，以及 Cloudflare zone 和 API 凭据 |

第一次可以先用 `quick`。需要固定地址，或希望每次预览的地址都易于识别时，用 `named`。

## Nexus 负责什么

Nexus 读取项目配置，启动或连接本地服务，并开启隧道。命名模式下，它还会创建指向隧道的 DNS 记录。会话结束时，Nexus 清理自己管理的资源。

[flash-tunnel] 和 [flash-dns] 提供底层操作。只需要管理隧道或 DNS 时，也可以单独用对应的库。

[启动临时预览](./quick-start) · [查看依赖关系](./ecosystem)

[flash-tunnel]: ./ecosystem#flash-tunnel
[flash-dns]: ./ecosystem#flash-dns
