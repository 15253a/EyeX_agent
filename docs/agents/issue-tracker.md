# Issue tracker: Local Markdown

本项目的 Wayfinder 地图与决策票使用本地 Markdown。当前架构地图位于 [智能眼镜 Agent 架构决策地图](../../.scratch/glasses-agent-architecture/map.md)。

## Wayfinding operations

- 地图：`.scratch/<effort>/map.md`，标签为 `wayfinder:map`，仅保存目的地、约定、已决事项索引、未明确区域和范围外工作。
- 子票：`.scratch/<effort>/issues/NN-<slug>.md`。`ID` 是本地身份；`Parent` 指向地图；`Type` 为 `research`、`prototype`、`grilling` 或 `task`。
- 状态：`Status: open` 表示未处理；领取时先写入 `Status: claimed` 与 `Assignee`；完成后写入 `Status: resolved`。
- 依赖：`Blocked by: 01, 02` 表示前置票。`Blocked by: none` 表示无前置。只有全部前置票为 `resolved` 时才可领取。
- 前沿：按文件编号扫描 `issues/`，选择开放、无未解决前置、未领取的票。读取地图后，仅按需读取候选票及相关已决票。
- 解决：在票的 `## Comments` 下追加具日期的 `### Resolution`，记录答案、理由、依据和用户确认；关闭票，并向地图的 `Decisions so far` 追加标题链接及一句摘要。决定的完整内容只保留在票中。
- 新票：先创建全部票，再补依赖；新发现的明确问题进入票，尚无法准确提问的部分保留在地图中。
- `frontier.md` 是便于人阅读的派生视图，状态与依赖以各票为准；票发生变化后同步该视图。
- 在叙述中使用票的标题，链接包裹标题。编号只用于文件和依赖元数据。
- 建图会话不解决人工决策票；后续会话一次解决一张票，研究票遵循 Wayfinder 的并行例外。

## Domain references

读取 PPT 依据时使用 [需求依据](../../.scratch/glasses-agent-architecture/source-notes.md)。`Event`、状态标签、健康观测和生活事件的关系仍待决定。出现用户确认的领域术语后，再按 `domain-modeling` 创建项目 `GLOSSARY.md`。
