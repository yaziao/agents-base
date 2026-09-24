# AI Development Template

一套可以长期复用的通用 AI Development Template，用于把 AI 协作规范、开发工作流和项目长期状态放在同一个可复制的仓库中。

## 项目定位

这个仓库不是某一种语言、框架或架构的项目模板，而是一套通用的软件开发协作规范。

以后创建新项目时，可以直接复制：

- AGENTS.md：稳定、通用的 AI 开发规则
- .agents/skills/：可按需使用的开发工作流
- .ai/：当前项目的长期任务状态和重要架构决策

项目规则不需要一开始就重写通用规则。建议保留通用部分，再随着项目演进逐步补充项目专属约束。

## 核心理念

> 能力完整，流程按需，规则稳定，项目逐步定制。

本模板采用三层结构：

~~~text
AGENTS.md
    ↓
通用 AI 开发规范

.agents/skills/
    ↓
可复用开发工作流

.ai/
    ↓
当前项目的长期任务状态和关键决策
~~~

职责边界如下：

| 层 | 负责什么 |
| --- | --- |
| AGENTS.md | 基本工作原则、修改边界、验证、安全、用户修改保护、任务范围和长任务规则 |
| Superpowers Skills | 具体的软件开发工作流，例如规划、调试、TDD、Review 和多 Agent 协作 |
| Custom Skills | 本模板额外提供的上下文管理、影响分析和通用代码 Review 能力 |
| .ai/ | 当前项目的任务状态和长期有效的架构决策 |

## 目录结构

~~~text
.
├── AGENTS.md
├── README.md
├── LICENSE
│
├── .ai/
│   ├── current-task.md
│   └── decisions.md
│
└── .agents/
    └── skills/
        ├── superpowers/
        │   ├── brainstorming/
        │   ├── writing-plans/
        │   ├── executing-plans/
        │   ├── test-driven-development/
        │   ├── systematic-debugging/
        │   ├── verification-before-completion/
        │   ├── requesting-code-review/
        │   ├── receiving-code-review/
        │   ├── dispatching-parallel-agents/
        │   ├── subagent-driven-development/
        │   ├── diagnosing-superpowers/
        │   └── writing-skills/
        │
        └── custom/
            ├── context-management/
            ├── change-impact-analysis/
            └── code-review/
~~~

## AGENTS.md

AGENTS.md 是稳定的通用规则层，约束 AI 如何工作，但不规定任何具体技术栈或项目架构。

它强调：

- 理解 → 定位 → 最小修改 → 验证
- 只修改解决当前任务所必需的范围
- 保护用户已有修改
- 优先定位根因
- 用真实验证支撑完成结论
- 关注安全、性能、依赖和长期任务上下文
- Skills 根据任务按需使用，而不是每个任务都执行完整流程

项目专属规则可以在新项目中追加。通用规则长期保留，项目规则逐步补充。

## Skills

### Superpowers Skills

本模板采用以下 Superpowers Skills：

- brainstorming
- writing-plans
- executing-plans
- test-driven-development
- systematic-debugging
- verification-before-completion
- diagnosing-superpowers
- requesting-code-review
- receiving-code-review
- dispatching-parallel-agents
- subagent-driven-development
- writing-skills

这些文件来自 Superpowers 官方仓库当前 main 分支，复制时保持原始内容不变。本次同步的提交为 5bf4e78011075bcfc0dc295f0724994cd123ee71。

