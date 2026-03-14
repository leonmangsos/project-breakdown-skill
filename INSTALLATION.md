# Project Breakdown Skill 安装指南

## 前置要求

- WorkBuddy Agent Framework (需要支持Skill加载机制)
- Node.js 18+ (如果使用相关工具)
- 读写文件系统权限

## 安装步骤

### 方法1：直接复制安装

1. **下载Skill文件**
   ```bash
   # 复制整个skill目录到WorkBuddy的skills目录
   cp -r "03-15 自动化流程skill" ~/.workbuddy/skills/project-breakdown
   ```

2. **验证安装**
   ```bash
   # 检查目录结构
   ls -la ~/.workbuddy/skills/project-breakdown/

   # 应该看到以下文件：
   # SKILL.md
   # README.md
   # config.json
   # templates/
   #   ├── web-app.json
   #   ├── api-service.json
   #   └── cli-tool.json
   ```

### 方法2：通过WorkBuddy Marketplace安装（如果支持）

```bash
# 假设WorkBuddy支持marketplace安装
wb skill install project-breakdown
```

### 方法3：手动创建目录结构

如果上面的方法不适用，可以手动创建：

```bash
# 创建技能目录
mkdir -p ~/.workbuddy/skills/project-breakdown
mkdir -p ~/.workbuddy/skills/project-breakdown/templates

# 下载或复制所有文件到该目录
# 确保包含：
# - SKILL.md
# - README.md
# - config.json
# - templates/*.json
```

## 配置

### 1. 基础配置

编辑 `config.json` 文件以自定义行为：

```json
{
  "autoSave": true,              // 自动保存项目状态
  "autoArchive": true,          // 自动归档旧项目
  "archiveAfterDays": 30,       // 归档天数
  "defaultPriority": "medium",  // 默认任务优先级
  "enableChangeLog": true,      // 启用变更日志
  "enableAutoIntervention": true // 启用自动介入
}
```

### 2. 触发关键词配置

根据需要修改触发关键词：

```json
{
  "triggerKeywords": {
    "newProject": ["创建", "开发", "新建", ...],
    "queryStatus": ["进度", "状态", "任务", ...],
    ...
  }
}
```

### 3. 项目路径配置

设置自定义项目存储路径（可选）：

```bash
# 在config.json中设置
{
  "projectDirectory": "/custom/path/to/projects"
}

# 或通过环境变量
export WORKBUDDY_PROJECT_PATH="/custom/path/to/projects"
```

## 集成到主Agent

### 修改主Agent提示词

在主Agent的系统提示词中添加以下内容：

```
# Project Breakdown Skill 集成说明

在每个交互回合开始时，主Agent需要执行以下检查流程：

1. **检查项目上下文**
   - 检查是否存在 .workbuddy/project-breakdown/current-project.json
   - 如果存在，加载项目状态
   - 如果不存在，继续正常对话

2. **分析用户意图**
   - 检测用户输入是否触发以下场景：
     a. 新项目创建（"创建"、"开发"、"新建"等）
     b. 需求回顾（"之前"、"回顾"、"查看"等）
     c. 需求变更（"修改"、"增加"、"调整"等）
     d. 进度查询（"进度"、"状态"、"完成"等）
     e. 继续项目（"继续"、"接下来"、"下一步"等）

3. **判断是否调用Skill**
   - 如果检测到上述触发场景
   - AND (存在活跃项目 OR 涉及新项目创建)
   - THEN 调用 project-breakdown skill

4. **Skill调用格式**
   调用 skill-tool: project-breakdown
   参数：
   - action: "initialize|query|update|complete-task"
   - projectContext: 当前对话中的项目上下文（如果有）
   - userInput: 用户的原始输入
   - taskStatus: 可选，当前任务的状态信息

5. **处理Skill返回**
   - 接收项目状态、任务清单、建议操作
   - 将项目信息纳入上下文
   - 基于返回结果继续对话或执行任务
```

### 代码集成示例

如果主Agent使用编程方式集成：

```javascript
// 主Agent代码示例

class MainAgent {
  constructor() {
    this.projectBreakdownSkill = new ProjectBreakdownSkill();
    this.activeProject = null;
  }

  async processUserInput(userInput) {
    // 1. 加载项目状态
    await this.loadProjectState();

    // 2. 检测触发条件
    const triggerResult = await this.detectTrigger(userInput);

    if (triggerResult.shouldTrigger) {
      // 3. 调用Skill
      const skillResult = await this.projectBreakdownSkill.execute({
        action: triggerResult.action,
        projectContext: this.activeProject,
        userInput: userInput,
        taskStatus: triggerResult.taskStatus
      });

      // 4. 更新项目状态
      this.activeProject = skillResult.project;

      // 5. 继续处理
      return await this.handleSkillResult(skillResult);
    } else {
      // 正常对话流程
      return await this.normalDialogue(userInput);
    }
  }

  async detectTrigger(userInput) {
    const keywords = {
      newProject: ["创建", "开发", "新建", ...],
      queryStatus: ["进度", "状态", "任务", ...],
      ...
    };

    // 实现触发检测逻辑
    // ...
  }
}
```

