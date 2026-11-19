---
description: 排查 flash-nexus 启动、Cloudflare 凭据、DNS 冲突与本地服务问题。
---

# 故障排查

先检查工具版本和当前目录加载到的配置：

~~~sh
npx flash-nexus --version
cloudflared --version
npx flash-nexus --dry-run
~~~

`--dry-run` 不会调用 Cloudflare。详见[输出说明](../reference/cli)。

## 找不到 cloudflared

安装 [cloudflared](https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/downloads/) 并加入 PATH，或将 `FLASH_NEXUS_CLOUDFLARED_PATH` 设为可执行文件路径。安装 npm 包时不会自动安装这个程序。

## 命名预览启动失败

| 现象 | 检查项 |
| --- | --- |
| `zone is required` | 通过 `--zone`、`FLASH_NEXUS_ZONE` 或 `dns.zone` 提供 `example.com` 这样的域名。只有 Zone ID 不够。 |
| Cloudflare 返回 401 或 403 | 检查 API Token、账号与 zone 的授权范围及[所需权限](./custom-domains)。 |
| 找不到账号或 zone | 显式设置 `CLOUDFLARE_ACCOUNT_ID`，确认 zone 已启用且属于该账号。 |
| 主机名不属于 zone | 使用 Nexus 所设置的 zone 下的主机名。需要生成子域名时，检查 `.env` 是否还设置了完整主机名。 |
| DNS 记录冲突 | 换一个主机名，或先检查现有记录的归属。 |

如果只需要临时地址，请显式传入 `--mode quick`。Nexus 默认使用 `named`。

## 本地服务或公网预览没就绪

- 在同一台机器上先打开本地地址，确认能访问后，再用 `--url` 连接这个服务。
- monorepo 中识别到错误项目时，用 `--cwd apps/web` 指定目录，或配置 [`server.command` 和 `server.port`](./local-servers)。
- 如果应用拒绝公网主机名，在应用里添加允许的主机名，见[本地服务指南](./local-servers)。
- 如果公网就绪检查超时，查看 `cloudflared` 输出，并确认本地服务仍在运行。

退出中断后发现残留资源，可参考[会话清理](./cleanup)。报告问题时请提供 Nexus 版本、系统、命令及错误信息，并在贴出内容前移除凭据。[提交 Issue](https://github.com/flash-nexus/flash-nexus/issues)。
