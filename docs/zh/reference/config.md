---
description: flash-tunnel.ts 支持的字段，以及 Tunnel 与 DNS 配置的职责边界。
---
# 配置项参考

从 `flash-tunnel` 导入 `defineConfig` 可以获得编辑器补全。独立使用时，`flash-tunnel.ts` 只描述本地服务和 Tunnel 设置；需要 DNS 编排时，使用 `flash-nexus.config.ts`。

## 本地服务

| 字段 | 类型 | 默认值 / 行为 |
| --- | --- | --- |
| `server.command` | `string` | 自动识别启动脚本 |
| `server.cwd` | `string` | 已加载配置时为配置目录 |
| `server.port` | `number` | 从脚本、框架或服务模式中探测 |
| `server.url` | `string` | 要连接的已有服务地址 |
| `server.dir` | `string` | 要托管的静态目录 |
| `server.timeout` | `number` | `60000` 毫秒 |

`command`、`url`、`dir` 三者选择其一。相对路径以配置目录为基准。

## Tunnel

| 字段 | 类型 | 默认值 / 行为 |
| --- | --- | --- |
| `tunnel.provider` | `string` | `'cloudflare'` |
| `tunnel.mode` | `'quick' \| 'named'` | 独立 flash-tunnel 默认为 `'quick'` |
| `tunnel.name` | `string` | 根据项目目录生成 |
| `tunnel.hostname` | `string` | named 模式下已经存在的域名 |

Quick 模式返回随机的 `trycloudflare.com` 地址。Named 模式要求 DNS 已经将域名指向对应 Tunnel。

## Cloudflare 选项

非敏感的 Tunnel 选项保留在 provider 配置中：

| 字段 | 类型 | 默认值 / 行为 |
| --- | --- | --- |
| `providers.cloudflare.cloudflaredPath` | `string` | `'cloudflared'` |
| `providers.cloudflare.originRequest.httpHostHeader` | `string` | 可选，覆盖源站 Host 请求头 |
| `providers.cloudflare.originRequest.noTLSVerify` | `boolean` | 可选，覆盖源站 TLS 验证行为 |

Cloudflare 凭据不属于配置字段，适配器从环境变量读取：

```env
CLOUDFLARE_API_TOKEN=...
CLOUDFLARE_ACCOUNT_ID=...
```

当 DNS 工作流需要跳过按名称查找 Zone 时，DNS 适配器会读取 `CLOUDFLARE_ZONE_ID`。它属于 `flash-dns` 或 `flash-nexus`，不属于 `flash-tunnel.ts`。

## 示例

```ts
import { defineConfig } from 'flash-tunnel'

export default defineConfig({
  server: { url: 'http://127.0.0.1:3000' },
  tunnel: { mode: 'quick' }
})
```

完整的 named 工作流见[Flash Nexus 整体架构](../guide/ecosystem)和[自定义域名](../guide/custom-domains)。

字段定义维护在 [flash-tunnel 源码仓库](https://github.com/flash-nexus/flash-tunnel/tree/main/packages/config)和 [flash-nexus 配置](https://github.com/flash-nexus/flash-nexus)中。
