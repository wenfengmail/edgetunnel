# 部署说明（Cloudflare Pages + GitHub 自动部署）

本项目 fork 自 `cmliu/edgetunnel`，部署在 Cloudflare Pages，并已和本仓库的 `main` 分支打通。

## Cloudflare 侧

| 项目 | 值 |
| :--- | :--- |
| Pages 项目名 | `silent-meadow-7c3` |
| 访问地址 | https://silent-meadow-7c3.pages.dev |
| KV 绑定 | 变量名 `KV` |
| 环境变量 | `ADMIN`（后台管理密码），已配置 |

> 后台入口：`https://silent-meadow-7c3.pages.dev/admin`，输入 `ADMIN` 的值登录。

## 自动部署链路

1. 本仓库自带 `.github/workflows/sync.yml`：每天 00:00(UTC) 以及手动触发时，把上游 `cmliu/edgetunnel` 的更新合并进本仓库 `main` 分支并 push。
2. 该 push 会触发 Cloudflare Pages 的 Git 构建，自动重新部署，无需在 Cloudflare 侧做任何手动操作。

> 首次使用时，需要在 GitHub 仓库的 Actions 页面点一次「I understand my workflows, go ahead and enable them」，
> 否则 fork 仓库的定时任务不会自动运行。

## 手动重新部署

- GitHub：Actions → `Upstream Sync` → Run workflow（同步上游并触发部署）
- 本地：`wrangler pages deploy . --project-name silent-meadow-7c3 --branch main`

## 其它说明

- 请勿把管理员密码等敏感内容写进 `wrangler.toml` 或代码里，统一在 Pages 项目的变量/绑定中配置。
- 本项目在 Cloudflare **Pages** 上运行正常；直接以 Workers 方式部署（`wrangler deploy`）会因脚本体积/启动开销报 `Error 1101`，故采用 Pages 方式。
