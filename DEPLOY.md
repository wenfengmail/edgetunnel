# 部署说明（Cloudflare Pages）

本项目 fork 自 `cmliu/edgetunnel`，当前状态如下。

## 已完成

| 项目 | 值 |
| :--- | :--- |
| Pages 项目 | `silent-meadow-7c3` |
| 默认地址 | https://silent-meadow-7c3.pages.dev |
| 自定义域名 | https://meadow.wanwan.ccwu.cc （`meadow` CNAME → `silent-meadow-7c3.pages.dev`，已生效） |
| KV 绑定 | 变量名 `KV` |
| 环境变量 | `ADMIN` |

## 当前问题

Cloudflare 已**封禁该 Pages 项目**（接口返回 `8000119 Your Pages project has been blocked`）：

- 已部署的旧版本仍可访问，但**任何新的构建/上传都会被拒绝**，包括 Git push 触发构建、Deploy Hook 和 `wrangler pages deploy`。
- 以 Workers 方式部署同一份 `_worker.js` 会在第一次请求就返回 `Error 1101`，属于同一类限制。

## 上游同步

`.github/workflows/sync.yml` 仍会每天把 `cmliu/edgetunnel` 的更新合并进本仓库 `main`，仓库代码会保持最新；
但部署需要在 Cloudflare 解除限制（联系 `abusereply@cloudflare.com`）或换用其它平台后才能重新生效。

## 手动重新部署（解封后）

```bash
mkdir -p dist && cp _worker.js dist/
wrangler pages deploy dist --project-name silent-meadow-7c3 --branch main
```
