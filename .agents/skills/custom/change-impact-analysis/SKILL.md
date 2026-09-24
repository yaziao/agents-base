---
name: change-impact-analysis
description: Map the blast radius of a significant code change before implementation, identify required synchronized changes, and keep unrelated refactoring out of scope.
---

# Change Impact Analysis

Use this skill before a change that affects a shared interface, data shape, control flow, configuration, concurrency, external behavior, or multiple modules. Do not use it to justify analysis overhead for a trivial local edit.

## Analysis chain

Trace the proposed change through:

~~~text
修改点
  ↓
调用方
  ↓
依赖模块
  ↓
数据结构和状态
  ↓
配置
  ↓
并发和生命周期
  ↓
外部接口
  ↓
测试和验证
~~~

Use repository evidence: definitions, references, data flow, configuration loading, error paths, tests, and recent changes where relevant. Mark assumptions as assumptions and avoid treating names or conventions as proof.

## Questions to answer

1. What behavior or contract is changing?
2. Which callers, consumers, stored data, configuration, or external boundaries depend on it?
3. Which error, retry, concurrency, lifecycle, and resource-release paths are affected?
4. Which tests or verification paths cover the changed behavior?
5. What must change together for the repository to remain coherent?
6. What is explicitly outside the scope of this task?

## Required output

Before implementation, summarize:

- Impact: the files, modules, interfaces, data, or runtime paths affected.
- Risks: likely regressions, compatibility concerns, security concerns, or uncertain assumptions.
- Required synchronized changes: changes that must be made together.
- Verification: checks that can demonstrate the change is safe.
- Out of scope: related areas that do not need modification.

For a small but non-trivial change, this can be a short note. For a broad change, keep the analysis in the plan or task state so it remains available during implementation.

## Scope guard

Do not turn impact analysis into permission for a broad rewrite. If an improvement is not required by the current behavior, safety, compatibility, or verification needs, keep it out of scope and mention it as a follow-up candidate instead.

If the actual implementation reveals a larger impact than expected, stop and update the analysis before continuing.
