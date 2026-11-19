---
description: 通过 flash-nexus SDK 在脚本中读取预览地址，并在使用完毕后停止会话。
---

# SDK 与自动化

当脚本需要读取公网地址或主动停止预览时，用 `@flash-nexus/sdk`。它与 CLI 使用同一套隧道和 DNS 流程。

## 用公网地址跑一次检查

~~~sh
pnpm add -D @flash-nexus/sdk
~~~

以下脚本假设应用已经在监听 5173 端口，并且 `cloudflared` 可用。将它保存为 `check-preview.mjs`：

~~~js
import { startFlashNexus } from '@flash-nexus/sdk'

const preview = await startFlashNexus({
  localUrl: 'http://127.0.0.1:5173',
  mode: 'quick',
})

try {
  console.log(preview.publicUrl)
  const response = await fetch(preview.publicUrl, {
    signal: AbortSignal.timeout(10_000),
  })
  if (!response.ok)
    throw new Error('Preview returned HTTP ' + response.status)
} finally {
  await preview.stop()
}
~~~

~~~sh
node check-preview.mjs
~~~

## 会话接口

| API | 用途 |
| --- | --- |
| `preview.localUrl`、`preview.publicUrl` | 会话就绪后读取本地地址和公网地址。 |
| `preview.sessionId` | 在日志和排查信息中标识当前会话。 |
| `preview.exited` | 等待正在运行的服务或隧道退出。 |
| `await preview.stop()` | 停止预览并等待清理完成。 |

可通过 `cwd` 指定项目目录，通过 `envFile` 选择其他环境文件。也可以传入 `config` 对象代替从磁盘加载项目配置。例如用 `config.tunnel.autoStopAfterMs` 限制会话时长，同时在 `finally` 中主动关闭会话。

命名模式下，让 [Cloudflare 凭据](./custom-domains) 保持可用，直到会话清理完成。详见[清理行为](./cleanup)。
