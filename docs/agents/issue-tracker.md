# Issue tracker: GitHub Issues

Repository: [15253a/EyeX_agent](https://github.com/15253a/EyeX_agent)

当前主图：[智能眼镜 Agent 架构决策地图](https://github.com/15253a/EyeX_agent/issues/1)。GitHub Issues 是架构决策与工作状态的唯一权威。

## Wayfinding operations

- 地图：一个带 `wayfinder:map` 标签的 Issue，保存目的地、约定、已决事项索引、未明确区域和范围外工作。开放票从原生子 Issue 关系查询。
- 子票：使用 GitHub 原生 sub-issues 关系连接到地图；使用 `wayfinder:research`、`wayfinder:prototype`、`wayfinder:grilling` 或 `wayfinder:task` 标签。
- 身份与状态：以 Issue URL/号码定位；使用原生 `open/closed` 状态及 assignee。讨论前先分配给实际推动该票的开发者，作为领取。
- 阻塞：使用原生 issue dependencies。新增关系时使用阻塞者的数据库 `id`，不是 Issue 号码或 `node_id`。
- 前沿：读取主图的开放子票，排除已有 assignee 或仍有开放 blocker 的票，按主图子票顺序选择。`issue_dependencies_summary.blocked_by` 表示尚未解决的阻塞数量。
- 解决：在子 Issue 发表具日期的 resolution comment，记录结论、依据和用户确认，随后关闭子 Issue，并向主图的 `Decisions so far` 追加标题链接及一句摘要。决定的完整内容保留在子 Issue。
- 新票：先创建全部 Issue，再连接父子关系和阻塞关系；新发现但尚无法准确提问的问题保留在主图的 `Not yet specified`。
- 使用已连接 GitHub 工具或 `gh` 操作。当前环境没有 `gh` 时，可使用已授权身份调用 GitHub REST API；凭据只在进程内用于认证，不写入项目或工具输出。
- REST：`GET/POST /repos/15253a/EyeX_agent/issues/{map}/sub_issues` 查询/建立父子关系；`GET/POST /repos/15253a/EyeX_agent/issues/{child}/dependencies/blocked_by` 查询/建立阻塞关系。
- 叙述使用地图和票的标题，以链接包裹标题。建图会话不解决人工决策票；后续会话一次解决一张人工决策票，研究票遵循 Wayfinder 的并行例外。

## Migration and domain references

2026-10-26 已从本地 Markdown tracker 迁移至 GitHub Issues。`.scratch/glasses-agent-architecture/` 中的地图、前沿和决策票仅为迁移前只读快照，不用于判断当前状态，不作双向同步。

读取材料依据时使用 [PPT 需求依据](../../.scratch/glasses-agent-architecture/source-notes.md)。`Event`、状态标签、健康观测和生活事件的关系仍待决定。出现用户确认的领域术语后，再按 `domain-modeling` 创建项目 `GLOSSARY.md`。
