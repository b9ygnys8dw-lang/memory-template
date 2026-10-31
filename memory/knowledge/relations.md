# 关系清单

> 触发：问"改这个会牵连什么""谁依赖谁"时读。这是关系的**唯一**来源，MCP 图按此灌入。
> 格式：`A → B：关系`，行尾标 `（作废）` 表示失效但保留。

## 关系

- INDEX.md → delivery.md：reads_from
- INDEX.md → design.md：reads_from
- INDEX.md → platform.md：reads_from
- INDEX.md → memory-ops.md：reads_from
- INDEX.md → writing.md：reads_from
- INDEX.md → relations.md：reads_from
- profile/ai.md → INDEX.md：governs
- memory-ops.md → writing.md：depends_on
- memory-ops.md → LOG.md：defines
- delivery.md → LOG.md：writes_to
- setup/mcp.json → platform.md：depends_on
- setup/prompt.md → INDEX.md：reads_from
- setup/README.md → platform.md：depends_on
- setup/skills/memory-system/SKILL.md → INDEX.md：reads_from
- .github/workflows/memory-check.yml → LOG.md：reads_from

## 六种关系

`depends_on`（依赖）、`writes_to`（写入）、`reads_from`（读取）、`caused`（导致）、`blocks`（阻塞）、`affects`（影响）

## 更新

- 新增或删除文件 → 同步改这里；这是唯一的同步点
- 关系失效 → 行尾加 `（作废）`，不删行
- MCP 建立时按本文件灌入，不另写一份
