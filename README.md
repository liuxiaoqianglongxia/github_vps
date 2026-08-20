# github_vps

在 GitHub Codespaces / Docker 环境中启动临时 Ubuntu Desktop 或 Windows 11 实验环境。

> 仅用于你有权使用的环境。不要把带有弱口令的 SSH、RDP、Web Terminal 或管理端口公开到互联网。

## 安全变更

从 2026-08-20 起，`start.sh` **不再提供固定默认密码**：

- Windows 必须显式提供 `WINDOWS_PASSWORD`。
- Ubuntu 必须显式提供 `ROOT_PASSWORD`。
- 密码至少 12 个字符。
- 脚本启动完成后不再回显密码。

不要把密码、Token、Cookie、私钥或 `.env` 文件提交到 Git。

## 启动 Ubuntu

```bash
chmod +x start.sh
ROOT_PASSWORD='请替换为高强度唯一密码' bash start.sh ubuntu
```

默认端口：

- Web Terminal: `4200`
- SSH: `8022`
- RDP: `3389`

连接示例：

```bash
ssh root@localhost -p 8022
```

如果你把端口映射到外部网络，应额外使用防火墙、访问控制或可信隧道限制来源，不要直接裸露给公网。

## 启动 Windows 11

```bash
WINDOWS_PASSWORD='请替换为高强度唯一密码' bash start.sh win11
```

可选自定义用户名：

```bash
WINDOWS_USERNAME='MASTER' \
WINDOWS_PASSWORD='请替换为高强度唯一密码' \
bash start.sh win11
```

资源参数：

```bash
WINDOWS_USERNAME='MASTER' \
WINDOWS_PASSWORD='请替换为高强度唯一密码' \
WINDOWS_RAM_SIZE='4G' \
WINDOWS_CPU_CORES='4' \
WINDOWS_DISK_SIZE='64G' \
bash start.sh win11
```

默认端口：

- 管理界面: `8006`
- RDP: `3389`

## 停止

停止 Ubuntu：

```bash
bash start.sh stop ubuntu
```

停止 Windows：

```bash
bash start.sh stop win11
```

停止全部：

```bash
bash start.sh stop
```

## 端口说明

| 系统 | 服务 | 端口 |
|---|---|---:|
| Ubuntu | Web Terminal | 4200 |
| Ubuntu | SSH | 8022 |
| Ubuntu | RDP | 3389 |
| Windows | Web 管理界面 | 8006 |
| Windows | RDP | 3389 |

Ubuntu 和 Windows 都使用 `3389`，不要同时启动并争用同一宿主机端口。

## 环境变量

### Windows

- `WINDOWS_USERNAME`：默认 `MASTER`
- `WINDOWS_PASSWORD`：**必填，至少 12 字符**
- `WINDOWS_VERSION`：默认 `11`
- `WINDOWS_RAM_SIZE`：默认 `4G`
- `WINDOWS_CPU_CORES`：默认 `4`
- `WINDOWS_DISK_SIZE`：默认 `64G`
- `WINDOWS_DISK2_SIZE`：默认 `10G`

### Ubuntu

- `ROOT_PASSWORD`：**必填，至少 12 字符**

## 暴露到外网前检查

1. 已设置高强度、唯一密码。
2. 只开放确实需要的端口。
3. 优先使用私有网络、访问控制或可信隧道，不直接把管理端口暴露给整个互联网。
4. 确认环境中没有长期凭据、私钥或敏感文件。
5. 临时环境使用完毕后停止服务并清理不再需要的数据。

## 说明

该仓库属于实验/临时环境工具，不应作为长期生产服务器基线。
