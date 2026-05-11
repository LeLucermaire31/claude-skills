---
name: superpowers-workflow
description: 全流程项目工作流编排。从需求分析到代码交付，自动判断项目类型并选择最优路径（分阶段把控/自动执行/混合模式），协调 superpowers、ralph-loop、caveman 和领域 Skill 协作。触发：/workflow、/prd-flow、"开始工作流"、"启动项目流程"、"新项目怎么开始"。
compatibility: 需要 superpowers 插件、ralph-loop 插件、caveman 插件、gstack skill（浏览器 QA）
---

# Superpowers + Ralph Loop 工作流

全程 caveman full 模式压缩 token。三路径（A/B/C）由 Phase 0 判定后自动路由。

## 核心原则

- **Superpowers** = 确定性工作：计划、验证、实现、审查。结构化流程，有人工确认节点。
- **Ralph Loop** = 迭代性工作：打磨、优化、完善。循环直到 completion-promise 成立。
- **Caveman** = 全程 token 压缩。子代理用 cavecrew 预设。
- **Skill 注入** = 必须实际调用 Skill 工具，禁止只口头提及。
- **人工确认** = Phase 0 判定后 + A/C 类设计节点后。不在中途打断自动流程。

## Phase 0: 需求分析器（路由层）

收到用户需求后，先不要执行任何工作。完成以下分析并输出给用户确认。

### 步骤

1. 判定项目类型（A/B/C），使用下方判定表
2. 评估复杂度（低/中/高）
3. 选择对应的执行路径
4. 列出所有人工决策点
5. 列出建议注入的领域 Skill（参考 Skill 映射表）

### 类型判定表

| 维度 | A 类（分阶段把控） | B 类（自动执行） | C 类（混合模式） |
|------|-------------------|-----------------|-----------------|
| 需求明确度 | 模糊、需探索 | 清晰、边界明确 | 中等、有模糊点 |
| 复杂度 | 高 | 低 | 中-高 |
| 风险等级 | 需人工层层把关 | 可自动完成 | 混合，关键点把关 |
| 典型示例 | 新系统设计、架构重构 | 修 bug、小功能、重构 | 大功能迭代、多模块开发 |
| 预计 token | 高（但 caveman 压缩） | 低 | 中-高 |

### Skill 映射表

| 项目场景 | 注入的领域 Skill |
|---------|-----------------|
| 前端/Web UI | `ui-ux-pro-max` |
| 移动端 UI | `ui-ux-pro-max` (React Native/Flutter/SwiftUI) |
| 后端 API 开发 | `claude-api`（如果调 Anthropic SDK） |
| 全栈项目 | `ui-ux-pro-max` + 后端对应 skill |
| 数据处理/脚本 | 询问用户有无对应领域 skill |
| Web QA/浏览器测试 | `gstack` — headless 浏览器截图、交互验证、响应式测试 |
| 通用/不确定 | 仅用 superpowers 核心，不注入领域 skill |

### Phase 0 输出格式

向用户输出以下分析结果：

```
## 项目分析

- **类型判定**: [A/B/C 类]
- **复杂度**: [低/中/高]
- **执行路径**: [简述选定路径的阶段]
- **人工决策点**: [列出所有确认节点]
- **建议注入 Skill**: [列出 skill 名称 + 注入阶段]
- **Ralph 参数**: [max-iterations 建议值 + completion-promise 建议]

确认后进入 Phase 1。
```

等待用户确认（可调整类型、参数、skill 选择）后再进入 Phase 1。

**Token**: 产出已保存，建议 `/compact` 后继续。

## Phase 1: 执行阶段

根据 Phase 0 判定的类型，进入对应执行路径。

---

### A 类路径: 分阶段把控

适用：高复杂度、需求模糊、需要人工层层把关。

#### Phase 1a: Superpowers — 设计探索

**目标**: 澄清需求，产出设计文档。

1. 激活 `brainstorming` skill — 探索用户意图、需求、约束
2. 【条件】如果 Phase 0 选定了领域 Skill（如 `ui-ux-pro-max`），在此阶段注入调用
3. 产出设计文档到 `docs/superpowers/specs/YYYY-MM-DD-项目名-design.md`
4. **人工确认节点** — 展示设计摘要，等待用户确认

不通过 → 调整设计 → 再确认。通过 → 进入 Phase 1b。

**Token**: 产出已保存，建议 `/compact` 后继续。

#### Phase 1b: Superpowers — 任务拆分

**目标**: 基于设计产出可执行的实现计划。

