---
description: 保存项目配置，下次直接运行 flash-nexus。
---

# 配置项目

如果经常用同一种方式预览，在项目目录创建 `flash-nexus.config.json` 或 `flash-nexus.config.ts`，保存本地服务和预览模式。之后直接运行 `npx flash-nexus` 即可。

## 选择配置文件格式

下面的配置会执行 `pnpm dev`，再用临时地址分享本地 5173 端口。

::: code-group

~~~json [flash-nexus.config.json]
{
  "server": {
    "command": "pnpm dev",
    "port": 5173
  },
  "tunnel": {
    "mode": "quick"
  }
}
~~~

~~~ts [flash-nexus.config.ts]
import { defineConfig } from 'flash-nexus'

export default defineConfig({
  server: { command: 'pnpm dev', port: 5173 },
  tunnel: { mode: 'quick' },
})
~~~

:::

只保存数据可以用 JSON，需要编辑器类型提示可以用 TypeScript。如果本地服务已经启动，把 `server` 改为 `{ "url": "http://127.0.0.1:5173" }`。

## 将运行环境配置放到 .env

Cloudflare 凭据和机器相关的值放在 `.env`。如需读取项目目录下的其他文件，可传入 `--env-file .env.preview`。

支持覆盖的同名字段通常按 **CLI 参数 → Nexus 环境变量 → 对应的底层环境变量 → 项目配置 → 默认值** 取值。变量名和回退顺序见[环境变量参考](../reference/environment)，可写入文件的字段见[配置参考](../reference/config)。

::: tip 查看加载到的配置
`npx flash-nexus --dry-run` 会打印配置加载器读到的内容，不会启动隧道。它不会展示应用全部 CLI 和环境变量覆盖后的最终值，也不会校验 Cloudflare 凭据。
:::

[设置命名预览](./custom-domains) · [了解本地服务如何选择](./local-servers)
