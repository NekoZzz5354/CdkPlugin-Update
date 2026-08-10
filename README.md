<p align="center">
  <img src="https://img.shields.io/badge/version-2.0.4--R1-blue?style=flat-square" alt="Version">
  <img src="https://img.shields.io/badge/minecraft-1.21+-green?style=flat-square" alt="Minecraft">
  <img src="https://img.shields.io/badge/java-21+-orange?style=flat-square" alt="Java">
  <img src="https://img.shields.io/badge/license-MIT-lightgrey?style=flat-square" alt="License">
</p>

<h1 align="center">🎫 CDK Plugin</h1>

<p align="center">
  <b>轻量 · 安全 · 易用的 Minecraft 兑换码插件</b><br>
  为 Spigot / Paper / Purpur 服务端提供完整的 CDK 礼包兑换系统
</p>

---

> 🤖 **AI 使用声明**：本项目所有代码、文档及资源均由 AI 工具辅助生成，
> 并以 **MIT 协议开源**。无需额外授权，引用或再分发时请保留原始版权声明。

---

## ✨ 功能特性

### 🎫 核心功能
- **多模式 CDK 生成** — 支持随机字符、自定义前缀、批量生成
- **灵活奖励配置** — 物品、指令、金币、经验，自由组合
- **过期时间控制** — 每个 CDK 可设独立有效期
- **一次性 / 多次使用** — 按需求配置兑换次数限制
- **使用记录追踪** — 谁在什么时候兑换了什么，一目了然

### 🛡️ 安全与稳定
- **本地数据持久化** — 所有数据存储在 `plugins/CdkPlugin/` 目录，不依赖外部数据库
- **智能配置迁移** — 版本更新时自动合并配置，**用户设置零丢失**，自动备份旧文件
- **版本号自维护** — `config.yml` 中的 `version` 字段由插件自动更新，无需手动修改
- **权限分级** — 管理员 / OP 权限隔离，防止误操作

### 🔔 更新系统
- **自动更新检查** — 启动 + 每 6 小时定时检查
- **双源切换** — `jsDelivr`（国内加速）/ `GITHUB`（raw 原链）一键切换
- **多渠道通知** — 控制台日志 + OP 聊天栏双重提醒
- **一键跳转下载** — 通知消息附带 Releases 链接

---

## 📦 安装

### 环境要求

| 项目 | 最低版本 |
|------|----------|
| Minecraft | 1.21 |
| Java | 21 |
| 服务端 | Spigot / Paper / Purpur |

### 安装步骤

```bash
# 1. 下载 CdkPlugin-2.0.4-R1.jar
# 2. 放入服务器 plugins/ 目录
# 3. 重启服务器
# 4. 编辑 plugins/CdkPlugin/config.yml 配置奖励内容
# 5. 输入 /cdk reload 重载配置
```

---

## 🚀 快速上手

### 生成兑换码

```bash
# 生成一个随机 CDK（默认长度16，style-2）
/cdk create

# 批量生成 10 个，前缀 VIP
/cdk create 10 VIP

# 生成指定过期时间的 CDK
/cdk create 5 EVENT 2026-12-31
```

### 兑换奖励

```bash
# 玩家输入
/cdk use ABCD-EFGH-IJKL
```

### 管理指令

| 指令 | 说明 |
|------|------|
| `/cdk create [数量] [前缀] [过期时间]` | 生成 CDK |
| `/cdk list` | 查看所有未使用的 CDK |
| `/cdk delete <代码>` | 删除指定 CDK |
| `/cdk reload` | 重载配置文件 |
| `/cdk version` | 查看插件版本、更新源和状态 |
| `/cdk update` | 手动检查更新 |

---

## ⚙️ 配置说明

`config.yml` 全字段带中文注释，开箱即用：