来源：[obra/superpowers](https://github.com/obra/superpowers)

本模板明确不采用以下 Skills：

- using-git-worktrees：不规定用户必须使用特定 Git 分支、Worktree 或目录隔离流程。
- finishing-a-development-branch：不规定统一的分支收尾、合并或发布流程。
- using-superpowers：本模板在 AGENTS.md 中自行规定 Skills 按需使用，不让 Superpowers 的元 Skill 接管整个 Agent 的行为。

### Custom Skills

本模板自己的 Skills 位于 .agents/skills/custom/：

- context-management：通过 .ai/current-task.md 和 .ai/decisions.md 维护长任务上下文，降低压缩后丢失状态、重复分析和重复修改的风险。
- change-impact-analysis：在重大修改前沿调用方、依赖、数据结构、配置、并发、外部接口和测试检查影响范围，避免无依据扩大重构。
- code-review：以 Critical、High、Medium、Low 描述实际发现的问题，覆盖正确性、安全、并发、资源、性能、兼容性和测试，不进行代码质量打分。

Custom Skills 是本项目自己的内容，不是 Superpowers 的原始文件。

## Skill 使用方式

Skill 能力完整，但必须按需使用。简单任务不应因为模板存在复杂流程而被迫执行复杂流程。

使用原则：

- 先判断任务复杂度和风险，再选择必要的 Skill。
- 只加载能改善当前任务结果的 Skill。
- 一个任务可以组合多个 Skill，也可以只使用一个。
- 未使用某个 Skill 不代表流程不完整，只要完成了当前任务需要的理解、修改和验证。

### 推荐组合

~~~text
简单修改
    → 直接完成并做适当检查

普通 Bug
    → systematic-debugging
    → verification-before-completion

新功能
    → brainstorming
    → writing-plans
    → executing-plans
    → verification-before-completion

大型任务
    → context-management
    → change-impact-analysis
    → 按需组合 brainstorming / writing-plans / executing-plans
    → 按需组合并行 Agent、子 Agent 和 Code Review
    → verification-before-completion
~~~

## 常见工作流

### 简单修改

适用于变量名、日志、注释、文案或边界清晰的小改动。理解现状后直接做最小修改，再进行与改动相称的检查。

### Bug 修复

优先使用 systematic-debugging 查找可复现的根因，再进行最小修复，最后使用 verification-before-completion 记录真实验证结果。

### 新功能

先用 brainstorming 澄清目标和设计；任务较复杂时使用 writing-plans 形成可执行计划，再用 executing-plans 实施。需要时补充 test-driven-development、requesting-code-review 和 verification-before-completion。

### 重构

先判断重构是否是当前目标所必需。重大重构先使用 change-impact-analysis 识别影响范围，再按需规划、分步实施和验证，避免借机扩大到无关模块。

### 大型任务

用 context-management 维护 .ai/ 状态，用 change-impact-analysis 识别边界；根据任务拆分情况选择 dispatching-parallel-agents 或 subagent-driven-development，并在关键阶段加入 Review 和验证。

### Code Review

可以使用 custom/code-review 做通用检查；需要发起或接收协作 Review 时，分别使用 requesting-code-review 和 receiving-code-review。Review 只描述实际问题和证据，不给代码质量打分。

## 如何用于新项目

推荐流程：

~~~text
复制本模板
    ↓
保留通用 AGENTS.md
    ↓
复制 .agents/skills
    ↓
复制 .ai
    ↓
让 Codex / Cursor 分析新项目
    ↓
逐步增加项目专属 AGENTS.md 规则
    ↓
开始开发
~~~

具体建议：

1. 将 AGENTS.md、.agents/skills/ 和 .ai/ 复制到新项目根目录。
2. 先让 AI 阅读项目结构、已有文档和现有约束，识别需要补充的项目规则。
3. 保留通用 AGENTS.md，只在必要位置追加项目专属规则；不要一开始就把整个文件重写掉。
4. 任务开始、进行中和完成时，按需要更新 .ai/current-task.md。
5. 只有长期有效、会影响后续技术选择的决定才记录到 .ai/decisions.md。

## 如何定制 AGENTS.md

通用规则适合作为稳定底座。项目专属规则应当：

- 描述项目事实和确实需要遵守的约束
- 说明验证方式、目录边界或不可改变的接口
- 放在更接近目标目录的 AGENTS.md，或追加到根文件
- 避免重复抄写 Skills 中已经定义的工作流
- 避免把一次性任务偏好写成长期规则

如果项目规则与通用规则冲突，应明确记录覆盖范围和原因，并尽量缩小规则的作用域。

## 如何添加自己的 Skill

在 .agents/skills/ 下创建一个目录，并添加带 YAML frontmatter 的 SKILL.md：

~~~text
.agents/skills/custom/my-skill/SKILL.md
~~~

一个合格的 Skill 应该：

- 使用小写字母、数字和连字符命名
- 在描述中说明能力和适用场景
- 只写会改变 Agent 决策或提升结果的内容
- 明确边界，避免吸引不相关任务
- 不重复 AGENTS.md 的稳定原则
- 需要时提供可验证的检查或输出格式

可以参考现有 Custom Skills 的结构，并在真正需要时添加 references/ 或 scripts/ 等支持文件。

## Cursor

将本模板复制到项目根目录后，在 Cursor 中打开该项目：

- 保留根目录的 AGENTS.md，并确认当前版本的 Agent 能读取它。
- 将 .agents/skills/ 作为项目内的可复用工作流目录，需要时在对话中明确引用对应的 SKILL.md。
- 让 Cursor 在长任务开始时读取 .ai/current-task.md 和 .ai/decisions.md。
- 如果当前版本使用其他项目规则入口，可以将通用规则作为基础内容引用或同步到该入口，但不要丢失 .ai/ 状态和 Skills。

## Codex

在 Codex 中将包含本模板的目录作为项目打开：

- AGENTS.md 作为项目级通用行为规则。
- 任务需要时读取 .agents/skills/ 中的对应 Skill；不要求每次都执行完整流程。
- 长任务开始前和上下文压缩后，优先恢复 .ai/current-task.md 与 .ai/decisions.md。
- 完成任务时只报告已经真实验证的结果，并把未完成事项写入状态文件。

## License

本模板使用 MIT License。仓库中的 Superpowers Skill 原始内容来自 [obra/superpowers](https://github.com/obra/superpowers)，并保留其 MIT License 的版权与许可要求。详见 [LICENSE](LICENSE)。
