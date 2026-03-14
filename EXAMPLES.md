# Project Breakdown Skill 使用示例

## 目录
1. [场景示例](#场景示例)
2. [完整对话流](#完整对话流)
3. [项目文件示例](#项目文件示例)
4. [故障排查示例](#故障排查示例)

---

## 场景示例

### 场景1：新项目创建 - Web应用

**用户输入：**
```
我想创建一个在线教育平台，包含课程管理、用户学习进度跟踪、在线测试功能
```

**主Agent处理流程：**
```
1. 检测触发关键词："创建"
2. 检测项目类型：在线教育平台 → Web应用
3. 加载web-app.json模板
4. 调用project-breakdown skill
5. 生成任务清单
```

**Skill返回结果：**
```json
{
  "projectId": "edu-platform-001",
  "projectName": "在线教育平台",
  "projectType": "web-app",
  "techStack": ["react", "nodejs", "mongodb", "typescript"],
  "summary": "包含课程管理、学习进度跟踪、在线测试功能的在线教育平台",
  "tasks": [
    {
      "taskId": "task-001",
      "title": "项目结构初始化",
      "category": "setup",
      "priority": "critical",
      "status": "pending",
      "estimatedEffort": "2h",
      "dependencies": []
    },
    {
      "taskId": "task-002",
      "title": "用户认证系统",
      "category": "core-feature",
      "priority": "critical",
      "status": "pending",
      "estimatedEffort": "4h",
      "dependencies": ["task-001"]
    },
    {
      "taskId": "task-003",
      "title": "课程管理功能",
      "category": "core-feature",
      "priority": "high",
      "status": "pending",
      "estimatedEffort": "6h",
      "dependencies": ["task-001", "task-002"]
    },
    {
      "taskId": "task-004",
      "title": "学习进度追踪",
      "category": "core-feature",
      "priority": "high",
      "status": "pending",
      "estimatedEffort": "4h",
      "dependencies": ["task-001", "task-002"]
    },
    {
      "taskId": "task-005",
      "title": "在线测试系统",
      "category": "core-feature",
      "priority": "high",
      "status": "pending",
      "estimatedEffort": "5h",
      "dependencies": ["task-001", "task-002"]
    }
  ],
  "recommendedNextTasks": ["task-001"],
  "actionTaken": "初始化项目"
}
```

**主Agent响应：**
```
好的，我已经为您的在线教育平台创建了项目规划！

📋 项目概览
- 项目名称：在线教育平台
- 项目类型：Web应用
- 技术栈：React + Node.js + MongoDB + TypeScript
- 总任务数：5个
- 预计总工时：21小时

🎯 核心功能模块
1. 课程管理
2. 学习进度跟踪
3. 在线测试系统

📝 任务清单（按优先级排序）
1. [待办] 项目结构初始化 (2h) - 关键任务
2. [待办] 用户认证系统 (4h) - 关键任务
3. [待办] 课程管理功能 (6h) - 高优先级
4. [待办] 学习进度追踪 (4h) - 高优先级
5. [待办] 在线测试系统 (5h) - 高优先级

💡 建议下一步
先完成"项目结构初始化"，这是所有后续任务的基础。

要开始执行第一个任务吗？
```

---

### 场景2：需求变更处理

**用户输入：**
```
我想给教育平台增加一个直播功能，让学生可以实时观看课程
```

**主Agent处理流程：**
```
1. 检测触发关键词："增加"
2. 加载当前项目状态
3. 分析变更需求：新增直播功能
4. 调用project-breakdown skill
5. 分析变更影响
```

**Skill返回结果：**
```json
{
  "projectId": "edu-platform-001",
  "changeType": "requirement-update",
  "changeSummary": "新增直播功能模块",
  "affectedTasks": [
    "task-003", // 课程管理功能
    "task-004"  // 学习进度追踪
  ],
  "newTasks": [
    {
      "taskId": "task-006",
      "title": "直播功能集成",
      "category": "core-feature",
      "priority": "high",
      "status": "pending",
      "estimatedEffort": "8h",
      "dependencies": ["task-001", "task-002"]
    }
  ],
  "updatedTasks": [
    {
      "taskId": "task-003",
      "title": "课程管理功能（支持直播）",
      "notes": "需要支持直播课程的创建和管理"
    },
    {
      "taskId": "task-004",
      "title": "学习进度追踪（含直播）",
      "notes": "需要追踪直播观看进度"
    }
  ],
  "changeLog": {
    "timestamp": "2026-03-15T10:30:00Z",
    "description": "新增直播功能需求",
    "details": "学生可以实时观看课程直播"
  }
}
```

**主Agent响应：**
```
收到！我已经分析了新增直播功能对项目的影响。

🔄 需求变更报告
- 变更类型：新增功能
- 变更描述：增加直播功能模块，支持学生实时观看课程

📊 影响分析
- 影响现有任务：2个
  - task-003: 课程管理功能（需要支持直播课程）
  - task-004: 学习进度追踪（需要追踪直播观看）

- 新增任务：1个
  - task-006: 直播功能集成 (8h)

💰 项目规模变化
- 原计划：5个任务，21小时
- 现在计划：6个任务，29小时
- 增加工作量：8小时

🎯 技术栈扩展
- 新增依赖：WebRTC、WebSocket、流媒体服务器

📝 更新后的任务列表
1. [待办] 项目结构初始化 (2h)
2. [待办] 用户认证系统 (4h)
3. [待办] 课程管理功能（支持直播）(6h)
4. [待办] 学习进度追踪（含直播）(4h)
5. [待办] 在线测试系统 (5h)
6. [待办] 直播功能集成 (8h) ⬅️ 新增

💡 建议
直播功能需要额外的技术选型（如WebRTC），建议在任务6开始前进行技术调研。

是否确认这个变更并更新项目计划？
```

---

### 场景3：项目进度查询

**用户输入：**
```
现在项目进展怎么样了？还有多少任务没做？
```

**主Agent处理流程：**
```
1. 检测触发关键词："进展"、"任务"
2. 加载当前项目状态
3. 统计任务进度
4. 调用project-breakdown skill
```

**Skill返回结果：**
```json
{
  "projectId": "edu-platform-001",
  "projectName": "在线教育平台",
  "status": "in-progress",
  "progress": {
    "totalTasks": 6,
    "completed": 3,
    "inProgress": 1,
    "pending": 2,
    "blocked": 0,
    "completionRate": 50
  },
  "taskSummary": [
    {
      "taskId": "task-001",
      "title": "项目结构初始化",
      "status": "completed",
      "completedAt": "2026-03-15T09:00:00Z"
    },
    {
      "taskId": "task-002",
      "title": "用户认证系统",
      "status": "completed",
      "completedAt": "2026-03-15T11:30:00Z"
    },
    {
      "taskId": "task-003",
      "title": "课程管理功能（支持直播）",
      "status": "completed",
      "completedAt": "2026-03-15T14:00:00Z"
    },
    {
      "taskId": "task-004",
      "title": "学习进度追踪（含直播）",
      "status": "in-progress",
      "startedAt": "2026-03-15T15:00:00Z"
    },
    {
      "taskId": "task-005",
      "title": "在线测试系统",
      "status": "pending",
      "dependencies": ["task-004"]
    },
    {
      "taskId": "task-006",
      "title": "直播功能集成",
      "status": "pending",
      "dependencies": ["task-002"]
    }
  ],
  "recommendedNextTasks": ["task-004", "task-006"]
}
```

**主Agent响应：**
```
当前项目进展如下：

📊 项目进度概览
- 项目名称：在线教育平台
- 项目状态：进行中
- 完成率：50% (3/6)

✅ 已完成任务 (3个)
1. ✓ 项目结构初始化 - 完成于 09:00
2. ✓ 用户认证系统 - 完成于 11:30
3. ✓ 课程管理功能（支持直播） - 完成于 14:00

🔄 进行中任务 (1个)
4. ◐ 学习进度追踪（含直播） - 进行中

⏳ 待办任务 (2个)
5. ⭕ 在线测试系统 (5h) - 依赖任务4
6. ⭕ 直播功能集成 (8h) - 可以并行

💡 下一步建议
可以同时进行：
- 继续完成"学习进度追踪"
- 开始"直播功能集成"（与其他任务无强依赖）

📈 工时统计
- 已完成：12小时
- 进行中：约2小时
- 剩余：13小时
- 总预计：27小时

需要我继续执行当前任务吗？
```

---

### 场景4：任务完成与状态更新

**用户输入：**
```
学习进度追踪功能已经完成了，更新一下状态
```

**主Agent处理流程：**
```
1. 检测触发关键词："完成"、"更新"
2. 识别任务：学习进度追踪
3. 调用project-breakdown skill
4. 更新任务状态
5. 检查依赖任务
```

**Skill返回结果：**
```json
{
  "updatedTask": {
    "taskId": "task-004",
    "title": "学习进度追踪（含直播）",
    "status": "completed",
    "completedAt": "2026-03-15T17:00:00Z"
  },
  "updatedDependencies": [
    {
      "taskId": "task-005",
      "title": "在线测试系统",
      "previousStatus": "pending",
      "newStatus": "pending", // 仍然有其他依赖
      "dependenciesResolved": ["task-004"]
    }
  ],
  "nextRecommendedTasks": ["task-005", "task-006"]
}
```

**主Agent响应：**
```
太棒了！任务状态已更新。

✅ 任务完成
- 任务：学习进度追踪（含直播）
- 完成时间：17:00
- 实际工时：2小时

📊 项目进度更新
- 完成率：66.7% (4/6)
- 剩余任务：2个

🔄 依赖任务更新
- task-005: 在线测试系统 - 依赖已满足，可以开始

🎯 下一步建议
可以同时进行：
1. 在线测试系统 (5h) - 现在可以开始
2. 直播功能集成 (8h) - 可以并行

📈 项目统计
- 已完成：14小时
- 剩余：13小时
- 进度：51.9%

要开始下一个任务吗？
```

---

## 完整对话流示例

### 完整的MVP开发流程

```
=== 会话1：项目初始化 ===

用户：创建一个博客系统，支持文章发布、评论、标签管理

主Agent：[调用project-breakdown skill]
       返回项目规划和任务清单

       好的，我已经为博客系统创建了项目规划！
       项目类型：Web应用
       技术栈：React + Node.js + MongoDB
       任务数：8个，预计24小时
       ...

用户：开始吧

主Agent：[执行task-001: 项目结构初始化]
       [更新任务状态]

       项目结构初始化完成！
       创建了以下目录：
       - src/
       - public/
       - server/
       ...

=== 会话2：继续开发 ===

用户：继续

主Agent：[检测触发关键词："继续"]
       [调用project-breakdown skill查询状态]
       [执行task-002: 用户认证系统]

       继续执行任务...
       用户认证系统完成！
       实现了注册、登录、JWT认证
       ...

=== 会话3：需求变更 ===

用户：我想增加一个点赞功能

主Agent：[检测触发关键词："增加"]
       [调用project-breakdown skill]
       [分析变更影响]

       已分析新增点赞功能的影响：
       - 新增任务：点赞功能模块
       - 影响任务：文章发布（需展示点赞数）
       - 增加工时：3小时
       是否确认？

用户：确认

主Agent：[更新项目文件]
       [生成新任务]

       已更新项目计划！
       新增任务：点赞功能 (3h)
       总任务数：9个
       ...

=== 会话4：进度查询 ===

用户：项目进展如何

主Agent：[检测触发关键词："进展"]
       [调用project-breakdown skill]
       [返回进度报告]

       项目进展如下：
       - 完成率：44.4% (4/9)
       - 已完成：12小时
       - 剩余：15小时
       下一步建议：文章发布功能
       ...

=== 会话5：继续执行 ===

用户：好，继续

主Agent：[执行task-003: 文章发布功能]
       [更新任务状态]

       文章发布功能完成！
       实现了CRUD操作和富文本编辑
       ...

=== ... 继续后续会话 ... ===

=== 会话N：项目完成 ===

用户：最后一个任务完成了！

主Agent：[更新所有任务状态]
       [生成项目完成报告]

       🎉 恭喜！博客系统MVP已完成！

       项目总结：
       - 完成任务：9/9 (100%)
       - 总工时：27小时
       - 功能模块：6个

       已实现功能：
       ✓ 文章发布和管理
       ✓ 用户认证和权限
       ✓ 评论系统
       ✓ 标签管理
       ✓ 点赞功能
       ✓ ...

       项目文件已保存到：
       .workbuddy/project-breakdown/blog-system-001/
```

---

## 项目文件示例

### current-project.json
```json
{
  "projectId": "edu-platform-001",
  "projectName": "在线教育平台",
  "lastUpdated": "2026-03-15T17:00:00Z",
  "status": "in-progress"
}
```

### project-history/edu-platform-001-tasks.json
```json
{
  "projectId": "edu-platform-001",
  "tasks": [
    {
      "taskId": "task-001",
      "title": "项目结构初始化",
      "category": "setup",
      "priority": "critical",
      "status": "completed",
      "estimatedEffort": "2h",
      "dependencies": [],
      "createdAt": "2026-03-15T08:00:00Z",
      "completedAt": "2026-03-15T09:00:00Z"
    },
    {
      "taskId": "task-002",
      "title": "用户认证系统",
      "category": "core-feature",
      "priority": "critical",
      "status": "completed",
      "estimatedEffort": "4h",
      "dependencies": ["task-001"],
      "createdAt": "2026-03-15T08:00:00Z",
      "completedAt": "2026-03-15T11:30:00Z"
    }
  ]
}
```

### project-history/edu-platform-001-metadata.json
```json
{
  "projectId": "edu-platform-001",
  "projectName": "在线教育平台",
  "projectType": "web-app",
  "techStack": ["react", "nodejs", "mongodb", "typescript", "webrtc"],
  "createdAt": "2026-03-15T08:00:00Z",
  "lastUpdated": "2026-03-15T17:00:00Z",
  "status": "in-progress",
  "summary": "包含课程管理、学习进度跟踪、在线测试、直播功能的在线教育平台",
  "requirements": [
    "课程管理",
    "用户学习进度跟踪",
    "在线测试功能",
    "直播功能"
  ],
  "totalTasks": 6,
  "completedTasks": 4,
  "inProgressTasks": 1,
  "pendingTasks": 1
}
```

### project-history/edu-platform-001-changes.log
```json
[
  {
    "timestamp": "2026-03-15T10:30:00Z",
    "changeType": "requirement-update",
    "description": "新增直播功能需求",
    "details": "学生可以实时观看课程直播",
    "affectedTasks": ["task-003", "task-004"],
    "newTasks": ["task-006"]
  },
  {
    "timestamp": "2026-03-15T17:00:00Z",
    "changeType": "task-completion",
    "description": "完成任务：学习进度追踪（含直播）",
    "taskId": "task-004",
    "taskDuration": "2h"
  }
]
```

---

## 故障排查示例

### 示例1：项目文件丢失

**用户：**
```
我发现项目文件不见了，能恢复吗？
```

**主Agent处理：**
```
1. 检查备份文件
2. 提供恢复选项
3. 重建项目状态（如果备份不可用）
```

**响应：**
```
检测到项目文件丢失，让我帮您处理：

📁 检查备份文件
- 发现备份：edu-platform-001-backup-20260315.json
- 备份时间：2026-03-15 16:00:00

🔄 恢复选项
1. 从备份恢复（推荐）
   - 恢复到16:00的状态
   - 会丢失17:00之后的更改

2. 重建项目状态
   - 基于对话历史重建
   - 需要您确认项目信息

选择哪种方式？
```

### 示例2：任务状态不一致

**用户：**
```
任务状态好像不对，task-005显示未开始，但我已经完成了
```

**主Agent处理：**
```
1. 检测状态冲突
2. 验证实际完成情况
3. 提供修正选项
```

**响应：**
```
检测到状态不一致：

⚠️ 冲突任务
- task-005: 在线测试系统
  - 文件记录状态：pending
  - 用户反馈：已完成

🔍 验证选项
1. 确认任务已完成
   - 更新状态为completed
   - 记录完成时间

2. 保持当前状态
   - 保持pending状态
   - 稍后手动更新

请确认任务是否真的已完成？如果是，我会立即更新状态。
```

---

## 总结

这些示例展示了Project Breakdown Skill的核心使用场景：

1. ✅ **项目初始化**：智能切分任务，生成完整规划
2. ✅ **需求变更**：分析影响，动态调整计划
3. ✅ **进度追踪**：实时统计，清晰展示
4. ✅ **状态管理**：自动更新，保持同步
5. ✅ **故障恢复**：备份机制，容错处理

通过这些功能，Skill能够：
- 减少30-50%的token消耗
- 提升项目稳定性
- 加快开发效率
- 保持上下文一致性

---

**版本**: 1.0.0
**最后更新**: 2026-03-15
