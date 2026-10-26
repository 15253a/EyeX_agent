# EyeX Agent

智能眼镜 Agent 的需求依据与架构决策仓库。

当前工作聚焦于厘清核心场景、模块职责、跨端分工、数据与控制流程、记忆、主动触发及异常恢复，形成足以进入实现计划的架构。

## 阅读入口

- [智能眼镜 Agent 架构决策地图](https://github.com/15253a/EyeX_agent/issues/1)：目的地、初始约束、已决事项索引及待明确区域。
- 当前可开始的问题：从主图的原生子 Issue 中，选择开放、未分配且无开放前置依赖的票；父子关系与阻塞依赖直接显示在 GitHub Issues 中。
- [PPT 需求依据](.scratch/glasses-agent-architecture/source-notes.md)：逐页依据与尚缺信息。
- [原始需求 PPT](20261019.pptx)。
- [GitHub tracker 约定](docs/agents/issue-tracker.md)：领取、讨论、解决和维护决策票的方式。

## 协作方式

本项目使用 Wayfinder 和 GitHub Issues 管理架构决策。先阅读地图，再选择开放、无未解决前置且未领取的票。架构决定记录在对应 Issue 的解决评论中，地图保留摘要和链接。

架构决策、领取状态、依赖和解决记录以 GitHub Issues 为唯一权威。`.scratch/glasses-agent-architecture/` 中的地图与决策票保留为迁移前只读快照，后续不回写同步；PPT 及需求依据作为来源资料保留。
