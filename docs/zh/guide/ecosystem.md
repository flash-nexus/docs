---
description: 查看 Nexus CLI 和 SDK 如何依赖 flash-tunnel 与 flash-dns。
---

# 生态与依赖关系

开启预览时，直接使用 **flash-nexus**。它组合了两个职责独立的底层库。

~~~mermaid
flowchart TD
    accTitle: Flash Nexus 软件包依赖关系
    accDescr: Nexus 使用 flash-tunnel 管理隧道，命名模式下使用 flash-dns 管理记录。
    Nexus["flash-nexus"] -->|管理隧道| Tunnel["flash-tunnel"]
    Nexus -->|命名模式下管理 DNS| DNS["flash-dns"]
~~~

| 项目 | 职责 |
| --- | --- |
| [flash-nexus](https://github.com/flash-nexus/flash-nexus) | 通过 [CLI](./quick-start) 或 [SDK](./automation) 编排完整的预览会话。 |
| [flash-tunnel](https://github.com/flash-nexus/flash-tunnel) | 启停 Cloudflare Tunnel，管理 `cloudflared`。 |
| [flash-dns](https://github.com/flash-nexus/flash-dns) | 解析主机名，创建、更新和删除有归属的 DNS 记录。 |

## 命名模式的流程

1. Nexus 确定本地服务、DNS zone 和公网主机名。
2. 调用 `flash-tunnel` 创建 Cloudflare Tunnel，取得隧道目标地址。
3. 调用 `flash-dns` 创建指向该隧道的 CNAME，记录归当前会话管理。
4. 预览就绪后输出公网地址。会话退出时，先移除 DNS 记录，再回收隧道。

临时模式直接使用 Cloudflare 分配的地址，省去 DNS 步骤。详见[自定义域名](./custom-domains)和[会话清理](./cleanup)。

## flash-tunnel

只需要隧道时可以单独运行：

~~~sh
npx flash-tunnel --mode quick --url http://127.0.0.1:5173
~~~

它的命名模式接收完整主机名，该主机名的 DNS 由其他流程管理。更多用法见 [flash-tunnel README](https://github.com/flash-nexus/flash-tunnel#readme)。

## flash-dns

用 `flash-dns plan` 查看配置对应的变更，再用 `flash-dns ensure` 应用符合归属规则的变更。命名、DNS 类型和服务商配置见 [flash-dns README](https://github.com/flash-nexus/flash-dns#readme)。
