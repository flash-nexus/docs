---
description: 独立使用 flash-tunnel 接入已有域名，或使用 flash-nexus 自动管理 DNS。
---
# 自定义域名

接入自己的域名有两种方式：

- **独立使用 flash-tunnel：** 先由其他 DNS 工作流准备记录，再传入已有域名。
- **使用 flash-nexus：** 一次配置 Zone 和命名规则，由 Nexus 组合 `flash-dns` 与 `flash-tunnel`，并清理本次会话创建的资源。

## 独立使用 flash-tunnel

独立 Tunnel 命令不会创建或删除 DNS 记录。可以先用 `flash-dns` 或其他 DNS 工作流准备域名，再启动 Tunnel：

```sh
npx flash-tunnel --mode named --hostname preview.example.com --url http://127.0.0.1:4317
```

将凭据放入环境变量：

```sh
export CLOUDFLARE_API_TOKEN='your_token_here'
export CLOUDFLARE_ACCOUNT_ID='your_account_id'
```

Token 只从环境变量读取。不要将它写入项目配置或 Git 历史。

## 使用 flash-nexus 管理 DNS

如果同一次会话需要同时创建 DNS 记录和 Tunnel，使用 `flash-nexus.config.ts`：

```ts
import { defineConfig } from 'flash-nexus'

export default defineConfig({
  tunnel: {
    mode: 'named',
    hostname: 'preview.example.com'
  },
  dns: {
    zone: 'example.com'
  }
})
```

然后执行：

```sh
npx flash-nexus
```

Nexus 会加载 `.env`，通过 `flash-tunnel` 创建 Tunnel，通过 `flash-dns` 创建 DNS 记录，并在清理时删除本次会话创建的资源。

## DNS 权限

集成工作流需要一个同时满足两个 provider 要求的 Cloudflare Custom token：

| 权限 | 用途 |
| --- | --- |
| Account · Cloudflare Tunnel · Edit | 创建和删除 named Tunnel，配置 ingress |
| Zone · DNS · Edit | 创建和删除会话归属的 CNAME 记录 |
| Zone · Zone · Read | 查询和校验 DNS Zone |

具体权限取决于选择的 provider 工作流。请将 Token 限定在项目使用的账号和 Zone 范围内。

## 子域名模板

独立使用 Tunnel 时，直接通过 `--hostname` 传入完整域名。使用 Nexus 管理的工作流时，将 Zone 和命名规则写入 `flash-nexus.config.ts`，由 `flash-dns` 在 Tunnel 启动前生成并校验主机名。

```ts
export default defineConfig({
  dns: {
    zone: 'example.com',
    naming: { template: 'fd-{project}.example.com' }
  }
})
```

## 命名和冲突

将 `zone`、`hostname` 或命名规则写入 `flash-nexus.config.ts`。Cloudflare 凭据放入 `.env` 或环境变量。`flash-dns` 会记录归属信息，帮助会话检测冲突，并只删除自己拥有的记录。
