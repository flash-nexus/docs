---
layout: home
title: 本地服务，即刻分享。
hero:
  name: flash-tunnel
  text: 本地服务，<br>即刻分享。
  tagline: 一条命令，把本地开发服务变成可分享的公网预览。自动识别、连接隧道，退出时自动清理。
  image:
    src: /logo.svg
    alt: flash-tunnel
  actions:
    - theme: brand
      text: 快速开始
      link: /zh/guide/quick-start
    - theme: alt
      text: 在 GitHub 上查看 ↗
      link: https://github.com/flash-nexus/flash-tunnel
features:
  - title: 识别你的项目
    details: 自动找到框架、包管理器、启动脚本和端口，支持 Vite、Next、Nuxt、Astro、静态站点等常见项目。
    link: /zh/guide/local-servers
    linkText: 了解本地服务
  - title: 分享恰好的地址
    details: 使用 flash-tunnel 获取临时地址，也可以让 flash-nexus 协调 DNS 记录和 Tunnel，生成稳定的命名预览。
    link: /zh/guide/ecosystem
    linkText: 了解整体架构
  - title: 分享之后，轻松离开
    details: 退出即回收 DNS 记录、隧道和子进程。后台守护与启动扫描，协助恢复被意外中断的会话。
    link: /zh/guide/cleanup
    linkText: 了解清理机制
---
