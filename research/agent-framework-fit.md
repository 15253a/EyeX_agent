# 国内 Agent 框架与 DeepSeek Harness 的适配事实

研究日期：2026-10-26。对应决策票：[国内 Agent 框架与 DeepSeek Harness 的适配事实](https://github.com/15253a/EyeX_agent/issues/12)。调查官方主分支、版本文档、发布页和许可，未安装框架或进行端侧实测；主分支能力不等于已发布版本承诺。框架提供的机制还需要结合业务流程及设备约束验证。

## 事实与候选比较

DeepSeek Harness 是 DeepSeek 官方开源的运行底座，包含可用产品及开发扩展机制，值得列入候选。应分别决定“使用什么模型”“什么运行时负责 Agent 循环”“循环部署在哪里”。使用 DeepSeek 模型并不要求选 Harness；AgentScope、Eino 也有模型接入。[官方介绍](https://deepseek.com/en/harness/)、[AgentScope 模型清单](https://github.com/agentscope-ai/agentscope/blob/v2.0.10/README.md)、[Eino DeepSeek 适配器](https://github.com/cloudwego/eino-ext/tree/main/components/model/deepseek)。

| 候选 | 语言与宿主/集成方式 | 事件、状态与扩展事实 | 版本与稳定性 |
|---|---|---|---|
| DeepSeek Harness | TypeScript/Node；源码项目要求 Node ≥24.9。桌面/Web/无界面/SDK profile；Python SDK 驱动附带二进制的 JSON-RPC 子进程，运行 SDK 无须系统 Node。[宿主配置](https://github.com/deepseek-ai/deepseek-harness/blob/master/package.json)、[Python SDK](https://github.com/deepseek-ai/deepseek-harness/blob/master/python/sdk/README.md) | Cordis 插件可替换模型、工具、循环与日志；持久 SessionEvent 与即时 agent/* 分层；支持 followup/steer/inject/cancel、从持久会话 resume。[架构](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)、[Agent API](https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/core/agent/README.md) | 10-23 发布 v0.2.1-rc.3，预发布；README 明示 developer preview 和破坏性变更。[发布](https://github.com/deepseek-ai/deepseek-harness/releases)、[声明](https://github.com/deepseek-ai/deepseek-harness/blob/master/README.md) |
| AgentScope | 阿里通义团队；Python ≥3.11 SDK，FastAPI 服务及 Web 前端。[项目配置](https://github.com/agentscope-ai/agentscope/blob/main/pyproject.toml)、[2.0.10 README](https://github.com/agentscope-ai/agentscope/blob/v2.0.10/README.md) | 事件流、中间件、Python/MCP/skills 工具、多模型、记忆接入；服务持久化与重连事件回放、外部取消、定时及后台任务；WebRTC 语音仍标 Experimental，分布式标 WIP。[服务架构](https://docs.agentscope.io/en/versions/2.0.10/deploy/agent-service) | 正式 v2.0.10，10-12 发布；stable 指向该版，latest 已指向 2.0.11dev；2.0 与 1.0 API 不兼容。[发布](https://github.com/agentscope-ai/agentscope/releases)、[stable](https://docs.agentscope.io/stable/en/index)、[版本索引](https://docs.agentscope.io/llms.txt) |
| Eino | 字节系 CloudWeGo；Go 框架，包含组件、Graph/Workflow 与 ADK。[README](https://github.com/cloudwego/eino/blob/main/README.md) | AgentEvent、Runner、回调和中间件；工具/模型接口；HITL interrupt/resume/checkpoint；TurnLoop 有持续输入、抢占、安全点取消及恢复，v0.9.22 源码可查。[ADK](https://www.cloudwego.io/docs/eino/core_modules/eino_adk/)、[取消/TurnLoop](https://www.cloudwego.io/docs/eino/core_modules/eino_adk/agent_cancel_and_turnloop_quickstart/)、[版本源码](https://github.com/cloudwego/eino/blob/v0.9.22/adk/turn_loop.go) | 最新正式 v0.9.22，10-19 发布；另有 0.10 alpha；应锁版本验证兼容性。[发布](https://github.com/cloudwego/eino/releases) |

主仓库许可：Harness 为 [MIT](https://github.com/deepseek-ai/deepseek-harness/blob/master/LICENSE)，AgentScope 与 Eino 为 [Apache-2.0](https://github.com/agentscope-ai/agentscope/blob/main/LICENSE)、[Apache-2.0](https://github.com/cloudwego/eino/blob/main/LICENSE-APACHE)。模型服务、插件和依赖的许可另计。

## 对眼镜项目的推断

**手机访问 Web 服务，与 Agent 循环内嵌 Android/iOS App，是两种部署。** 本次官方材料未提供足以确认三者可直接内嵌手机并稳定后台运行的证据；Go 可编译、Python 有 SDK、Harness 有 Web UI，均不能替代该验证。眼镜音视频、蓝牙、手机后台生命周期及断网策略，仍需单独设计。

Harness 值得验证事件插件边界能否接入传感事件；其默认 SDK 带编码人格和桌面工具，sdk-minimal 仍是 shell/image 型编码运行时，需明确替换组合及维护成本。[SDK 组合](https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/bundle/sdk-app/README.md)、[minimal 组合](https://github.com/deepseek-ai/deepseek-harness/blob/master/packages/bundle/sdk-minimal/README.md)。

若允许服务端运行，Python 团队可优先考察 AgentScope，Go 团队可优先考察 Eino；若坚持手机内嵌，应先设宿主可行性门槛，再比较框架。以上是验证顺序建议，未作选型决定。

## 待实测与后续决策

在[Agent 运行时与开发框架选型](https://github.com/15253a/EyeX_agent/issues/13)中统一验收：同一条“眼镜输入→状态更新→记忆检索→工具动作→结果确认”流程，测首响应和打断延迟、后台内存/耗电、连续传感事件背压、进程被杀后的恢复及动作去重、断网降级。阈值由场景与时效决策确定。会话恢复不代表外部动作恰好执行一次；Harness SDK 也明确恢复会话不恢复 shell/browser 等外部进程。[恢复边界](https://github.com/deepseek-ai/deepseek-harness/blob/master/python/sdk/README.md)。
