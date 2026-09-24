---
name: init
description: Use when entering a repository for the first time or when the user explicitly requests /init to establish or inspect project AI context without overwriting existing configuration.
---

# Project AI Initialization

## Overview

Initialize or inspect the AI development context of the current repository. This is a repository discovery and context-establishment skill, not a coding, refactoring, dependency, Git, or third-party Skill installation workflow.

The durable source of truth is:

- .ai/ for project state and confirmed project context;
- AGENTS.md for project-level instructions;
- .agents/skills/ for the single project Skill source.

Run this Skill only for an explicit initialization request such as /init or /init --refresh. Do not invoke it for ordinary development tasks merely because the repository lacks .ai files.

## Safety contract

Before writing anything:

1. Inspect the working tree and relevant repository files.
2. Detect existing AI configuration, development rules, documentation, and Skills.
3. Classify each target as missing, compatible, conflicting, or user-owned.
4. Decide the smallest safe change.

Existing content is preserved by default. Never use a delete-and-regenerate sequence.

不要：

- 覆盖或删除已有指令、项目文档或 .ai/ 文件；
- 修改业务代码、项目架构、依赖、构建设置、CI/CD 或系统配置；
- 安装依赖或第三方 Skill；
- 修改 Git 状态、Git 配置、分支、提交或 Worktree；
- 为不同 AI 工具创建重复的完整 Skill 目录；
- 把没有项目证据支持的事实写入上下文；
- 把初始化扩大成通用清理或重构任务。

If a required change conflicts with existing content, stop the write, explain the conflict, and ask for an explicit decision. A refresh request permits deeper analysis; it does not grant permission to overwrite user content.

## Mode selection

Use the marker, not the presence of AGENTS.md, as the initialization state:

~~~text
.ai/.initialized exists
    ├── no  → first initialization
    └── yes → inspection mode

/init --refresh
    └── explicit full re-analysis, still with conflict protection
~~~

If .ai/.initialized exists but is malformed, stale, or has an unsupported version, treat the project as already initialized, report the condition, and do not silently replace the marker.

If .ai/ exists without the marker, treat the project as partially initialized. Inspect and reuse its contents; do not assume that any missing file is safe to replace.

## First initialization

When .ai/.initialized does not exist:

### 1. Read before writing

Inspect only what is relevant, including:

- README and other project documentation;
- AGENTS.md, CONTRIBUTING guidance, and existing development rules;
- top-level and important subdirectory structure;
- project manifests and configuration files;
- documented or discoverable build, test, lint, check, run, and release commands;
- CI/CD configuration and repository automation;
- existing AI instructions, tool configuration, and project-local Skills;
- working-tree state and files that may already be user-modified.

The absence of any one file is not itself a problem. Record only facts supported by the files, commands, or other repository evidence.

### 2. Build the project context

Create or update the smallest set of missing initialization files:

~~~text
.ai/
├── .initialized
├── current-task.md
├── decisions.md
└── project.md
~~~

Create .ai/ and missing files only when they do not exist. If a file already exists, read it and preserve it. If its structure conflicts with the template, do not rewrite it; report the difference and propose an incremental merge.

### 3. Handle AGENTS.md

- If AGENTS.md is missing, create a concise project-level file from confirmed project facts and the repository's actual development commands and constraints.
- If AGENTS.md exists, read it before making any suggestion. Do not replace it, delete rules, or change its meaning.
- Add a rule only when it is clearly missing, supported by project evidence, and the user has authorized an incremental update.
- Treat a copied generic template AGENTS.md and an existing project-specific AGENTS.md as user-owned content. Preserve both rule sets and make the relationship explicit rather than choosing one to overwrite the other.

Project-specific AGENTS.md should contain facts and constraints; general workflows remain in the available Skills.

### 4. Write the standard state files

When the files are missing, use these structures. Preserve existing files even if they use a different but intentional structure.

current-task.md:

~~~markdown
# Current Task

## Goal

## Status

## Completed

## In Progress

## Pending

## Constraints

## Decisions

## Verification

## Known Problems

## Next Step
~~~

decisions.md:

~~~markdown
# Architecture Decisions

用于记录长期有效的重要技术决策。

普通代码修改不要记录。

## YYYY-MM-DD

### Decision

### Context

### Reason

### Alternatives

### Impact
~~~

project.md should contain confirmed facts under at least these headings:

~~~markdown
# Project Context

## Project Purpose

## Technology and Runtime

## Build

## Tests

## Main Directories

## Important Configuration

## Development Constraints

## CI/CD

## Key Entry Points

## Known Problems

## Evidence
~~~

Use “not confirmed” or leave a section empty when the repository does not provide evidence. Do not fill gaps with common conventions or assumptions.

### 5. Inspect Skills and AI environments

Treat .agents/skills/ as the single source of Skill content. Inspect its subdirectories and SKILL.md files, including superpowers, custom, and any additional project-local groups.

Also inspect existing tool-specific instruction files or directories when present. They are compatibility views or configuration sources, not permission to copy full Skill content into multiple locations. If duplicated Skills or conflicting instruction sources are found:

