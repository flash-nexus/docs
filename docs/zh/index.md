---
layout: home
title: 分享本地服务
hero:
  name: flash-nexus
  text: 分享本地服务，<br>一条命令开启预览。
  tagline: 启动或连接本地服务，用 Cloudflare 临时地址分享，也可以使用自己的域名。
  actions:
    - theme: brand
      text: 快速开始
      link: /zh/guide/quick-start
    - theme: alt
      text: 了解生态与依赖
      link: /zh/guide/ecosystem
features:
  - title: 临时预览
    details: 无需配置 DNS，用临时 trycloudflare.com 地址分享本地应用或 API。
    link: /zh/guide/quick-start
  - title: 自定义域名
    details: 使用固定主机名或子域名模板，由 Nexus 管理对应的 Cloudflare Tunnel 和 DNS 记录。
    link: /zh/guide/custom-domains
  - title: CLI 与 SDK
    details: 用项目配置重复启动同一套流程，也可以将预览会话接入自己的开发工具。
    link: /zh/guide/automation
---

## 试一下

安装 `cloudflared`，并确保本地服务正在监听 5173 端口：

~~~sh
npx flash-nexus --mode quick --url http://127.0.0.1:5173
~~~

打开终端输出的 `public` 地址。分享结束后按 **Ctrl+C** 关闭预览，原本已运行的本地服务会继续运行。
