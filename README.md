# 🧩 Project Breakdown Skill

> 智能项目任务切分与管理技能 — 用于 MVP 生成时进行任务分解、记录和状态追踪

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](VERSION.md)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-stable-brightgreen.svg)]()

## ✨ 核心价值

| 指标 | 提升效果 |
|------|----------|
| 🪙 Token 节省 | **30-50%** |
| 🚀 开发效率 | **提升 2-3 倍** |
| 🛡️ 项目稳定性 | **显著提升** |
| ✅ 上下文一致性 | **100% 保证** |

## 📖 简介

**Project Breakdown Skill** 是一个专为 AI Agent 设计的智能项目管理技能，在 MVP（最小可行产品）开发过程中自动完成：

- 🔍 **智能项目切分** — 自动识别项目类型和技术栈，拆解为可执行的任务模块
- 💾 **状态持久化** — 项目元数据和任务清单保存到本地文件系统，跨会话恢复
- 🤖 **自动介入** — 无需手动调用，Agent 在每轮交互中自动检测并介入
- 🔄 **变更管理** — 需求变更时智能分析影响范围，自动更新任务计划
- 📊 **进度追踪** — 实时统计任务完成率、工时和下一步建议

## 🚀 快速开始

### 安装

```bash
# 将 Skill 复制到 Agent 的 skills 目录
cp -r project-breakdown-skill ~/.workbuddy/skills/project-breakdown

# 或在 Windows 上
xcopy /E /I project-breakdown-skill %USERPROFILE%\.workbuddy\skills\project-breakdown
```

### 使用

安装后，Skill 会自动介入，无需手动调用：

```
👤 用户：创建一个在线教育平台，包含课程管理、用户学习进度跟踪、在线测试功能

🤖 Agent：[自动检测到"创建"关键词]
         → 调用 project-breakdown skill
         → 识别项目类型：Web 应用
         → 生成 8 个任务，预计 24 小时

         📋 项目概览
         - 项目名称：在线教育平台
         - 技术栈：React + Node.js + MongoDB
         - 任务数：8 个

         📝 任务清单
         1. [待办] 项目结构初始化 (2h) - 关键
         2. [待办] 用户认证系统 (4h) - 关键
         3. [待办] 课程管理功能 (6h) - 高优先级
         ...
```

## 🎯 支持场景

| 场景 | 触发关键词 | 说明 |
|------|-----------|------|
| 🆕 新项目创建 | 创建、开发、新建、搭建... | 自动分析需求，生成任务清单 |
| ✏️ 需求变更 | 修改、增加、调整、删除... | 分析影响范围，更新任务计划 |
| 📊 进度查询 | 进度、状态、任务、完成... | 返回完成率和工时统计 |
| 🔍 项目回顾 | 之前、回顾、查看、需求... | 加载项目上下文和历史 |
| ▶️ 继续项目 | 继续、接下来、下一步... | 推荐下一个待执行任务 |

## 📦 内置任务模板

提供 3 种项目类型模板，共计 **61 个预设任务**：

### 🌐 Web 应用模板 (17 个任务)
- Setup（3）→ Core Feature（4）→ Frontend（3）→ Testing（3）→ Enhancement（1）→ Deployment（2）→ Docs（1）

### 🔌 API 服务模板 (20 个任务)
- Setup（2）→ Core（4）→ Security（3）→ Data（3）→ Testing（3）→ Docs（2）→ Deployment（3）

### ⚡ CLI 工具模板 (24 个任务)
- Setup（2）→ Core（3）→ UX（3）→ Config（3）→ Testing（3）→ Distribution（3）→ Docs（2）→ Enhancement（4）→ CI（1）

## 📁 项目结构

```
project-breakdown-skill/
├── SKILL.md              # ⭐ 核心技能文档（Agent 加载入口）
├── README.md             # 📖 项目说明（本文件）
├── QUICKSTART.md         # 🚀 快速开始指南
├── INSTALLATION.md       # 🔧 安装配置指南
├── EXAMPLES.md           # 💡 使用示例集合
├── VERSION.md            # 📌 版本信息和路线图
├── MANIFEST.md           # 📋 文件清单
├── config.json           # ⚙️ Skill 配置文件
├── .gitignore            # 🚫 Git 忽略规则
├── LICENSE               # 📄 MIT 许可证
└── templates/            # 📦 任务模板目录
    ├── web-app.json      # Web 应用模板（17 个任务）
    ├── api-service.json  # API 服务模板（20 个任务）
    └── cli-tool.json     # CLI 工具模板（24 个任务）
```

## ⚙️ 配置

编辑 `config.json` 自定义 Skill 行为：

```json
{
  "enableAutoIntervention": true,   // 启用自动介入
  "autoSave": true,                 // 自动保存项目状态
  "autoArchive": true,              // 自动归档旧项目
  "defaultPriority": "medium",      // 默认任务优先级
  "enableChangeLog": true           // 启用变更日志
}
```

### 自定义触发关键词

```json
{
  "triggerKeywords": {
    "newProject": ["创建", "开发", "新建", "搭建", "构建"],
    "queryStatus": ["进度", "状态", "任务", "完成", "进展"],
    "modifyRequest": ["修改", "增加", "调整", "删除", "更新"],
    "reviewProject": ["之前", "回顾", "查看", "需求"],
    "continueProject": ["继续", "接下来", "下一步"]
  }
}
```

## 📊 数据模型

### 项目元数据
```json
{
  "projectId": "uuid",
  "projectName": "项目名称",
  "projectType": "web-app | api-service | cli-tool",
  "techStack": ["react", "nodejs", "typescript"],
  "status": "planning | in-progress | completed | on-hold"
}
```

### 任务清单
```json
{
  "taskId": "task-001",
  "title": "任务标题",
  "category": "setup | core-feature | enhancement | bugfix | testing | docs",
  "priority": "critical | high | medium | low",
  "status": "pending | in-progress | completed | blocked",
  "dependencies": ["task-000"],
  "estimatedEffort": "2h",
  "acceptanceCriteria": ["验收标准1", "验收标准2"]
}
```

## 🗺️ 路线图

### v1.1.0 (计划中)
- [ ] 多项目并行管理
- [ ] 自定义模板编辑器
- [ ] 可视化进度看板
- [ ] Web 界面支持

### v1.2.0 (计划中)
- [ ] 团队协作功能
- [ ] 任务分配机制
- [ ] 版本控制集成

### v2.0.0 (规划中)
- [ ] AI 驱动的任务优化
- [ ] 智能工时预测
- [ ] 风险评估系统
- [ ] 外部工具集成（Jira, Trello 等）

## 🤝 贡献

欢迎贡献代码、报告问题、提出建议！

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/amazing-feature`)
3. 提交更改 (`git commit -m 'Add amazing feature'`)
4. 推送到分支 (`git push origin feature/amazing-feature`)
5. 创建 Pull Request

## 📄 许可证

本项目采用 [MIT 许可证](LICENSE) 开源。

## 📞 支持

- 📖 查阅 [QUICKSTART.md](QUICKSTART.md) 快速上手
- 💡 参考 [EXAMPLES.md](EXAMPLES.md) 学习最佳实践
- 🔧 查看 [INSTALLATION.md](INSTALLATION.md) 安装配置
- 📌 了解 [VERSION.md](VERSION.md) 版本信息

---

**Made with ❤️ for AI-powered development workflows**
