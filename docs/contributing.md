---
description: Contribute to the Flash Nexus tools or to these bilingual docs.
---

# Contributing

## Work on a package

Report behavior in the repository that owns it:

| Repository | Scope |
| --- | --- |
| [flash-nexus/flash-nexus](https://github.com/flash-nexus/flash-nexus) | CLI, SDK, configuration, and preview orchestration |
| [flash-nexus/flash-tunnel](https://github.com/flash-nexus/flash-tunnel) | Tunnel adapters and local process lifecycle |
| [flash-nexus/flash-dns](https://github.com/flash-nexus/flash-dns) | DNS providers, naming, planning, and ownership |
| [flash-tunnel/docs](https://github.com/flash-tunnel/docs) | This documentation site |

To work on Nexus, use Node.js 22.14 or newer and the pnpm version declared by the repository.

~~~sh
git clone https://github.com/flash-nexus/flash-nexus.git
cd flash-nexus
pnpm install
pnpm lint
pnpm typecheck
pnpm test
pnpm build
~~~

## Work on these docs

~~~sh
git clone https://github.com/flash-tunnel/docs.git flash-tunnel-docs
cd flash-tunnel-docs
pnpm install
pnpm dev
~~~

English pages live in `docs/`. Mirror the routes and examples in `docs/zh/`. Keep Mermaid diagrams in Markdown fences so they can be edited next to the prose. Check examples against the corresponding CLI or SDK before documenting them.

~~~sh
pnpm typecheck
pnpm check
pnpm build
pnpm check:build
~~~

Pull requests run validation. Pushes to `main` deploy the docs to GitHub Pages. See the [site README](https://github.com/flash-tunnel/docs#deployment) for deployment settings.
