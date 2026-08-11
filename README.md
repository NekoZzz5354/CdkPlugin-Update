<h1 align="center">🎫 CDK Plugin</h1>

<p align="center">
  <b>轻量 · 安全 · 易用的 Minecraft 兑换码插件</b><br>
  为 Spigot / Paper / Purpur 及分支服务端提供完整的 CDK 礼包兑换系统
</p>

<p align="center">
  <img src="https://img.shields.io/github/v/release/NekoZzz5354/CdkPlugin-Update?style=flat-square&label=Version" alt="Version">
  <img src="https://img.shields.io/badge/minecraft-1.21+-green?style=flat-square" alt="Minecraft">
  <img src="https://img.shields.io/badge/java-21+-orange?style=flat-square" alt="Java">
  <img src="https://img.shields.io/github/license/NekoZzz5354/CdkPlugin-Update?style=flat-square&label=License" alt="License">
</p>

---

> 🤖 本项目由AI辅助生成，以MIT协议开源。

---

## ✨ 功能特性

### 🎫 核心功能
- **多模式 CDK 生成** — 支持随机字符、自定义前缀、批量生成
- **灵活奖励配置** — 物品、指令、金币、经验，自由组合
- **多条指令支持** — 单条 CDK 可绑定多条指令，用 `|` 分隔，依次执行
- **过期时间控制** — 每个 CDK 可设独立有效期
- **一次性 / 多次使用** — 按需求配置兑换次数限制
- **使用记录追踪** — 谁在什么时候兑换了什么，一目了然

### 🛡️ 安全与稳定
- **本地数据持久化** — 所有数据存储在 `plugins/CdkPlugin/` 目录，不依赖外部数据库
- **智能配置迁移** — 版本更新时自动合并配置，用户设置零丢失，自动备份旧文件
- **版本号自维护** — `config.yml` 中的 `version` 字段由插件自动更新
- **权限分级** — 管理员 / OP 权限隔离，防止误操作

---

## 🔔 更新

- **三源切换** — `jsDelivr`（国内加速）/ `GITHUB`（raw 原链）/ `GITHUB_RELEASE`（API + Token）
- **缓存绕过** — 自动附加时间戳参数，彻底解决 CDN 缓存导致的延迟问题
- **Token 可选** — GITHUB_RELEASE 源支持 GitHub Token 认证，留空则匿名降级
- **多渠道通知** — 控制台日志 + OP 聊天栏双重提醒
- **兼容服务端** — Spigot / Paper / Purpur 及分支服务端

---

## 📦 安装

### 环境要求

| 项目 | 最低版本 |
|------|----------|
| Minecraft | 1.21 |
| Java | 21 |
| 服务端 | Spigot / Paper / Purpur 及分支服务端 |

### 安装步骤

```bash
# 1. 下载 CdkPlugin-x.x.x.jar
# 2. 放入服务器 plugins/ 目录
# 3. 重启服务器
# 4. 编辑 plugins/CdkPlugin/config.yml 配置奖励内容
# 5. 输入 /cdk reload 重载配置
```

---

## 🚀 快速上手

### 生成兑换码（基础）

```bash
/cdk create                    # 生成1个随机CDK（默认长度16）
/cdk create 10 VIP             # 批量生成10个，前缀VIP
/cdk create 5 EVENT 2026-12-31 # 生成5个，2026年底过期
```

### 生成兑换码（绑定指令奖励）

```bash
# 单条指令
/cdk create single vip1 1 "eco give {player} 1000" 7d -s style-2

# 多条指令（用 | 分隔，必须写在引号内）
/cdk create single vip2 1 "eco give {player} 5000 | give {player} diamond 10 | effect give {player} speed 60 1" 7d -s style-2
```

> 兑换时插件会**按 `|` 顺序逐条执行**，每条指令独立解析 `{player}` 占位符。

### 占位符

| 占位符 | 替换为 |
|--------|--------|
| `{player}` | 兑换玩家的用户名 |
| `{uuid}` | 兑换玩家的 UUID |

### 兑换奖励

```bash
/cdk use ABCD-EFGH-IJKL    # 玩家输入兑换码
```

### 管理指令

