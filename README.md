# Flash Nexus documentation

The English and Simplified Chinese VitePress site for [flash-nexus](https://github.com/flash-nexus/flash-nexus), with companion documentation for [flash-tunnel](https://github.com/flash-nexus/flash-tunnel) and [flash-dns](https://github.com/flash-nexus/flash-dns).

- Website: <https://flash-tunnel.imba97.dev>
- Source: [flash-tunnel/docs](https://github.com/flash-tunnel/docs)
- English: `docs/`
- 简体中文：`docs/zh/`

## Local development

Use Node.js 22.14 or newer and the pnpm version declared in `package.json`.

~~~sh
pnpm install
pnpm dev
~~~

Keep the English and Chinese routes in sync. Write diagrams in fenced `mermaid` blocks. Those blocks render in the browser on the pages that use them.

~~~sh
pnpm typecheck
pnpm check
pnpm build
pnpm check:build
pnpm preview
~~~

The checks verify locale parity, local links, assets, anchors and canonical URLs in the build.

## Deployment

GitHub Actions validates pull requests and publishes builds from `main` to GitHub Pages. The site uses `flash-tunnel.imba97.dev` as its custom domain.

1. In repository **Settings → Pages**, set the publishing source to **GitHub Actions**.
2. Set the custom domain to `flash-tunnel.imba97.dev`.
3. Point the domain's `flash-tunnel` CNAME record at `flash-tunnel.github.io`. Wait for GitHub's verification and enable HTTPS.
4. Run the deployment workflow and check the result in Actions.

`docs/public/CNAME` records the custom domain in the build artifact. The Pages setting is also required. Routes keep `.html` extensions for direct visits on GitHub Pages.

The shared theme derives from Imba97's MIT-licensed pmpx and Synra documentation. See [LICENSE](LICENSE).
