# 记忆入口

> 每次只读本文件、`profile/ai.md`，再按下面清单读 1 个场景文件；不预先读全部场景文件。

## 启动清单

1. 读本文件 + `profile/ai.md`
2. 按路由表选 1 个场景文件，整读全文（每个 ≤3KB）
3. 不确定读哪个：用关键词 grep 全部 `触发:`，或直接 grep `LOG.md` 的标题
4. 时间只查两次：会话开始一次；写记忆文件前顺带一次（日期按 UTC）
5. 读文件前先拿规模：`wc -l` 与 `grep -c "^## "`（节数）
6. 读完核对：读到的节数 = 实际节数，行覆盖到末尾；对不上就继续读，别报"读完"
7. 读不到、文件缺失、内容冲突 → 直说，不凭印象补全

## 收尾前（交付完成或用户说结束时）

问自己一句：**本场有没有一条纠正、偏好或结论，还没写进去？** 有就补写，没有就说没有。

## 路由表

| 你在做什么 | 读哪 1 个 |
|---|---|
| 交付文件、导出、改格式 | `knowledge/delivery.md` |
| 做方案、评审设计、拆机制 | `knowledge/design.md` |
| 多端、MCP、技能、时间源 | `knowledge/platform.md` |
| 记东西、改条目、作废、找不到条目 | `knowledge/memory-ops.md` |
| 要写日记、拿不准该不该记 | `knowledge/writing.md` |
| 改这个会牵连什么、谁依赖谁 | `knowledge/relations.md` |
| 图谱为空、要建初始索引 | `knowledge/mcp-seed.md` |
| 交付物之外的个人偏好 | `profile/user.md` |
| 回溯"以前发生过什么" | `LOG.md` 按事件标题，只读命中那一节 |
| 想调整我的反应方式 | `profile/behavior.md` |
| 清点哪些条目没用上（说"清点"） | `HITS.md` + `memory-ops.md` §清点 |
| 要导出给别家模型 | `PORTABLE.md` |
| 换环境、换模型、重建后恢复 | `setup/README.md` |
| 不在表里 | 不读场景文件，直接做 |

## 检索优先级（禁止两边并读）

| 顺序 | 去哪找 | 命中后怎么用 |
|---|---|---|
| 1 | 仓库：场景文件整读；`LOG.md` 两段式——列全部标题（自带结论）→ 语义挑 1–2 条 → 只读那节 | 定稿，直接当事实用 |
| 2 | MCP：`grep -i "<关键词>" "<kg_json 路径>"`；无终端时 `search_nodes` | 草稿，必须标"未定稿" |
| 3 | 都没有 | 直说"没有记录"，不编造 |

**作废条目退出检索**：grep 时一律排除，`grep 关键词 LOG.md | grep -v 作废`。旧条目不得与现行条目争线索。

**命中即记**：某条真的影响了判断，就在 `HITS.md` 追加一行 `日期 条目名`，不写别的。

禁止：两边都读后拼在一起给结论；仓库已命中还去 MCP 补充；把草稿当定稿；冲突时自己选一个（一律以仓库为准，并说明不一致）。

## MCP

- 只做草稿：未验证判断、会话内清单、长会话进度；结论一律写仓库
- 写完同会话 `grep` 一次，能命中才算写完
- 两张库都不共享实体：图谱库实体名用仓库原名（类型 `file` `tool` `project` `person`），摘要库一律 `S:` 前缀（类型 `summary`）
- json 不存在就忽略；只有确实需要关联查询时，才按 `knowledge/mcp-seed.md` 建
- 数据文件路径见 `knowledge/platform.md`

## 不读什么

- 不读整份 `LOG.md`，不批量浏览历史
- 不为了"保险"把 knowledge/ 全读一遍
- 不要用 `read_graph`

## 文件清单

```
INDEX.md                本文件（每次必读）
PORTABLE.md             导出给别家模型的提示包
LOG.md                  情景日记，按标题检索
HITS.md                 命中记录（哪条被用过）
profile/ai.md           语气与边界（每次必读）
profile/user.md         用户画像（按需）
profile/behavior.md     我的行为记录（按需）
knowledge/delivery.md   交付
knowledge/design.md     设计与方案
knowledge/platform.md   平台、多端、时间源、MCP 路径
knowledge/memory-ops.md 写入口径与条目格式
knowledge/writing.md   日记怎么写的唯一入口
knowledge/relations.md 文件间关系的唯一来源
knowledge/mcp-seed.md   MCP 初始灌入（一次性）
setup/                  搬运包：prompt.md、mcp.json、skills 三个技能
```
