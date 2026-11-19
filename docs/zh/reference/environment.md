---
description: Nexus 的共享环境变量、Cloudflare 凭据和底层回退顺序。
---

# 环境变量

Nexus 默认读取项目目录中的 `.env`。可以通过 `--env-file <path>` 指定其他文件，也可以直接从进程环境传入变量。

## Nexus 设置

优先取 Nexus 变量，再取表中对应的底层回退，两者都没有时使用项目配置或默认值。CLI 参数和显式 SDK 选项可以覆盖对应的环境设置。

| 用途 | Nexus 变量 | 底层回退 |
| --- | --- | --- |
| 预览模式 | `FLASH_NEXUS_MODE` | `FLASH_TUNNEL_MODE` |
| DNS zone 名称 | `FLASH_NEXUS_ZONE` | `FLASH_DNS_ZONE` |
| 完整主机名 | `FLASH_NEXUS_HOSTNAME` | 先 `FLASH_TUNNEL_HOSTNAME`，再 `FLASH_DNS_HOSTNAME` |
| 子域名模板 | `FLASH_NEXUS_SUBDOMAIN` | `FLASH_TUNNEL_SUBDOMAIN` |
| `cloudflared` 路径 | `FLASH_NEXUS_CLOUDFLARED_PATH` | `FLASH_TUNNEL_CLOUDFLARED_PATH` |

如果调用方未指定可执行文件路径，Cloudflare Tunnel 适配器还会读取 `FLASH_TUNNEL_CLOUDFLARED`。在 Nexus 中建议使用 `FLASH_NEXUS_CLOUDFLARED_PATH`。

## Cloudflare 凭据

| 变量 | 用途 |
| --- | --- |
| `CLOUDFLARE_API_TOKEN` | 命名模式下的隧道和 DNS API 认证。 |
| `CLOUDFLARE_ACCOUNT_ID` | 指定 Cloudflare 账号。使用多个账号时建议显式填写。 |
| `CLOUDFLARE_ZONE_ID` | DNS 适配器使用的可选 zone 标识，Nexus 仍需要 zone 名称。 |

配置方法和所需权限见[使用自己的域名](../guide/custom-domains)。临时模式不需要这些凭据。