| 指令 | 说明 |
|------|------|
| `/cdk create single <ID> <数量> "<命令>" [过期时间] [-s 样式]` | 生成单次CDK |
| `/cdk create multiple <名称> <ID> <次数> "<命令>" [过期时间] [-s 样式]` | 生成多次CDK |
| `/cdk list` | 查看所有未使用的 CDK |
| `/cdk delete id <ID>` | 删除指定ID下所有CDK |
| `/cdk delete code <代码>` | 删除单个CDK |
| `/cdk info <代码>` | 查看CDK详情 |
| `/cdk add <ID> <数量>` | 为ID增加使用次数 |
| `/cdk export` | 导出CDK到文件 |
| `/cdk reload` | 重载配置文件 |
| `/cdk version` | 查看插件版本与更新源状态 |
| `/cdk update` | 手动检查更新 |

---

## ⚙️ 配置说明

`config.yml` 全字段带中文注释，开箱即用：

```yaml
# 由插件自动维护，请勿手动修改
version: "x.x.x"

# ===== 基础设置 =====
prefix: "§6[CDK] §r"
default-length: 16
default-style: "style-2"

# ===== 字符集 =====
charset:
  digits: true
  uppercase: true
  lowercase: true

# ===== 更新检查 =====
update:
  enabled: true
  # 三选一：
  # jsDelivr       → 国内CDN加速（默认）
  # GITHUB         → GitHub raw原链
  # GITHUB_RELEASE → GitHub API（需填token）
  source: "jsDelivr"
  token: ""
  check-interval-hours: 6

# ===== 配置迁移 =====
migration:
  enabled: true
  auto-backup: true
  backup-keep-count: 5
```

### 切换更新源

| `source` 值 | 说明 | 是否需要 Token |
|--------------|------|:---:|
| `jsDelivr` | 国内CDN加速，自动绕过缓存 | ❌ |
| `GITHUB` | GitHub raw 原链 | ❌ |
| `GITHUB_RELEASE` | GitHub Releases API，最权威 | ✅（留空则匿名） |

> 只改 `source` 一个词，重启或 `/cdk reload` 即刻生效。

---

## 🔄 更新机制

```
插件启动
  ↓
按 source 选择更新源（jsDelivr / GITHUB / GITHUB_RELEASE）
  ↓
请求时自动附加时间戳参数（绕过CDN缓存）
  ↓
对比本地版本号（config.yml 中的 version 字段）
  ↓
发现新版本 → 控制台 + OP 聊天栏通知
  ↓
OP 点击链接 → GitHub Releases → 下载 jar → 替换重启
  ↓
插件自动迁移 config.yml（新字段补齐，旧配置保留）
```

---

## 📁 文件结构

```
plugins/CdkPlugin/
├── config.yml              ← 主配置文件（version字段由插件自动维护）
├── cdk-data.yml            ← CDK 数据持久化存储
├── config.backup.*.yml     ← 自动备份文件
└── logs/
    └── cdk.log             ← 操作日志
```

---

## 🔐 权限节点

| 权限 | 说明 | 默认 |
|------|------|------|
| `cdk.admin` | 管理员权限（生成/删除/重载） | OP |
| `cdk.use` | 使用 `/cdk use` 兑换 | 所有玩家 |
| `cdk.update.notify` | 接收更新通知 | OP |

---

## 🐛 已知问题 & 解决方案

| 问题 | 解决 |
|------|------|
| 更新检查延迟 | 切换 `source` 为 `GITHUB_RELEASE` 或 `jsDelivr`（已内置缓存绕过） |
| 颜色代码显示原始文本 | 确保服务端支持 `§` 颜色代码（Paper 默认支持） |
| GITHUB_RELEASE 返回 403 | Token 无效或权限不足，检查 `update.token` 配置 |
| 多条指令不生效 | 确保用 `|` 分隔且**整个命令字符串用引号包裹** |

---

## 📄 License

MIT License — 自由使用、修改、分发。详见 [LICENSE](LICENSE)。

---

<p align="center">
  Made with ❤️ for the Minecraft community<br>
  <a href="https://github.com/NekoZzz5354/CdkPlugin-Update/releases">📦 Releases</a> ·
  <a href="https://github.com/NekoZzz5354/CdkPlugin-Update/issues">🐛 Issues</a>
