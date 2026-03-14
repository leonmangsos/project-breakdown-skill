---
name: project-breakdown
description: "This skill should be used when the user requests project task breakdown, task management, or MVP planning. It intelligently decomposes projects into executable task modules, tracks task status persistently, and manages requirement changes — reducing 30-50% token usage and boosting development efficiency 2-3x."
---

# Project Breakdown Skill

智能项目任务切分与管理技能，用于在MVP生成时进行任务分解、记录和状态追踪，避免每次交互时重新进行项目分析，提升稳定性和效率，减少token消耗。

## When to Use This Skill
**自动触发场景**（无需手动调用，由主Agent在以下情况自动介入）：
1. **项目初始化阶段**：用户提出新项目需求（如"创建一个xxx应用"、"开发xxx功能"）时，首次进行项目分析和任务切分
2. **需求回溯阶段**：当用户要求查看、修改、回顾项目需求或任务进度时（如"之前说了什么"、"修改某个功能"、"现在进展如何"）
3. **上下文恢复阶段**：在对话中涉及已存在项目的功能修改、bug修复、需求调整时
4. **项目状态查询**：用户询问项目整体情况、已完成任务、剩余任务等

**明确调用场景**（用户或主Agent主动触发）：
- "项目切分"、"任务分解"、"生成任务清单"
- "查看项目状态"、"当前进度"、"任务列表"
- "修改项目计划"、"调整任务"、"更新需求"
- "继续之前的项目"、"恢复项目"

## What This Skill Does

### 核心功能
1. **智能项目切分**
   - 分析用户需求，自动识别项目类型、技术栈、核心功能
   - 将项目拆解为可执行的、独立的任务模块
   - 为每个任务定义：优先级、依赖关系、预估复杂度、验收标准

2. **项目状态持久化**
   - 将任务清单、项目元数据保存到本地文件系统
   - 记录每个任务的执行状态：未开始、进行中、已完成、已阻塞
   - 追踪任务变更历史（需求变更、任务拆分、状态更新）

3. **智能状态恢复**
   - 在每次交互开始时自动检查是否已存在项目上下文
   - 加载最新的项目状态和任务清单
   - 判断当前操作是否需要更新项目规划或直接基于现有任务继续

4. **需求变更管理**
   - 当需求变更时，智能分析变更影响范围
   - 更新相关任务状态、依赖关系和优先级
   - 提供变更影响报告

### 文件结构
```
.workbuddy/
└── project-breakdown/
    ├── current-project.json          # 当前激活项目信息
    ├── project-history/              # 项目历史记录
    │   ├── {project-id}-tasks.json   # 任务清单
    │   ├── {project-id}-metadata.json # 项目元数据
    │   └── {project-id}-changes.log   # 变更日志
    └── templates/                     # 任务模板
        ├── web-app.json
        ├── api-service.json
        └── cli-tool.json
```

## Data Models

### 项目元数据 (project-metadata.json)
```json
{
  "projectId": "uuid",
  "projectName": "项目名称",
  "projectType": "web-app|api-service|cli-tool|library",
  "techStack": ["react", "nodejs", "typescript"],
  "createdAt": "ISO8601 timestamp",
  "lastUpdated": "ISO8601 timestamp",
  "status": "planning|in-progress|completed|on-hold",
  "summary": "项目简述",
  "requirements": ["核心需求1", "核心需求2"],
  "stakeholders": ["产品负责人", "开发者"]
}
```

### 任务清单 (tasks.json)
```json
{
  "projectId": "uuid",
  "tasks": [
    {
      "taskId": "task-001",
      "title": "任务标题",
      "description": "详细描述",
      "category": "setup|core-feature|enhancement|bugfix|testing|docs",
      "priority": "critical|high|medium|low",
      "status": "pending|in-progress|completed|blocked",
      "dependencies": ["task-000"],
      "estimatedEffort": "hours",
      "acceptanceCriteria": ["验收标准1", "验收标准2"],
      "assignedTo": "agent-name",
      "createdAt": "ISO8601 timestamp",
      "completedAt": "ISO8601 timestamp",
      "notes": "备注信息"
    }
  ]
}
```

## Usage Flow

### 场景1：新项目初始化
1. **检测触发**：用户提出新项目需求
2. **执行项目分析**：
   - 提取项目类型、技术栈、核心功能
   - 生成项目ID（UUID）
   - 创建项目元数据文件
3. **执行任务切分**：
   - 使用模板或智能分析生成任务清单
   - 设置任务依赖关系和优先级
   - 保存任务清单文件
4. **返回结果**：
   - 项目概览
   - 任务清单摘要
   - 建议的执行顺序

