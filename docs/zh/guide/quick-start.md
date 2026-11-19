---
description: 使用 flash-nexus 和 Cloudflare Quick Tunnel 分享正在运行的本地服务。
---

# 快速开始

先用临时地址跑通一次，无需 Cloudflare 账号、API Token 或自有域名。

## 1. 检查工具

使用 **Node.js 22.14 或以上版本**，并安装 [cloudflared](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/)。

~~~sh
node --version
cloudflared --version
~~~

如果 `cloudflared` 不在 PATH 上，可以用 [`FLASH_NEXUS_CLOUDFLARED_PATH`](../reference/environment) 指定可执行文件路径。

## 2. 开启预览

先启动应用或 API，再运行以下命令。将端口替换成服务实际监听的端口。

~~~sh
npx flash-nexus --mode quick --url http://127.0.0.1:5173
~~~

终端会输出 `local` 和 `public` 两个地址。打开或分享 `public` 对应的 `trycloudflare.com` 地址。链接使用期间，保持 Nexus 进程运行。

::: tip 让 Nexus 启动服务
如果项目目录有可运行的 dev 脚本，可以试试 `npx flash-nexus --mode quick`。也可以在[项目配置](./configuration)里显式指定启动命令。
:::

## 3. 结束预览

按 **Ctrl+C**，等待清理完成。通过 `--url` 接入的服务会继续运行。下次开启临时预览时会得到新的公网地址。

[配置自有域名](./custom-domains) · [设置项目启动命令](./local-servers)
