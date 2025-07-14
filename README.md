# GitNote Git 服务端

这是 GitNote 项目的 Git 仓库服务端部分，提供通过 SSH 协议访问的 Git 裸仓库，支持推送时触发自定义 Hook 与 API 通信。

## 🧩 主要功能

- 基于 Alpine 的轻量 SSH 服务
- 使用 Git 裸仓库作为远程存储
- 推送 `main` 分支时，自动调用外部 API 进行校验或同步
- 支持基于公钥的 SSH 登录认证
- 可通过 Docker 单独部署或与 Rust API 一起使用

## 📂 项目结构

```text
.
├── Dockerfile             # SSH + Git 镜像构建文件
├── entrypoint.sh          # 容器启动脚本，自动创建用户和初始化仓库
├── update                 # Git update hook 脚本，调用 API
├── ssh_keys/
│   └── authorized_keys.example  # 公钥登录
```

## 🔐 安全说明

- 禁用 root 登录，仅允许使用 `git` 用户
- 仅支持公钥认证，密码登录可选启用
- `update` hook 限定只处理 `main` 分支推送
- 推送时将调用外部 API，返回失败将拒绝此次 push

## 🧪 开发建议

- 推荐使用 Docker Compose 与 API 服务联合部署
- Git 仓库存储挂载在 `/git-repo`，便于数据持久化与共享
