# Agent-code

代码开发 agent —— 研究并从零构建一个类 Claude Code 的智能体编码框架。

## 📖 调研文档

[**《类 Claude Code 框架：能力、演进与从零构建》**](./类Claude-Code框架调研.html)

一份自包含、可交互的 HTML 页面（**双击用浏览器打开即可，纯本地、零外部依赖、不联网**），系统梳理了这类「智能体编码框架」的全貌与构建方法：

| 章节 | 内容 |
| --- | --- |
| **核心心智** | 一切围绕的 agent loop（prompt → model → tool → observation → 循环），含循环骨架伪代码 |
| **发展史** | 2023 → 2026 的 5 个时代与关键里程碑（AutoGPT → Devin/SWE-agent → Cline → Claude Code → MCP → Agent SDK） |
| **框架全景** | 35 个主流框架分 4 类：终端 CLI · 编辑器 / IDE 集成 · 自主 SWE 智能体 · 构建用 SDK / 框架 |
| **架构原理** | 6 大核心组件：Agent Loop · 工具系统 · 上下文与记忆 · 权限沙箱 · 规划分解 · Harness 工程（各含**深入机制：线级协议与逐工具实现** + MVP vs 成熟版对比） |
| **深入专题** | 6 个实现级(implementation-grade)深挖：MCP 协议内部 · Prompt 缓存与推理经济学 · 代码智能与检索（Aider PageRank / LSP / 嵌入 RAG） · 互操作协议（ACI / ACP / A2A） · 会话状态·检查点·Hooks·可观测性 · 评估 Harness 内部（SWE-bench 五阶段与泄露陷阱） |
| **构建路线图** | Stage 0 → 8 分阶段构建路线，每阶段含目标 / 构建步骤 / 新增组件 / 验收标准 / 常见错误 |
| **迭代与评估** | 10 条迭代优化原则 + 三轴评估法（正确性 / 成本延迟 / 安全） |

> ⚠️ 文档数据反映约 **2026 年年中** 的态势。版本号、SWE-bench 分数、估值、开源状态等具体数字变动较快，关键决策前请回到各项目官方来源二次确认。

## 📖 调研文档 · 二

[**《数字员工型 Agent：WorkBuddy · Cowork 类通用办公智能体全景与构建指南》**](./WorkBuddy类Agent调研.html)

同款自包含、可交互的 HTML 页面（双击浏览器打开即可，纯本地、零外部依赖）。对标上一份「编码 agent」，这份聚焦**平行赛道**——能在 OS/浏览器层自主执行多步办公任务、接入企业系统、以自然语言驱动的「agentic coworker / 数字员工」：

| 章节 | 内容 |
| --- | --- |
| **核心心智** | 「感知—推理—行动」闭环（截图 → VLM → 动作 → 观察），从「用电脑」到「指挥电脑」的范式跃迁 |
| **发展史** | Chatbot → Copilot → Computer-use → Agentic coworker 四代演进（2022 → 2026），及编码线与办公线的分叉与合流 |
| **产品全景** | 3 大阵营 20+ 产品：焦点双雄（WorkBuddy · Claude Cowork）· 海外玩家（OpenAI / Google / 微软 / Amazon / Sierra / Cognition / Manus）· 国内玩家（字节 / 阿里 / 飞书 / 智谱 / 月之暗面 / MiniMax / 百度 / 面壁） |
| **架构原理** | 9 大组件：computer-use/OS 操控 · 浏览器 vs 桌面路径 · 连接器与 MCP · 技能生态 · 权限沙箱/HITL · 数字员工模型（脑手解耦）· 多 agent · 云端异步 · 上下文记忆（各含 MVP vs 成熟版） |
| **构建路线图** | Stage 0 → 9 分阶段构建路线，含可复用开源选型（computer-use-demo / browser-use / OpenAdapt / MCP / Manus） |
| **评估与风险** | 8 大 benchmark 现实成绩（长程办公仅 20–30%、Web 短任务 ~94%）+ 企业落地 4 指标 + 3 类失败模式与安全风险 |

> 🔎 本文档在调研之外增设了 **对抗式事实核实**（约 87 条易变事实独立复核），已更正 SkillHub 规模与性质、WorkBuddy 定价口径、Manus 月活量级、Cowork GA 时点、benchmark 分数归属等十余处偏差；红色标记处为已知存疑项。

## 🎯 项目目标

以这份调研为路线图，从最小可运行的 agent loop（Stage 0）出发，逐步加固为一个完整的编码智能体框架 —— 先让它跑，再让它变好。
