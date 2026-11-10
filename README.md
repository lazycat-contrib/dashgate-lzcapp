# dashgate-lzcapp

[Kha-kis/dashgate](https://github.com/Kha-kis/dashgate) 的懒猫微服打包，
仅发布到**喵喵（私有）商店**，镜像模式（不复制镜像、只改运行时地址）。

**DashGate** 是一个自托管应用网关：把散落在各处的 Web 服务收进一个面板，
支持分组访问控制（谁能看到哪些应用）以及从 Docker / Traefik / NPM /
Nginx / Caddy 自动发现应用。

## 打包内容

| 项目 | 值 |
|------|----|
| 包名 | `community.lazycat.app.dashgate` |
| 上游镜像 | `khak1s/dashgate`（镜像模式） |
| 运行时加速 | `docker.1ms.run`（Action 默认 Docker Hub 加速地址） |
| 版本来源 | 上游三段 SemVer 标签（`1.5.11`，无 `v` 前缀） |
| 容器端口 | 1738 |
| 持久化 | `/lzcapp/var/config`（SQLite 库、config.yaml、图标） |
| 部署参数 | 无 |

## 首次使用

1. 安装后打开应用入口，进入初始化向导，创建第一个管理员账号。
2. 用管理员登录，在 **Settings → Admin → Content** 手动添加应用，
   或在 **Discovery** 中按需启用自动发现。

## 关于 Docker 自动发现

上游提供 `DOCKER_DISCOVERY=true` + 挂载 `/var/run/docker.sock` 的用法。
**本包默认不挂载 docker.sock**：把 Docker socket 交给一个带 Web 面板的
容器，等同于把整机容器的控制权交出去，风险远大于收益；上游文档自己也
建议改用受限的 socket 代理。需要自动发现时，请在应用内用手动录入，
或自行按上游建议接入 socket 代理后重新打包。

Traefik / NPM / Nginx / Caddy 的自动发现走 HTTP API，不涉及特权，
可以在应用设置里自行开启（本包未预置相关环境变量）。

## 访问方式说明

DashGate 自带账号体系，但 `lzc-manifest.yml` 把 `/` 放进了 `public_path`：
**整个站点公开访问**，由应用自己的登录（会话 / LDAP / OIDC）负责鉴权。

- 懒猫平台的入口登录无法作为应用内登录的前置环节，因此这里交给应用自己。
- 请第一时间把初始化向导里创建的账号换成强密码。

## 版本与发布

- `version_source` 指向 `dashgate` 镜像，每天定时检查上游新标签。
- 新版本会更新 `package.yml` 的 `version` 与 `lzc-manifest.yml` 的镜像地址，
  构建 LPK、创建带版本号的 Release 资产，再提交到喵喵商店。
- 自动更新只取 `^v?[0-9]+\.[0-9]+\.[0-9]+$`。
- 官方商店关闭（`stores.official.enabled: false`），不会触发官方审核。

## 版权

上游 [Kha-kis/dashgate](https://github.com/Kha-kis/dashgate)，MIT License
（见 `content/licenses/LICENSE`）。本仓库只做懒猫打包，不修改上游代码。

本包尚未在懒猫实机安装验证。
