---
description: 为 Flash Nexus 工具或这套双语文档贡献内容。
---

# 参与贡献

## 修改工具

按问题所属的职责选择仓库：

| 仓库 | 负责范围 |
| --- | --- |
| [flash-nexus/flash-nexus](https://github.com/flash-nexus/flash-nexus) | CLI、SDK、配置与预览会话编排 |
| [flash-nexus/flash-tunnel](https://github.com/flash-nexus/flash-tunnel) | 隧道适配器与本地进程生命周期 |
| [flash-nexus/flash-dns](https://github.com/flash-nexus/flash-dns) | DNS 服务商适配、命名、变更预览及归属 |
| [flash-tunnel/docs](https://github.com/flash-tunnel/docs) | 本文档站 |

开发 Nexus 使用 Node.js 22.14 或以上版本，以及仓库声明的 pnpm 版本。

~~~sh
git clone https://github.com/flash-nexus/flash-nexus.git
cd flash-nexus
pnpm install
pnpm lint
pnpm typecheck
pnpm test
pnpm build
~~~

## 修改文档

~~~sh
git clone https://github.com/flash-tunnel/docs.git flash-tunnel-docs
cd flash-tunnel-docs
pnpm install
pnpm dev
~~~

英文在 `docs/`，中文在 `docs/zh/`。两边使用同样的路由结构，示例也应保持对应。Mermaid 图直接写在 Markdown 代码块中，便于与正文一起编辑。新增示例前，核对对应 CLI 或 SDK 的实际行为。

~~~sh
pnpm typecheck
pnpm check
pnpm build
pnpm check:build
~~~

Pull request 会运行检查。推送到 `main` 后，站点通过 GitHub Pages 部署。部署配置见[站点 README](https://github.com/flash-tunnel/docs#deployment)。