```yaml
# 由插件自动维护，请勿手动修改
version: "2.0.4-R1"

# ===== 基础设置 =====
prefix: "§6[CDK] §r"          # 聊天栏前缀
default-length: 16              # CDK 默认长度
default-style: "style-2"       # 默认样式（style-1: 纯数字 / style-2: 字母数字混合）

# ===== 字符集 =====
charset:
  digits: true                  # 包含数字 0-9
  uppercase: true               # 包含大写字母 A-Z
  lowercase: true               # 包含小写字母 a-z

# ===== 更新检查 =====
update:
  enabled: true
  # 更新源（二选一，不填URL）
  # jsDelivr → 国内CDN加速（默认推荐）
  # GITHUB  → GitHub raw原链（国外服务器推荐）
  source: "jsDelivr"
  check-interval-hours: 6
  releases-url: "https://github.com/NekoZzz5354/CdkPlugin-Update/releases/latest"

# ===== 配置迁移 =====
migration:
  enabled: true                 # 更新时自动合并配置
  auto-backup: true             # 迁移前自动备份
  backup-keep-count: 5          # 保留最近5个备份
```

### 切换更新源

| `source` 值 | 实际请求地址 | 适用场景 |
|--------------|--------------|----------|
| `jsDelivr` | `cdn.jsdelivr.net/gh/.../version.json` | 国内服务器（默认） |
| `GITHUB` | `raw.githubusercontent.com/.../version.json` | 国外服务器 |

> 只改 `source` 一个词，重启或 `/cdk reload` 即刻生效。

---

## 🔄 更新机制

```
插件启动
  ↓
读取 version.json（按 source 选择 jsDelivr 或 GitHub raw）
  ↓
对比本地版本号（config.yml 中的 version 字段）
  ↓
发现新版本 → 控制台 + OP 聊天栏通知
  ↓
OP 点击链接 → GitHub Releases → 下载 jar → 替换重启
  ↓
插件自动迁移 config.yml（新字段补齐，旧配置保留）
  ↓
自动更新 config.yml 中的 version 字段
```

### version.json 结构

```json
{
  "version": "2.0.4-R1",
  "downloadUrl": "https://github.com/NekoZzz5354/CdkPlugin-Update/releases/download/v2.0.4-R1/CdkPlugin-2.0.4-R1.jar",
  "releasesUrl": "https://github.com/NekoZzz5354/CdkPlugin-Update/releases/latest",
  "updateNotes": "配置文件version字段改为插件自维护，更新源改为预选项切换",
  "releaseDate": "2026-08-10",
  "minServerVersion": "1.20",
  "requiredJava": "21"
}
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
| `cdk.use` | 使用 `/cdk redeem` 兑换 | 所有玩家 |
| `cdk.update.notify` | 接收更新通知 | OP |

---

## 🐛 已知问题 & 解决方案

| 问题 | 解决 |
|------|------|
| 更新检查失败 | 切换 `source` 为 `GITHUB` 或 `jsDelivr` 重试 |
| 颜色代码显示原始文本 | 确保服务端支持 `§` 颜色代码（Paper 默认支持） |
| 配置项丢失 | v2.0.4+ 已修复，旧版请手动备份后升级 |

---

## 📜 版本历史

| 版本 | 亮点 |
|------|------|
| **v2.0.4-R1** | 🔧 config.yml 的 version 改为插件自维护；更新源改为 jsDelivr/GITHUB 预选项切换 |
| **v2.0.4** | 🐛 修复配置覆盖 Bug，新增智能配置迁移器 |
| **v2.0.3** | 🐛 修复 `§` 颜色代码不显示问题 |
| **v2.0.2** | ✨ 新增自动更新检查 + `/cdk update` 指令 |
| **v2.0.1** | ✨ 重构数据存储，本地持久化 |
| **v2.0.0** | 🎉 V2 重构版本，全新架构 |

---

## 📄 License

MIT License — 自由使用、修改、分发。
详见 [LICENSE](LICENSE)。

---

<p align="center">
  Made with ❤️ for the Minecraft community<br>
  <a href="https://github.com/NekoZzz5354/CdkPlugin-Update/releases">📦 Releases</a> ·
  <a href="https://github.com/NekoZzz5354/CdkPlugin-Update/issues">🐛 Issues</a>
</p>