## 测试安装

### 1. 基础功能测试

```bash
# 测试Skill是否被正确加载
wb skill list
# 应该看到 project-breakdown 在列表中

# 测试Skill元数据
wb skill info project-breakdown
# 应该显示Skill的详细信息
```

### 2. 交互测试

在WorkBuddy中测试以下场景：

#### 测试1：新项目创建
```
用户：创建一个在线教育平台

预期结果：
1. 主Agent检测到触发
2. 调用project-breakdown skill
3. 返回项目概览和任务清单
4. 开始执行第一个任务
```

#### 测试2：状态查询
```
用户：现在项目进展如何

预期结果：
1. 主Agent检测到触发
2. 调用project-breakdown skill
3. 返回进度报告
4. 显示任务完成情况
```

#### 测试3：需求变更
```
用户：增加一个直播功能

预期结果：
1. 主Agent检测到触发
2. 调用project-breakdown skill
3. 分析变更影响
4. 返回更新后的任务清单
```

## 常见安装问题

### 问题1：Skill未被识别

**症状**：`wb skill list` 中看不到 project-breakdown

**解决方案**：
```bash
# 检查文件是否在正确位置
ls ~/.workbuddy/skills/project-breakdown/SKILL.md

# 检查SKILL.md格式是否正确
# 必须包含：Description, When to Use, What This Skill Does 等章节

# 重启WorkBuddy服务
wb restart
```

### 问题2：自动介入不工作

**症状**：主Agent不会自动调用该Skill

**解决方案**：
```bash
# 1. 检查配置
cat ~/.workbuddy/skills/project-breakdown/config.json
# 确保 "enableAutoIntervention": true

# 2. 检查主Agent是否正确集成
# 确保主Agent的提示词中包含了集成说明

# 3. 检查触发关键词
# 确认配置中的关键词与用户的实际输入匹配
```

### 问题3：文件权限问题

**症状**：无法创建或写入项目文件

**解决方案**：
```bash
# 检查目录权限
ls -la ~/.workbuddy/

# 修复权限
chmod 755 ~/.workbuddy/
chmod -R 755 ~/.workbuddy/skills/

# 或使用sudo（不推荐）
sudo chmod -R 755 ~/.workbuddy/
```

### 问题4：配置文件格式错误

**症状**：启动时提示JSON解析错误

**解决方案**：
```bash
# 验证JSON格式
cat ~/.workbuddy/skills/project-breakdown/config.json | python3 -m json.tool

# 如果有错误，使用备份文件恢复
cp ~/.workbuddy/skills/project-breakdown/config.json.backup \
   ~/.workbuddy/skills/project-breakdown/config.json
```

## 升级

### 从旧版本升级

```bash
# 1. 备份当前配置
cp ~/.workbuddy/skills/project-breakdown/config.json \
   ~/.workbuddy/skills/project-breakdown/config.json.backup

# 2. 备份项目数据（如果需要）
cp -r ~/.workbuddy/project-breakdown/ \
   ~/.workbuddy/project-breakdown-backup/

# 3. 下载新版本
# 替换 SKILL.md, README.md, config.json, templates/ 等文件

# 4. 恢复配置（如果需要）
cp ~/.workbuddy/skills/project-breakdown/config.json.backup \
   ~/.workbuddy/skills/project-breakdown/config.json

# 5. 重启WorkBuddy
wb restart
```

## 卸载

```bash
# 1. 停止使用该Skill
# 在主Agent配置中移除集成代码

# 2. 可选：删除Skill文件
rm -rf ~/.workbuddy/skills/project-breakdown

# 3. 可选：删除项目数据（谨慎操作）
rm -rf ~/.workbuddy/project-breakdown/

# 4. 重启WorkBuddy
wb restart
```

## 支持与反馈

如果遇到问题或有建议：

1. 查看 README.md 中的使用指南
2. 检查配置文件是否正确
3. 查看WorkBuddy日志
4. 提交Issue或联系技术支持

---

**版本**: 1.0.0
**最后更新**: 2026-03-15
