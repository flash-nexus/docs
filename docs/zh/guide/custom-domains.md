---
description: 使用自己的 Cloudflare 域名，由 flash-nexus 管理对应的 DNS 记录。
---

# 使用自己的域名

命名模式会创建一个 Cloudflare Tunnel 和一条 CNAME 记录。DNS 记录归预览会话管理，正常结束时会随会话一起清理。

## 1. 配置 Cloudflare 凭据

在工作目录创建 `.env`。CI 中可从密钥管理器注入相同的值。

~~~dotenv
CLOUDFLARE_API_TOKEN=your-token
CLOUDFLARE_ACCOUNT_ID=your-account-id
# 可选：跳过按名称查询 zone
# CLOUDFLARE_ZONE_ID=your-zone-id
~~~

把令牌限制在使用的账号和 zone。授予账号的 **Cloudflare Tunnel Write**、zone 的 **DNS Write**，以及按名称查询 zone 时使用的 **Zone Read**。权限细节见 [Cloudflare API 配置指南](https://developers.cloudflare.com/tunnel/get-started/)。不要把令牌提交到 Git。

::: info Zone 和 Zone ID
Zone 是 `example.com` 这样的域名，`CLOUDFLARE_ZONE_ID` 是 Cloudflare 分配的标识。即使提供了 ID，也要通过 `--zone`、`FLASH_NEXUS_ZONE` 或配置文件指定域名。
:::

## 2. 选择主机名

::: code-group

~~~sh [固定主机名]
npx flash-nexus --mode named --url http://127.0.0.1:5173 --zone example.com --hostname preview.example.com
~~~

~~~sh [随机后缀]
npx flash-nexus --mode named --url http://127.0.0.1:5173 --zone example.com --subdomain "preview-{rand:4}"
~~~

:::

任选一种命名方式。`--hostname` 接收 zone 下的完整主机名，`--subdomain` 接收 `demo` 或 `preview-{rand:4}` 这样的单段子域名。上面的模板会生成类似 `preview-k8p3.example.com` 的地址。切换为随机子域名时，如果环境变量已有完整主机名，先去掉它。

如果希望根据项目名生成固定形式的地址，可用 [`dns.naming.template`](../reference/config#dns)，例如 `preview-{project}.example.com`。

## 3. 保存下来，下次直接启动

把 `FLASH_NEXUS_MODE=named`、`FLASH_NEXUS_ZONE` 和 `FLASH_NEXUS_HOSTNAME` 或 `FLASH_NEXUS_SUBDOMAIN` 写入 `.env`，再把本地启动命令或本地地址写入 [`flash-nexus.config.json`](./configuration)。之后在项目目录直接运行 `npx flash-nexus`。

::: tip 使用尚未占用的主机名
如果同名 DNS 记录不受 Nexus 管理，或属于另一个会话，Nexus 会报告冲突。可以换一个子域名，也可以先向该记录的所有者确认用途。
:::

详见[环境变量](../reference/environment)和[会话清理](./cleanup)。
