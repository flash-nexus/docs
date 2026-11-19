---
description: 让 flash-nexus 连接已运行的服务、启动项目命令或直接托管静态产物。
---

# 接入本地服务

可以连接已经运行的服务，让 Nexus 执行项目命令，也可以直接预览静态产物。

## 接入已运行的服务

~~~sh
npx flash-nexus --mode quick --url http://127.0.0.1:3000
~~~

可以把地址保存为配置文件中的 `server.url`。关闭预览后，原服务会继续运行。如果端口已经有进程监听，先确认它正是你要分享的服务。

## 由 Nexus 启动命令

保存为 `flash-nexus.config.json`：

~~~json
{
  "server": { "command": "pnpm dev", "port": 5173 },
  "tunnel": { "mode": "quick" }
}
~~~

`server.port` 告诉 Nexus 去哪个端口等服务。实际监听端口由应用自身配置或启动命令决定。会话结束时，Nexus 会停止自己启动的子进程。

没有显式指定本地服务时，Nexus 会通过 Tunnel 核心查找可运行的项目脚本或静态产物。monorepo 可以直接指定应用目录：

~~~sh
npx flash-nexus --cwd apps/web --mode quick
~~~

## 预览静态产物

先构建应用，再把 `server.dir` 指向包含 `index.html` 的目录。`server.command`、`server.url` 和 `server.dir` 三选一。

~~~json
{
  "server": { "dir": "dist", "port": 4173 },
  "tunnel": { "mode": "quick" }
}
~~~

## 本地服务拒绝公网主机名时

Nexus 通过 Tunnel 核心启动受支持的 Vite 进程时，可以附加允许访问的主机名。如果服务是你自己启动的，在 [Vite 的 allowedHosts](https://vite.dev/config/server-options#server-allowedhosts) 或对应框架配置中加入预览主机名。

详见[故障排查](./troubleshooting)和[服务配置字段](../reference/config#server)。
