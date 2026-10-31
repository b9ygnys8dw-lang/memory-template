---
name: kg-memory
description: 用 Knowledge Graph Memory（11 个工具，含 update_entities/update_relations）记录实体与关系。当需要记录文件、工具、机制之间的依赖与影响链，或回答"改这个会牵连什么"时使用。
---

# 图谱记忆

图谱写关联，不写事实。事实进仓库。

## 工具（11 个）

| 用途 | 工具 |
|---|---|
| 查重 | `search_nodes(query)` |
| 按名取 | `open_nodes(names[])` |
| 新建 | `create_entities(entities[])` |
| 加观察 | `add_observations(observations[])` |
| 建关系 | `create_relations(relations[])` |
| 改实体/关系 | `update_entities`、`update_relations` |
| 删 | `delete_entities`、`delete_observations`、`delete_relations` |
| 全图 | `read_graph` —— **少用**，只在重建索引时一次 |

## 命名空间（硬约束）

两个记忆服务共用同一套图结构，只靠命名隔离。规则不可破：

| 库 | 实体名 | 类型白名单 | 关系白名单 |
|---|---|---|---|
| KG Memory（本库） | 原名，无前缀：`INDEX.md`、`memory-ops.md` | `file` `tool` `project` `person` | `depends_on` `writes_to` `reads_from` `caused` `blocks` `affects` |
| Memory（摘要库） | 必须 `S:` 前缀 | 只有 `summary` | 只有 `caused` `follows` |

禁止：

- 本库出现 `S:` 开头或类型为 `summary` 的实体 → 立即 `delete_entities`
- 本库实体名带日期、阶段名、句子化描述（那是摘要库的事）
- 摘要库出现 `file` / `tool` 类型实体（那是本库的事）
- 同一实体在两库各建一份

写入前自检三问：有前缀吗？类型在白名单吗？名字是仓库原名吗？任一答否就不写。

## observation 用键值，不用句子

一条 observation = `键: 值`，键只能从下表选。

| 键 | 存什么 | 例 |
|---|---|---|
| `事实` | 客观信息 | `事实: 每次写前查服务器时间` |
| `评价` | 一句主观判断 | `评价: 稳定` |
| `意愿` | 后续动作倾向 | `意愿: 继续使用` |
| `因` | 为什么这么判断（**必填**） | `因: 定时任务延迟可忽略` |
| `时` | 年-月 | `时: 2026-10` |
| `源` | 出处 | `源: platform.md` |
| `状态` | 已确认 / 推断 | `状态: 已确认` |

禁止：

- 自造键（`味道`、`口味`、`好喝度`）——同义词会让检索失效
- 一条 observation 写多个键：`评价: 好喝 | 意愿: 再来` → 拆成两条
- 省掉 `因`：没理由的结论下次无法复用

## 例

```
实体: 奶茶店A (project)
  observations: [
    "事实: 半糖乌龙，茶味偏重",
    "评价: 好喝",
    "意愿: 常来",
    "因: 茶味重、不甜腻",
    "时: 2026-10"
  ]
检索 search_nodes("评价: 好喝") 就能拉到所有同类
```

## 建模规范

- 实体类型只四类：`person`、`project`、`file`、`tool`
- 实体名 = 仓库原名（`INDEX.md`、`memory-ops.md`），不要同义词
- 关系类型固定六个：`depends_on`、`writes_to`、`reads_from`、`caused`、`blocks`、`affects`
- observation 一句一条，带日期前缀：`2026-10-25: 容器不持久导致 MCP 数据丢失`

## 写入流程

1. `search_nodes` 查是否存在 → 存在就 `add_observations`，**不新建**
2. 新建：`create_entities` 一次带全 observations，每实体 ≤5 条
3. 再 `create_relations`，同一对实体同一关系只建一次
4. 单次上限：5 实体、10 关系

## 读取流程

- 问关联 → `search_nodes("关键词")`
- 已知实体名 → `open_nodes([...])`，比 `search_nodes` 更省
- 不要用 `read_graph`：全图进上下文会淹掉重点
- 命中 >10 条只取最近 5 条

## 纠错

- 实体名写错 → `update_entities` 改名，**不要删了重建**（否则关系全断）
- 同一物两个节点 → 保留更接近仓库文件名的一个，`delete_entities` 另一个（删前确认）
- 反向重复关系（`A affects B` 与 `B depends_on A`）→ 只留更具体的

## 与摘要服务的隔离

- 图谱服务实体名**不加前缀**
- 摘要服务实体名统一加 `S:` 前缀
- 两库不共享实体，避免同一概念两边各长一个

## 边界

- 草稿层，不作事实依据；关键结论仍写仓库
- 不写隐私、凭证、整段对话
- 与仓库冲突 → 以仓库为准，不比对、不合并
