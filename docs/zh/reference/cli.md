---
description: flash-nexus 当前支持的预览启动和配置检查选项。
---

# 命令行参考

~~~sh
npx flash-nexus [options]
~~~

如果已经将 CLI 安装为项目依赖，可用 `pnpm exec flash-nexus`。启动命令和会话时限等设置见[配置参考](./config)。

| 选项 | 用途 |
| --- | --- |
| `--cwd <dir>` | 项目目录，默认是当前工作目录。 |
| `--env-file <path>` | 指定环境文件，相对路径以项目目录为基准。 |
| `--url <url>` | 接入已运行的本地 HTTP 服务。 |
| `--mode <mode>` | `quick` 使用临时地址，`named` 使用自己的 Cloudflare zone。默认是 `named`。 |
| `--zone <domain>` | 命名预览的 DNS zone 名称，例如 `example.com`。 |
| `--hostname <fqdn>` | 命名预览的完整公网主机名。 |
| `--subdomain <pattern>` | 单段子域名，或 `preview-{rand:4}` 这样的模板，会与 zone 拼接。 |
| `--dry-run` | 打印从文件加载的配置，不启动会话。 |
| `-h`、`--help` | 查看支持的选项。 |
| `-v`、`--version` | 查看当前 CLI 版本。 |

## 示例

~~~sh
npx flash-nexus --mode quick --url http://127.0.0.1:5173
npx flash-nexus --cwd apps/web --env-file .env.preview
npx flash-nexus --cwd apps/web --dry-run
~~~

启动后，CLI 会输出以 `local` 和 `public` 开头的地址，并在预览期间保持运行。在终端按 **Ctrl+C** 可关闭预览。脚本中需要直接读取地址或控制退出时，用 [SDK](../guide/automation)。

::: info 关于 dry-run
输出的是加载到的配置对象。CLI 参数和环境变量是在启动阶段覆盖的，不一定出现在这份输出中。
:::

[使用自定义域名](../guide/custom-domains) · [环境变量](./environment)