- report the sources and which one appears canonical;
- preserve existing files;
- suggest consolidation or links only when useful;
- do not create, install, remove, or synchronize them automatically.

Match Skill suggestions to evidence from the project. Suggestions may cover testing, debugging, containers, CI/CD, data access, APIs, documentation, or a detected language/framework, but the Skill itself must not hard-code any one stack or generate a large stack-specific Skill set.

### 6. Create the marker last

Only after the safe writes and validation succeed, create .ai/.initialized with exactly:

~~~text
template=ai-development-template
version=1
initialized_at=YYYY-MM-DD
~~~

Use the current calendar date for YYYY-MM-DD. Do not create the marker before the context files are valid. If initialization is interrupted or validation fails, do not claim completion and do not create a false success marker.

## Inspection mode

When .ai/.initialized exists and the user did not request --refresh, do not run full initialization and do not rewrite context files.

Compare the current repository with .ai/project.md and the marker. Check for:

- substantial directory or entrypoint changes;
- changes to confirmed technology, runtime, build, or test commands;
- CI/CD or major configuration changes;
- AGENTS.md rules that no longer match current evidence;
- project.md sections that are stale, missing, or contradicted;
- new confirmed long-term development constraints;
- new Skills, duplicate Skill sources, or conflicting AI configuration.

Output:

~~~text
项目已经初始化。

发现以下变化：

- ...

建议更新：

- ...
~~~

Inspection mode is read-only unless the user separately approves a specific incremental change. Running /init repeatedly with no repository changes should produce no new files and no content churn.

## Refresh mode

For /init --refresh:

1. Re-scan the repository and compare the findings with the existing .ai/ context.
2. Preserve existing AGENTS.md, project.md, state files, and user-authored sections.
3. Add only confirmed missing information or missing files.
4. If a proposed update would replace or reinterpret existing content, write a clearly named proposal or show the exact suggested change instead of applying it.
5. Do not alter business code, dependencies, Git, CI/CD, build configuration, or unrelated documentation.
6. Revalidate the context and report every applied or deferred change.

Refresh is a request to re-analyze, not a request to force-overwrite. If the user wants a conflicting existing file replaced, ask for explicit approval for that exact file and scope.

## Initialization report

For first initialization, report:

~~~text
项目初始化完成。

项目：
xxx

已完成：

- 创建或保留 .ai/
- 创建或保留 .ai/project.md
- 创建或保留 .ai/current-task.md
- 创建或保留 .ai/decisions.md
- 创建 .ai/.initialized
- 创建、保留或提出项目 AGENTS.md 变更

识别到的项目特征：

- ...

建议使用的 Skills：

- ...

未自动执行：

- 未安装第三方 Skill
- 未修改业务代码
- 未修改 Git
- 未修改项目依赖
~~~

If facts or ownership cannot be confirmed, add:

~~~text
需要人工确认：

- ...
~~~

For inspection or refresh, distinguish actual changes from suggestions and say when no actionable change was found.

## Quick reference

| Situation | Action |
| --- | --- |
| Marker missing, targets missing | Analyze, create missing context, validate, write marker last |
| Marker missing, existing targets present | Read and preserve; fill only missing parts or propose changes |
| Marker present, normal /init | Inspect and report; do not reinitialize |
| Marker present, /init --refresh | Re-analyze; apply only safe incremental changes |
| Conflicting existing file | Preserve it and ask for a scoped decision |
| Missing third-party capability | Suggest it; never install it automatically |
| Duplicate Skill sources | Report canonical source and suggest consolidation; do not copy content |

## Common mistakes

| Mistake | Correct behavior |
| --- | --- |
| Using AGENTS.md existence as the initialization flag | Use .ai/.initialized |
| Replacing an existing AGENTS.md with the template | Read, preserve, and propose only evidence-based additions |
| Treating an existing .ai/ directory as empty | Inspect every relevant file before creating anything |
| Updating project.md from guesses | Record only confirmed facts and evidence |
| Running full initialization on every /init | Use inspection mode after the marker exists |
| Interpreting --refresh as overwrite permission | Re-analyze while keeping the same safety boundaries |
| Installing Skills because a project appears to need them | Report suggestions only |
| Generating per-tool copies of Skills | Keep .agents/skills/ as the single content source |
| Claiming completion before the marker and files are validated | Create the marker last and report failures honestly |

## Relationship to other Skills

This Skill establishes project context and inspects the AI environment. It does not replace brainstorming, planning, TDD, debugging, verification, or Review workflows. Load those Skills separately when the actual development task requires them.

## References

The safety and conflict-handling design was informed by the current implementation of [repo-init-skill](https://github.com/happyfeetw/repo-init-skill/tree/ba9ba09699374ab4f42589daae13656ef026e6d3), especially its scan-before-write and explicit conflict modes.

The single-source Skill discovery design was informed by the current implementation of [skills-init](https://github.com/EzraApple/skills-init/tree/e1906133980590c614d59c89e9dba1d804632b14), especially its .agents/skills source and idempotent tool-view handling.

This Skill does not copy their commands, CLI, file layout, templates, or implementation. The current repository architecture and its AGENTS.md, .ai/, .agents/skills/, Superpowers, and Custom Skills remain authoritative.