### 场景2：需求变更处理
1. **检测触发**：用户修改需求或要求调整
2. **加载项目状态**：读取当前项目元数据和任务清单
3. **影响分析**：
   - 对比新旧需求
   - 识别受影响的任务
   - 计算变更范围
4. **更新项目文件**：
   - 更新需求列表
   - 修改相关任务描述、依赖关系
   - 记录变更日志
5. **返回变更报告**：变更摘要、影响任务列表、新任务清单

### 场景3：状态查询与恢复
1. **检测触发**：用户询问项目状态或相关操作
2. **加载项目状态**：检查是否存在活跃项目
3. **返回状态摘要**：
   - 项目概览
   - 任务进度统计（总数、完成数、进行中、阻塞）
   - 下一步建议

### 场景4：任务完成追踪
1. **检测触发**：任务完成或状态更新
2. **更新任务状态**：修改对应任务的status字段
3. **更新时间戳**：记录完成时间
4. **更新依赖任务**：检查并更新依赖该任务的其他任务状态
5. **保存文件**：持久化变更

## Auto-Intervention Logic

### 每回合交互判定流程
主Agent在每个交互回合开始时执行以下判定：

```
IF 对话上下文中存在项目相关信息：
    IF 本地存在 .workbuddy/project-breakdown/current-project.json：
        加载项目状态
        分析用户当前输入
        IF 用户输入涉及：
            - 查看项目/任务状态
            - 修改需求/功能
            - 继续某个任务
            - 询问项目进度
            - 回顾之前的需求
            THEN 调用本Skill处理
        ELSE：
            继续正常对话
    ELSE：
        IF 用户提出新项目需求：
            调用本Skill初始化项目
        ELSE：
            继续正常对话
ELSE：
    IF 用户提出新项目需求：
        调用本Skill初始化项目
    ELSE：
        继续正常对话
```

### 触发信号检测
主Agent需检测以下信号：
- **新建项目信号**："创建"、"开发"、"新建"、"开始一个"、"我要做个"等词汇 + 项目描述
- **查询状态信号**："进度"、"任务"、"状态"、"完成情况"、"进展"等
- **修改需求信号**："修改"、"调整"、"改变"、"更新" + 功能/需求
- **回顾信号**："之前"、"刚才"、"我们讨论的"、"现在的需求"
- **继续项目信号**："继续"、"接下来"、"下一步"（在已有项目上下文中）

## Best Practices

1. **任务粒度控制**：每个任务应该是可在一个会话中完成的工作单元
2. **依赖关系管理**：明确标记任务依赖，避免执行顺序错误
3. **状态实时更新**：任务状态变更后立即保存文件
4. **变更可追溯**：所有需求变更和任务调整都记录日志
5. **清晰的验收标准**：每个任务都有明确的完成标准
6. **优先级合理分配**：优先处理关键路径上的任务

## Integration with Main Agent

### 主Agent集成点
1. **对话开始前**：检查是否有活跃项目，加载上下文
2. **用户意图分析**：在意图识别阶段判断是否需要调用本Skill
3. **任务执行后**：更新任务状态和完成时间
4. **需求变更时**：调用本Skill进行影响分析和更新
5. **项目阶段转换**：识别项目阶段变化（如从规划到实施）并更新状态

### 数据交换
```javascript
// 主Agent调用本Skill
const result = await callSkill("project-breakdown", {
  action: "initialize|query|update|complete-task",
  projectContext: {...}, // 当前对话中的项目上下文
  userInput: "...",      // 用户输入
  taskStatus: {...}      // 可选：当前任务状态
});

// 返回格式
{
  projectId: "uuid",
  projectMetadata: {...},
  tasks: [...],
  recommendedNextTasks: [...],
  changeReport: {...},   // 如果有变更
  actionTaken: "初始化项目/查询状态/更新需求/完成任务"
}
```

## Error Handling

1. **文件系统错误**：如果读写文件失败，使用内存状态并记录警告
2. **数据损坏**：如果项目文件损坏，提示用户并提供恢复选项
3. **版本冲突**：如果检测到项目文件被外部修改，提供合并选项
4. **缺少依赖**：如果依赖任务未完成，标记当前任务为"blocked"状态

## Performance Optimization

1. **增量更新**：只保存变更的任务和字段，避免全量重写
2. **懒加载**：只在需要时加载完整任务清单，平时只加载元数据
3. **缓存机制**：缓存当前项目状态，减少文件读取
4. **智能压缩**：变更日志定期归档压缩

## Extension Points

1. **自定义模板**：支持用户自定义项目类型模板
2. **集成CI/CD**：可与自动化构建流程集成
3. **多项目支持**：支持同时管理多个项目
4. **协作模式**：支持多Agent协作的项目管理
