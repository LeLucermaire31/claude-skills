# claude-skills

Claude Code Skills 集合。

## superpowers-workflow

全流程项目工作流编排 Skill。从需求分析到代码交付，自动判断项目类型并选择最优路径（分阶段把控 / 自动执行 / 混合模式），协调 Superpowers、Ralph Loop、Caveman 和领域 Skill 协作。

### 安装

```bash
# 将 .skill 文件或目录复制到 Claude Code skills 目录
cp superpowers-workflow.skill ~/.claude/skills/
# 或
cp -r superpowers-workflow/ ~/.claude/skills/superpowers-workflow/
```

### 使用

触发命令：`/workflow`、`/prd-flow`，或直接说"开始工作流"。

详细教程见 [superpowers-workflow-tutorial.md](superpowers-workflow-tutorial.md)。

---

## 依赖与致谢

本 Skill 基于以下开源项目，向原作者表示由衷感谢。

| 依赖 | 作者 | 仓库 | 许可证 |
|------|------|------|--------|
| **Superpowers** | Jesse Vincent ([@obra](https://github.com/obra)) + [Prime Radiant](https://github.com/obra) | [obra/superpowers](https://github.com/obra/superpowers) | MIT |
| **Ralph Loop** | [baleen37](https://github.com/baleen37) | [baleen37/claude-plugins](https://github.com/baleen37/claude-plugins) | MIT |
| **Caveman** | Julius Brussee | [JuliusBrussee/caveman](https://github.com/JuliusBrussee/caveman) | MIT |
| **gstack** | Garry Tan | [garrytan/gstack](https://github.com/garrytan/gstack) | MIT |

### 许可证兼容性说明

上述四个项目均采用 **MIT 许可证**，允许自由使用、修改、分发及二次创作，只需保留原始版权声明和许可证文本。

本项目同样以 **MIT 许可证** 发布。详见 [LICENSE](LICENSE)。

---

## 文件结构

```
claude-skills/
├── README.md                               # 本文件
├── LICENSE                                 # MIT 许可证
├── superpowers-workflow-tutorial.md        # 安装使用教程
├── superpowers-workflow.skill              # Skill 安装包
└── superpowers-workflow/                   # Skill 源码
    ├── SKILL.md                            # Skill 主定义
    ├── commands/
    │   └── workflow.md                     # 命令入口
    ├── references/
    │   ├── stage-gates.md                  # 阶段门控规则
    │   └── ralph-loop-params.md            # Ralph Loop 参数参考
    └── evals/
        └── evals.json                      # 评估用例
```