1. 激活 `writing-plans` skill
2. 基于 Phase 1a 设计文档产出实现计划
3. 每个任务粒度：2-5 分钟可完成，含确切文件路径
4. 产出到 `docs/superpowers/plans/YYYY-MM-DD-项目名-plan.md`
5. **人工确认节点** — 展示任务列表，等待用户确认

不通过 → 调整计划。通过 → 进入 Phase 1c。

**Token**: 产出已保存，建议 `/compact` 后继续。

#### Phase 1c: Superpowers — 执行实现

**目标**: 按计划写代码。

1. 激活 `using-git-worktrees` — 创建隔离工作空间
2. 激活 `subagent-driven-development` — 按任务分派子代理
3. 每个子任务: implementer → spec-reviewer → code-quality-reviewer → 通过
4. 子代理使用 `cavecrew` 预设压缩输出
5. 所有任务通过 → Phase 1d

**Token**: 产出已保存，建议 `/compact` 后继续。

#### Phase 1d: Superpowers — 验证审查

**目标**: 交付前把关。

1. 激活 `requesting-code-review` — 整体代码审查
2. 激活 `verification-before-completion` — 运行验证命令，确认通过

**任意一项不通过 → 回退 Phase 1c** 针对性修复，重新审查。

**Token**: 产出已保存，建议 `/compact` 后继续。

#### Phase 1e: Superpowers — 收尾

1. 激活 `finishing-a-development-branch` — 选择 merge / PR / 保留 / 丢弃
2. 清理工作空间
3. 输出交付摘要

**Token**: 产出已保存，建议 `/compact` 后继续。

---

### B 类路径: 自动执行

适用：低复杂度、需求清晰、可自动完成。

#### Phase 1: 任务确认

Phase 0 分析结果中已包含任务描述。向用户展示任务摘要并确认。
确认后设置 `using-git-worktrees` 隔离。

**Token**: 产出已保存，建议 `/compact` 后继续。

#### Phase 2: Ralph Loop — 自动迭代执行

1. 启动 `/ralph-loop`，参数：

| 复杂度 | --max-iterations | --completion-promise |
|--------|-----------------|---------------------|
| 低 | 3 | 可选 |
| 中 | 5 | 建议设置 |
| 高 | 8 | 必须设置 |

2. 【条件】如果 Phase 0 选定了领域 Skill，在 Ralph prompt 中明确要求调用
3. Ralph 每轮自动改进，看到上一轮产出后迭代
4. 退出条件：completion-promise 成立 或 达到 max-iterations

**Token**: 产出已保存，建议 `/compact` 后继续。

#### Phase 3: 终审

根据项目类型选择终审方式：

| 项目类型 | 终审工具 |
|---------|---------|
| Web 前端/UI | `gstack` — 浏览器 QA（见下方 gstack QA 流程） |
| 后端 API / CLI / 库 | `verification-before-completion` |
| 全栈 | `gstack` + `verification-before-completion` |
| 通用/不确定 | `requesting-code-review` |

**gstack QA 流程（Web/UI 项目）：**

1. 激活 `gstack` skill
2. 启动项目 dev server，获取 URL
3. 执行 QA 检查：
   - `$B goto <url>` — 导航到目标页面
   - `$B snapshot -i` — 获取交互元素清单
   - `$B snapshot -a -o /tmp/qa-annotated.png` — 标注截图留证
   - `$B is visible "<关键元素>"` — 断言关键元素可见
   - `$B console --errors` — 检查 JS 错误
   - `$B network` — 检查网络请求状态
   - `$B responsive /tmp/qa-layout` — 移动/平板/桌面三端截图
4. 核心用户流程走查：
   - `$B snapshot -i` → baseline
   - 点击/填写关键步骤
   - `$B snapshot -D` → diff 验证每步变化符合预期
5. 产出 QA 报告：截图 + 断言结果 + 问题清单

**gstack 验证通过标准：**
- 所有关键元素 `is visible` 全部 true
- `console --errors` 无 JS 异常
- 核心用户流程 diff 链符合预期
- 三端截图无布局崩溃

**不通过 → 回退 Phase 2**（最多 1 次）。超限 → 人工介入，输出问题报告。

**Token**: 产出已保存，建议 `/compact` 后继续。

---

### C 类路径: 混合模式

适用：中高复杂度、需求中等明确度。关键节点人工把关，执行阶段自动。

#### Phase 1a: Superpowers — 设计探索

同 A 类 Phase 1a。产出设计文档 → 人工确认。

