# superpowers-workflow 安装与使用教程

`superpowers-workflow` 是一个全流程项目工作流编排 Skill，从需求分析到代码交付，自动判断项目类型并选择最优执行路径，协调 superpowers、ralph-loop、caveman 和领域 Skill 协作。

---

## 目录

1. [前置依赖](#前置依赖)
2. [安装步骤](#安装步骤)
3. [快速开始](#快速开始)
4. [核心概念](#核心概念)
5. [三种执行路径](#三种执行路径)
6. [Phase 0：需求分析器](#phase-0需求分析器)
7. [A 类路径：分阶段把控](#a-类路径分阶段把控)
8. [B 类路径：自动执行](#b-类路径自动执行)
9. [C 类路径：混合模式](#c-类路径混合模式)
10. [Skill 注入规则](#skill-注入规则)
11. [回滚机制](#回滚机制)
12. [Token 优化](#token-优化)
13. [常见问题](#常见问题)

---

## 前置依赖

使用本 Skill 前，需安装以下插件/Skill：

| 依赖 | 类型 | 说明 |
|------|------|------|
| **superpowers** | 插件 | 提供 brainstorming、writing-plans、requesting-code-review 等结构化流程 |
| **ralph-loop** | 插件 | 提供 `/ralph-loop` 自动迭代执行能力 |
| **caveman** | 插件 | 全程 token 压缩，子代理使用 cavecrew 预设 |
| **gstack** | Skill | 浏览器 QA（仅 Web/UI 项目需要） |

安装这些依赖的命令：

```bash
# 安装 superpowers 插件（示例，以实际安装方式为准）
claude plugins install superpowers

# 安装 ralph-loop 插件
claude plugins install ralph-loop

# 安装 caveman 插件
claude plugins install caveman

# 安装 gstack skill
claude skills install gstack
```

---

## 安装步骤

### 方法一：直接复制 .skill 文件

1. 下载 `superpowers-workflow.skill` 文件
2. 将其复制到 Claude Code 的 skills 目录：

```bash
# macOS / Linux
cp superpowers-workflow.skill ~/.claude/skills/

# Windows (PowerShell)
Copy-Item superpowers-workflow.skill $env:USERPROFILE\.claude\skills\
```

3. 重启 Claude Code 或重新加载 skills

### 方法二：从源码目录加载

1. 将 `superpowers-workflow/` 整个目录复制到 skills 目录：

```bash
cp -r superpowers-workflow/ ~/.claude/skills/superpowers-workflow/
```

2. 重启 Claude Code

### 验证安装

在 Claude Code 中输入以下命令确认 Skill 已加载：

```
/superpowers-workflow
```

或直接说：

```
开始工作流
```

如果看到 Phase 0 分析输出，说明安装成功。

---

## 快速开始

### 5 秒触发

在 Claude Code 中，用以下任意方式触发工作流：

| 触发方式 | 示例 |
|----------|------|
| 斜杠命令 | `/workflow` 或 `/prd-flow` |
| 自然语言 | "开始工作流"、"启动项目流程"、"新项目怎么开始" |
| 直接描述 | "帮我做一个个人博客，需要明确需求和设计" |

### 第一次使用流程

```
用户: 帮我开发一个 Todo 应用，支持添加、删除、标记完成

AI: [Phase 0 分析]
    - 类型判定: C 类（混合模式）
    - 复杂度: 中
    - 执行路径: 设计→任务拆分→Ralph自动执行→终审→收尾
    - 建议注入 Skill: ui-ux-pro-max + gstack
    - ...

    请确认后继续。

用户: 确认

AI: [Phase 1a: 设计探索 → 产出设计文档 → 再次确认]
    [Phase 1b: 任务拆分 → 产出计划 → 再次确认]
    [Phase 2: Ralph Loop 自动执行]
    [Phase 3: 终审 QA]
    [Phase 4: 收尾]
```

---

## 核心概念

### 三大引擎

```
┌──────────────────────────────────────────────────────┐
│                  superpowers-workflow                  │
│                                                      │
│  ┌──────────────┐  ┌──────────────┐  ┌────────────┐  │
│  │ Superpowers  │  │ Ralph Loop   │  │  Caveman    │  │
│  │ 确定性工作    │  │ 迭代性工作   │  │ Token 压缩  │  │
│  │              │  │              │  │             │  │
│  │ · 计划       │  │ · 自动执行   │  │ · 全程压缩  │  │
│  │ · 验证       │  │ · 循环改进   │  │ · 子代理    │  │
│  │ · 审查       │  │ · 直到完成   │  │   精简输出  │  │
│  └──────────────┘  └──────────────┘  └────────────┘  │
│                                                      │
│  ┌──────────────────────────────────────────────────┐ │
│  │  领域 Skill 注入（ui-ux-pro-max, gstack 等）     │ │
│  └──────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────┘
```

- **Superpowers**：处理确定性工作——计划、验证、实现、审查。结构化流程，有人工确认节点。
- **Ralph Loop**：处理迭代性工作——打磨、优化、完善。循环直到 completion-promise 成立。
- **Caveman**：全程压缩 token 输出，降低上下文消耗。子代理使用 cavecrew 预设。

---

## 三种执行路径

工作流根据 Phase 0 分析结果自动路由到三条路径之一：

| 维度 | **A 类（分阶段把控）** | **B 类（自动执行）** | **C 类（混合模式）** |
|------|----------------------|---------------------|---------------------|
| 需求明确度 | 模糊、需探索 | 清晰、边界明确 | 中等、有模糊点 |
| 复杂度 | 高 | 低 | 中-高 |
| 风险等级 | 需人工层层把关 | 可自动完成 | 混合，关键点把关 |
| 典型场景 | 新系统设计、架构重构 | 修 bug、小功能、重构 | 大功能迭代、多模块开发 |
| 预计 Token | 高 | 低 | 中-高 |

---

## Phase 0：需求分析器

Phase 0 是工作流的入口路由层。**收到需求后，AI 先不做任何实现**，而是完成以下分析并寻求用户确认：

### 分析内容

1. **判定项目类型**（A/B/C），使用类型判定表
2. **评估复杂度**（低/中/高）
3. **选择执行路径**
4. **列出所有人工决策点**
5. **推荐注入的领域 Skill**

### Skill 映射表

| 项目场景 | 注入的领域 Skill |
|---------|-----------------|
| 前端/Web UI | `ui-ux-pro-max` |
| 移动端 UI | `ui-ux-pro-max` (React Native/Flutter/SwiftUI) |
| 后端 API | `claude-api`（调用 Anthropic SDK 时） |
| 全栈项目 | `ui-ux-pro-max` + 后端对应 skill |
| Web QA/浏览器测试 | `gstack` |
| 通用/不确定 | 仅用 superpowers 核心 |

### 输出示例

```
## 项目分析

- **类型判定**: C 类（混合模式）
- **复杂度**: 中
- **执行路径**: 设计探索 → 任务拆分 → Ralph 自动执行 → 终审 → 收尾
- **人工决策点**:
  1. Phase 0 分析确认（当前）
  2. Phase 1a 设计文档确认
  3. Phase 1b 任务计划确认
- **建议注入 Skill**: ui-ux-pro-max（设计阶段）+ gstack（终审 QA）
- **Ralph 参数**: --max-iterations 5, --completion-promise "所有功能通过测试且 UI 无异常"

确认后进入 Phase 1。
```

---

## A 类路径：分阶段把控

适用：高复杂度、需求模糊、需要人工层层把关。

### Phase 1a: Superpowers — 设计探索

- 激活 `brainstorming` skill，探索需求
- 如 Phase 0 选定了领域 Skill，在此阶段注入
- 产出设计文档到 `docs/superpowers/specs/YYYY-MM-DD-项目名-design.md`
- **人工确认节点**：展示设计摘要，等待确认

### Phase 1b: Superpowers — 任务拆分

- 激活 `writing-plans` skill
- 基于设计文档产出实现计划
- 每个任务粒度：2-5 分钟可完成，含确切文件路径
- 产出到 `docs/superpowers/plans/YYYY-MM-DD-项目名-plan.md`
- **人工确认节点**：展示任务列表，等待确认

### Phase 1c: Superpowers — 执行实现

- 激活 `using-git-worktrees` 创建隔离工作空间
- 激活 `subagent-driven-development` 按任务分派子代理
- 每个子任务：implementer → spec-reviewer → code-quality-reviewer → 通过

### Phase 1d: Superpowers — 验证审查

- 激活 `requesting-code-review` 整体审查
- 激活 `verification-before-completion` 运行验证
- **不通过 → 回退 Phase 1c** 修复

### Phase 1e: Superpowers — 收尾

- 激活 `finishing-a-development-branch` 选择 merge/PR/保留/丢弃
- 清理工作空间，输出交付摘要

---

## B 类路径：自动执行

适用：低复杂度、需求清晰、可自动完成。

### Phase 1: 任务确认

展示任务摘要并确认 → 设置 `using-git-worktrees` 隔离。

### Phase 2: Ralph Loop 自动迭代

启动 `/ralph-loop`，参数配置：

| 复杂度 | --max-iterations | --completion-promise |
|--------|-----------------|---------------------|
| 低 | 3 | 可选 |
| 中 | 5 | 建议设置 |
| 高 | 8 | 必须设置 |

Ralph 每轮自动改进，退出条件：completion-promise 成立 或 达到 max-iterations。

### Phase 3: 终审

| 项目类型 | 终审工具 |
|---------|---------|
| Web 前端/UI | `gstack` — 浏览器 QA |
| 后端 API / CLI / 库 | `verification-before-completion` |
| 全栈 | `gstack` + `verification-before-completion` |
| 通用/不确定 | `requesting-code-review` |

**gstack QA 流程（Web/UI 项目）：**
- 导航到目标页面
- 获取交互元素清单
- 标注截图留证
- 断言关键元素可见
- 检查 JS 错误和网络请求
- 移动/平板/桌面三端截图
- 核心用户流程走查

**不通过 → 回退 Phase 2**（最多 1 次）。

---

## C 类路径：混合模式

适用：中高复杂度、需求中等明确度。关键节点人工把关，执行阶段自动。

### 阶段流程

```
Phase 1a (设计探索) ──→ 人工确认
        ↓
Phase 1b (任务拆分) ──→ 人工确认
        ↓
Phase 2  (Ralph Loop 自动执行)
        ↓
Phase 3  (终审 QA)
        ↓
Phase 4  (收尾)
```

### Ralph 参数（C 类）

| 复杂度 | --max-iterations | --completion-promise |
|--------|-----------------|---------------------|
| 中 | 5 | 建议设置 |
| 高 | 8 | 必须设置 |

### 回滚机制

Ralph 迭代期间，以下条件任一触发 → 立即回退 Phase 1b：

| 触发条件 | 判定标准 |
|---------|---------|
| 测试连续失败 | 同一测试 2 轮均 FAIL |
| Review 不通过 | 出现 Critical 或 ≥2 个 Important issue |
| 无实质改进 | 连续 2 轮 reviewer 无新增通过的检查项 |

重试上限 2 次。超限 → 人工介入。

---

## Skill 注入规则

1. **"调用" = 实际执行**：必须使用 `Skill` 工具调用，严禁只说"建议用 xxx skill"
2. **注入时机**：Phase 0 确认后，对应执行阶段开始前
3. **优先级**：用户指令 > Skill 指令 > 默认行为
4. **不确定时**：询问用户选择

---

## 回滚机制

| 回滚类型 | 适用范围 | 回退目标 | 上限 |
|---------|---------|---------|------|
| 验证不通过 | A 类 Phase 1d | Phase 1c | 不限 |
| 终审不通过 | B 类 Phase 3, C 类 Phase 3 | 执行阶段 | 1 次 |
| Ralph 遇错 | C 类 Phase 2 | Phase 1b | 2 次 |

超限后操作：
1. 输出详细问题报告
2. 建议人工介入点
3. 等待用户决策

---

## Token 优化

每个阶段完成后 AI 会提醒执行 `/compact`。关键 compact 触发点：

- Phase 0 确认后
- 设计/计划确认后
- 实现完成
- Ralph 退出
- 终审通过
- 收尾完成

上下文使用超 80% 时，AI 会在最近安全确认点后建议执行 `/clear`。

---

## 常见问题

### Q: Skill 安装后没有生效？

检查 Skill 文件是否放在正确的目录（`~/.claude/skills/`），重启 Claude Code 后重新尝试触发命令。

### Q: Phase 0 判定类型不对怎么办？

在 Phase 0 确认节点可以直接告诉 AI 你想要哪种类型，比如"这个应该用 A 类路径"或"改为 B 类自动执行"。

### Q: 可以跳过某些阶段吗？

可以。在任何人工确认节点告诉 AI 跳过即可，例如"跳过设计文档，直接开始实现"。

### Q: Ralph Loop 卡住了怎么办？

直接告诉 AI 停止当前迭代，AI 会输出当前进度和问题报告，等待你的决策。

### Q: 不想要 Skill 注入怎么办？

在 Phase 0 确认时说明"不需要注入 Skill"即可，工作流会仅使用 superpowers 核心流程。

### Q: gstack 依赖必须安装吗？

仅当你的项目是 Web/UI 类型时才需要。后端/CLI/库项目不需要 gstack。

---

## 文件结构

```
superpowers-workflow/
├── SKILL.md                         # 主 Skill 定义文件
├── commands/
│   └── workflow.md                  # 工作流命令入口
├── references/
│   ├── stage-gates.md               # 阶段门控规则
│   └── ralph-loop-params.md         # Ralph Loop 参数参考
└── evals/
    └── evals.json                   # 评估用例
```
