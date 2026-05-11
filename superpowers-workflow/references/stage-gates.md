# 阶段门控检查清单

## A 类路径

### Phase 1a → 1b 门控
- [ ] 设计文档已写入 `docs/superpowers/specs/`
- [ ] 用户已确认设计方向
- [ ] 如注入领域 Skill，Skill 的输出已纳入设计
- [ ] 非功能需求已覆盖（性能、安全、可访问性等）
- [ ] 建议执行 `/compact` 压缩上下文

### Phase 1b → 1c 门控
- [ ] 实现计划已写入 `docs/superpowers/plans/`
- [ ] 每个任务粒度 2-5 分钟
- [ ] 每个任务含确切文件路径
- [ ] 用户已确认任务列表
- [ ] `using-git-worktrees` 已激活
- [ ] 建议执行 `/compact` 压缩上下文

### Phase 1c → 1d 门控
- [ ] 所有子任务 spec-review 通过
- [ ] 所有子任务 code-quality-review 通过
- [ ] 无残留 Critical/Important issue
- [ ] 建议执行 `/compact` 压缩上下文

### Phase 1d → 1e 门控
- [ ] `requesting-code-review` 通过
- [ ] `verification-before-completion` 验证命令全部通过
- [ ] 所有测试通过
- [ ] 建议执行 `/compact` 压缩上下文

## B 类路径

### Phase 1 → Phase 2 门控
- [ ] 任务描述已向用户确认
- [ ] Ralph Loop 参数已设定（max-iterations + completion-promise）
- [ ] `using-git-worktrees` 已激活
- [ ] 如注入领域 Skill，Skill 已就绪
- [ ] 建议执行 `/compact` 压缩上下文

### Phase 2 → 3 门控
- [ ] Ralph Loop 正常退出（completion-promise 成立 或 max-iterations 达到）
- [ ] 最近一轮产出无 Critical issue
- [ ] 建议执行 `/compact` 压缩上下文

### Phase 3 → 完成 门控

**gstack QA（Web/UI 项目）：**
- [ ] `gstack` skill 已激活，dev server 已启动
- [ ] 所有关键元素 `is visible` 全部 true
- [ ] `console --errors` 无 JS 异常
- [ ] 核心用户流程 `snapshot -D` diff 链符合预期
- [ ] `responsive` 三端截图无布局崩溃
- [ ] QA 报告已产出（截图 + 断言 + 问题清单）

**verification（后端/CLI/库项目）：**
- [ ] `verification-before-completion` 验证命令全部通过
- [ ] 所有测试通过

**通用：**
- [ ] 终审通过
- [ ] 不通过 → 回退 Phase 2（最多 1 次）
- [ ] 建议执行 `/compact` 压缩上下文

## C 类路径

### Phase 1a → 1b 门控
- [ ] 设计文档已产出
- [ ] 用户已确认设计方向
- [ ] 如注入领域 Skill，Skill 的输出已纳入
- [ ] 建议执行 `/compact` 压缩上下文

### Phase 1b → 2 门控
- [ ] 实现计划已产出
- [ ] 用户已确认任务列表
- [ ] `using-git-worktrees` 已激活
- [ ] Ralph Loop 参数已设定
- [ ] 建议执行 `/compact` 压缩上下文

### Phase 2 回滚检查（每轮 Ralph 迭代后）
- [ ] 测试连续 2 轮失败？→ 回退 Phase 1b
- [ ] 出现 Critical issue？→ 回退 Phase 1b
- [ ] 连续 2 轮无实质改进？→ 回退 Phase 1b
- [ ] 回退次数 ≥ 2？→ 人工介入

### Phase 2 → 3 门控
- [ ] Ralph Loop 正常退出
- [ ] 回滚次数 < 2
- [ ] 建议执行 `/compact` 压缩上下文

### Phase 3 → 4 门控
- [ ] 终审通过
- [ ] 不通过 → 回退 Phase 2（最多 1 次）
- [ ] 建议执行 `/compact` 压缩上下文

### Phase 4 → 完成 门控
- [ ] `finishing-a-development-branch` 完成
- [ ] 工作空间清理完成
- [ ] 建议执行 `/compact` 压缩上下文