**Token**: 产出已保存，建议 `/compact` 后继续。

#### Phase 1b: Superpowers — 任务拆分

同 A 类 Phase 1b。产出实现计划 → 人工确认。设置 `using-git-worktrees` 隔离。

**Token**: 产出已保存，建议 `/compact` 后继续。

#### Phase 2: Ralph Loop — 自动执行

1. 启动 `/ralph-loop`，参数按 C 类设置：

| 复杂度 | --max-iterations | --completion-promise |
|--------|-----------------|---------------------|
| 中 | 5 | 建议设置 |
| 高 | 8 | 必须设置 |

2. 【条件】如果 Phase 0 选定了领域 Skill，在 Ralph prompt 中明确要求调用
3. 每轮自动执行 + review + 改进

**回滚机制**：Ralph 迭代期间，以下条件任一触发 → 立即回退 Phase 1b：

| 触发条件 | 判定标准 |
|---------|---------|
| 测试连续失败 | 同一测试 2 轮均 FAIL |
| Review 不通过 | 出现 Critical 或 ≥2 个 Important issue |
| 无实质改进 | 连续 2 轮 reviewer 无新增通过的检查项 |

回滚后重新评估任务拆分。重试上限 2 次。超限 → 人工介入，输出详细问题报告。

**Token**: 产出已保存，建议 `/compact` 后继续。

#### Phase 3: 终审

同 B 类 Phase 3（根据项目类型选择终审工具）。

**不通过 → 回退 Phase 2**（最多 1 次，不消耗 Phase 1b 重试次数）。

**Token**: 产出已保存，建议 `/compact` 后继续。

#### Phase 4: Superpowers — 收尾

同 A 类 Phase 1e。`finishing-a-development-branch` 收尾。

**Token**: 产出已保存，建议 `/compact` 后继续。

---

## 全流程通用规则

### Caveman 模式

- 全阶段保持 caveman full
- 子代理一律用 `cavecrew` 预设类型（输出压缩 ~60%）
- 迭代中不重复输出上一轮已有信息
- 代码、commit message、PR description 正常写，不压缩

### 工作空间隔离

- A 类: Phase 1c 处激活 `using-git-worktrees`
- B 类: Phase 1 确认后激活
- C 类: Phase 1b 处激活
- 不确定是否需要 → 询问用户

### Skill 注入规则

- "调用" = 实际使用 `Skill` 工具调用，严禁只说"建议用 xxx skill"而不调用
- 注入时机: Phase 0 确认后，对应执行阶段开始前
- 优先级: 用户指令 > Skill 指令 > 默认行为
- 不确定是否注入 → 询问用户选择

### 人工确认节点

必须的输出格式：展示摘要 + 明确标注"请确认后继续"。

| 路径 | 确认节点 |
|------|---------|
| 所有 | Phase 0 分析结果 |
| A 类 | Phase 1a 设计后、Phase 1b 计划后 |
| B 类 | Phase 1 任务确认 |
| C 类 | Phase 1a 设计后、Phase 1b 计划后 |

### 回滚规则

| 回滚类型 | 适用范围 | 回退目标 | 上限 |
|---------|---------|---------|------|
| 验证不通过 | A 类 Phase 1d | Phase 1c | 不限，修到通过 |
| 终审不通过 | B 类 Phase 3, C 类 Phase 3 | 执行阶段 | 1 次 |
| Ralph 遇错 | C 类 Phase 2 | Phase 1b | 2 次 |

超限后操作：
1. 输出详细问题报告（什么失败、尝试了什么、为什么失败）
2. 建议人工介入点
3. 等待用户决策

### Token 优化

每个阶段完成后提醒用户执行 `/compact`。产出已保存到磁盘，过程讨论可丢弃。

**compact 触发表**：

| 触发点 | A | B | C |
|--------|---|---|---|
| Phase 0 确认后 | ✓ | ✓ | ✓ |
| 设计确认后 | ✓ | — | ✓ |
| 计划确认后 | ✓ | ✓ | ✓ |
| 实现完成 | ✓ | — | — |
| 验证通过 | ✓ | — | — |
| Ralph 退出 | — | ✓ | ✓ |
| 终审通过 | — | ✓ | ✓ |
| 收尾完成 | ✓ | — | ✓ |

**`/clear` 规则**：
- 不自动触发，仅建议
- 上下文使用超 80% 时，在最近安全确认点后建议用户执行
- 安全点：Phase 0 确认后、设计/计划文档保存后
- 必须用户确认才执行
