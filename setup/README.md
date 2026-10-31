# 搬运包

> 换设备、换客户端、重建容器时按本目录恢复。这里只是配置与技能，记忆本体在上级目录的 md 文件里。

## 内容

| 路径 | 是什么 |
|---|---|
| `prompt.md` | 提示词与规则（唯一来源）：站点规则栏与任意模型都用它 |
| `mcp.json` | 两个记忆 MCP 的配置字段（stdio、npx） |
| `skills/memory-system/SKILL.md` | 记忆系统：读写、检索、作废 |
| `skills/kg-memory/SKILL.md` | 图谱草稿：实体与关系、影响链 |
| `skills/mcp-draft-summary/SKILL.md` | 摘要草稿：`S:` 前缀阶段小结 |

三个技能上传时各自打包成 zip，包内必须恰好是 `SKILL.md`。

## 日常写入（工作副本）

```sh
gh auth setup-git            # 一次性：给 git 装凭据助手，否则 clone/push 会要求输用户名
gh repo clone <你的账号>/<你的仓库> /workspace/memory-repo
# 每次写：pull --rebase → 改 → wc -c 校验 → commit → push
```

不用 Contents API 逐文件读改写。**`gh auth setup-git` 只需跑一次，换容器要重跑。**

## 恢复顺序

1. 打开站点侧「规则」，粘贴 `prompt.md` 里的代码块（本文件是唯一来源）
2. 打开「MCP」，按 `mcp.json` 填两个 server；`MEMORY_FILE_PATH` 必须与 `knowledge/platform.md` 里写的路径一致
3. 上传 `skills/` 下三个技能（可整包或单文件，文件名必须叫 `SKILL.md`）
4. **新开会话**（工具集在会话启动时快照，当前会话不会热加载）
5. 验证：读 `memory/INDEX.md` 能读到；`grep` 图谱 json 有结果；技能能被触发

## 注意

- 图谱 json 与摘要 json 都在容器临时目录，**重建即消失，属正常**，不补不猜
- 仓库里的 md 是唯一真相源；本目录只放"怎么把它跑起来"
- 改了 `SKILL.md` 就改本目录这一份，不要另存第二版
