---
description: flash-nexus.config 中的本地服务、隧道与 DNS 字段。
---

# 配置参考

在项目目录创建 `flash-nexus.config.json` 或 `flash-nexus.config.ts`。TypeScript 配置可以从 `flash-nexus` 导入 `defineConfig`。见[可复制的配置示例](../guide/configuration)。

## 本地服务 {#server}

| 字段 | 用途 |
| --- | --- |
| `server.url` | 接入已运行的本地服务。 |
| `server.command` | 启动 `pnpm dev` 一类的命令。 |
| `server.dir` | 预览包含 `index.html` 的静态目录。 |
| `server.cwd` | 本地服务的工作目录。 |
| `server.port` | 监听或等待的端口，不会修改应用自己的端口配置。 |
| `server.timeout` | 等待本地服务就绪的毫秒数。 |

从 `url`、`command`、`dir` 中选一个。都没有指定时，底层可以识别项目脚本或静态产物。`--url` 会覆盖配置文件中的本地服务来源。

## 隧道 {#tunnel}

| 字段 | 用途 |
| --- | --- |
| `tunnel.mode` | `quick` 或 `named`。默认是 `named`。 |
| `tunnel.name` | Cloudflare Tunnel 资源的名称，与公网主机名分别设置。 |
| `tunnel.hostname` | 完整公网主机名。没有设置 `dns.hostname` 时可以用这个字段。 |
| `tunnel.subdomain` | 单段子域名或随机模板，与 zone 拼接。 |
| `tunnel.cloudflaredPath` | `cloudflared` 不在 PATH 时使用的可执行文件路径。 |
| `tunnel.originRequest.httpHostHeader` | 覆盖发送到本地服务的 Host 请求头。 |
| `tunnel.originRequest.noTLSVerify` | 有明确需求时，禁用对 HTTPS 源站证书的校验。 |
| `tunnel.autoStopAfterMs` | 经过指定毫秒数后停止会话。 |
| `tunnel.verifyTimeoutMs` | 等待公网地址通过就绪检查的时限。 |
| `tunnel.keep` | 本地会话关闭时保留云端资源。 |
| `tunnel.persist` | 保存本地会话状态，默认是 `true`。 |

## DNS {#dns}

以下字段用于命名模式。Nexus 创建指向隧道目标的 CNAME 记录。

| 字段 | 用途 |
| --- | --- |
| `dns.zone` | DNS zone 名称，例如 `example.com`。也可用 `tunnel.zone`。 |
| `dns.hostname` | 完整公网主机名，在配置文件中优先于 `tunnel.hostname`。 |
| `dns.naming.template` | 未选用完整主机名或子域名时使用的模板，例如 `preview-{project}.example.com`。 |
| `dns.variables` | DNS 模板中占位符的取值。 |
| `project.name` | 可选项目名称，用于 `{project}`。未设置时，可以使用包名或目录名。 |

需要单段随机子域名时，把 `tunnel.subdomain` 设为 `preview-{rand:4}`。需要按项目名生成固定形式的地址时，用 `dns.naming.template`。详见[自定义域名指南](../guide/custom-domains)。

Cloudflare 凭据通过[环境变量](./environment)提供。完整 TypeScript 结构见[配置类型](https://github.com/flash-nexus/flash-nexus/blob/main/packages/types/src/index.ts)。
