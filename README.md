# wefewe/qoder2api

`qoder2api` 的自建构建镜像仓库（用于本集群内部部署，非上游官方仓库）。

## 这是什么

- **上游**：`github.com/shuishuipingan/qoder2api-hub`（MIT，Python 3.11-alpine，零外部 pip 依赖；将阿里 Qoder 国际版与国内版原生服务封装为标准 OpenAI 兼容接口，支持多账号轮询、稳定物理设备指纹隔离、OAuth 设备授权一键登录、每日打卡领积分与 Pro 福利自动领取）。
- 仓库结构：
  - `upstream/` —— 上游源码快照（自动同步自 `shuishuipingan/qoder2api-hub` 最新 Release tag）；
  - `.github/workflows/build.yml` —— 多架构（amd64/arm64）构建并推送 `ghcr.io/wefewe/qoder2api`（含预发布本地冒烟测试门禁）；
  - `.github/workflows/sync-upstream.yml` —— 每 2 小时自动同步上游源码（有变化即 dispatch 重新构建）；
  - `.github/workflows/keepalive.yml` —— 月度保活提交（防 60 天定时停用）。

## 许可

上游为 MIT 许可，版权归原作者所有，详见 `upstream/LICENSE`。

## 部署（本集群）

- 镜像：`ghcr.io/wefewe/qoder2api:latest`
- 内部端口：`:8790`
- 数据持久化：`/app/accounts`（账户 Token 与配置）、`/app/usage`（调用流水与日志）
- 编排见 `/opt/swarm/stacks/us/qoder2api.yml`
