# MCP 初始灌入

> 触发：图谱为空、要建初始索引、或容器重建后需要恢复关联查询时读。
> 只灌"文件与工具"，不灌内容；事实仍以仓库为准。

## 前提

- 需要图谱型 MCP（实体/关系），数据路径见 `platform.md`
- 幂等：`kg_json` 已存在且非空就跳过，不重复建

## 实体（11）

| 名称 | 类型 | 一句话 |
|---|---|---|
| INDEX.md | file | 每次会话唯一入口 |
| LOG.md | file | 情景日记，按标题检索 |
| PORTABLE.md | file | 导出给别家模型 |
| profile/ai.md | file | 语气与边界，每次必读 |
| profile/user.md | file | 用户画像 |
| profile/behavior.md | file | 我的行为记录 |
| knowledge/delivery.md | file | 交付规范 |
| knowledge/design.md | file | 设计与方案 |
| knowledge/platform.md | file | 平台、时间源、MCP 路径 |
| knowledge/memory-ops.md | file | 写入口径与条目格式 |
| knowledge/mcp-seed.md | file | 本清单 |

## 关系

**不在本文件重复**：按 `knowledge/relations.md` 灌入，那里是唯一来源。

## 执行

1. `search_nodes("INDEX")` 自检；已有实体就直接停
2. `create_entities` 写入 13 条，observation 用键值：`事实` `时` `状态`
3. `create_relations` 写入 8 条
4. `grep` 一次验证能命中，能命中就算完成

## 不做

- 不灌条目内容（偏好、事件留在仓库）
- 不与摘要库共享实体
