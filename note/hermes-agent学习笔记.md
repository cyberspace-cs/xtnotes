# Hermes Agent 学习笔记

> 来源：[lvy010/hermes-agent](https://github.com/lvy010/hermes-agent)（fork 自 Nous Research 的 Hermes Agent，MIT 协议）
> 定位：**The self-improving AI agent**——唯一内置「学习循环」的 Agent：从经验创建技能、使用中改进、持久化知识、搜索历史对话、跨会话构建用户模型。
> 一句话记忆锚点：**「会自己长大的 Agent = 闭环学习 + 多模型路由 + 多平台网关 + 随处运行」**

---

## 0. 为什么值得学（和我的项目对照）

| Hermes Agent 能力 | 我的项目对应 | 我学到了什么 |
|-------------------|--------------|--------------|
| 闭环学习（经验→技能） | `Audit-AIOPS` 的 SkillRegistry | 技能应当**可被自动创建**（不仅是手写注册） |
| 三层/分层记忆 | `shuati-coach` MemoryStore 五段式 | 它用 **FTS5 全文搜索 + LLM 摘要**做跨会话召回 |
| 多模型路由 | `AGENT_LLM_MAPPING` 的 7 厂商切换 | 它支持 Kimi/Moonshot、GLM、MiniMax 等，且 `hermes model` 无代码切换 |
| 多平台网关 | 我的 AgentHub/Channel | 它用**单一 gateway 进程**统一 TG/Discord/Slack/WhatsApp |
| cron 自动化 | — | 自然语言调度任务，**这是我项目缺的能力** |
| 子代理并行 | — | 生成子代理 + Python 脚本 RPC 调工具 |
| RL 研究就绪 | 我的 SFT `distill` | 它用 Atropos 做工具调用模型的 RL 训练 |

**核心启发**：我的项目目前是「规则 + LLM 编排」，Hermes 把「自改进」做成了产品级闭环——这是从 demo 到 product 的关键差距。

---

## 1. 架构总览（模块地图）

```
hermes-agent/
├── agent/            # 核心 Agent 循环、状态管理（hermes_state.py）
├── hermes_cli/       # 交互式终端 UI（TUI）
├── cli.py            # 命令入口
├── gateway/          # 消息网关：Telegram/Discord/Slack/WhatsApp/Signal
├── skills/           # 内置技能（程序性记忆）
├── optional-skills/  # 可选技能
├── tools/            # 40+ 工具 + toolsets.py
├── environments/     # 终端后端抽象：local/Docker/SSH/Daytona/Singularity/Modal
├── cron/             # 自然语言定时任务调度
├── acp_adapter/ acp_registry/ mcp_serve.py  # 协议/MCP 集成
├── honcho_integration/  # 辩证式用户建模（plastic-labs/honcho）
├── tinker-atropos/   # RL 子模块（Atropos）
├── rl_cli.py trajectory_compressor.py  # RL 训练、轨迹压缩
├── docker/ nix/      # 部署 / 可复现环境
└── docs/ README.md   # 文档
```

**技术栈**：Python 92.5% / TeX（文档）/ 包管理 `uv` / 环境 Nix + Docker / 搜索 FTS5(SQLite) / 协议 MCP + ACP + Honcho。

---

## 2. 六大核心能力拆解

### 2.1 真实终端 UI（TUI）
- 多行编辑、斜杠命令补全、对话历史、interrupt-and-redirect（打断重定向）、流式工具输出。
- 对照：我的项目是 Web/API 入口，缺**可中断、可重定向**的交互能力。

### 2.2 多平台接入（单一网关）
- 一个 `gateway` 进程同时服务 Telegram/Discord/Slack/WhatsApp/Signal/CLI。
- 语音转录（voice memo）+ 跨平台会话连续。
- 命令行对照表（`/new` `/model` `/compress` `/usage` `/skills` `/stop`）。

### 2.3 闭环学习（重点）
这是 Hermes 最大的差异化，拆成 4 层：
1. **Agent 策展记忆**：周期性 nudges 推动自我回顾。
2. **自主技能创建**：复杂任务完成后自动沉淀技能（procedural memory）。
3. **技能自改进**：使用中持续优化已有技能。
4. **跨会话召回**：FTS5 全文搜索 + LLM 摘要，搜索历史对话。
- 兼容 [agentskills.io](https://agentskills.io) 开放标准（技能可跨 Agent 共享）。

### 2.4 计划自动化（cron）
- 内置 cron 调度器，自然语言描述 → 定时执行 → 投递到任意平台。
- 例：每日报告、夜间备份、每周审计。

### 2.5 委派与并行
- 生成隔离的子代理处理并行工作流。
- 写 Python 脚本通过 RPC 调用工具，**把多步 pipeline 折叠成零上下文成本的回合**。

### 2.6 随处运行（6 种终端后端）
- local / Docker / SSH / Daytona / Singularity / Modal。
- Daytona、Modal 提供 **serverless 持久化**：空闲时环境休眠，按需唤醒，几乎零成本。
- 可跑在 $5 VPS 或 GPU 集群。

### 2.7 研究就绪（RL）
- 批量轨迹生成、Atropos RL 环境、轨迹压缩——为训练下一代工具调用模型准备数据。

---

## 3. 多模型支持（无锁定）

`hermes model` 一行切换，无需改代码：

| Provider | 说明 |
|----------|------|
| Nous Portal | 官方 |
| OpenRouter | 200+ 模型 |
| z.ai / GLM | 智谱 |
| Kimi / Moonshot | 月之暗面（我项目里也接了 `moonshot`） |
| MiniMax |  MiniMax |
| OpenAI | OpenAI / 自定义端点 |

> 映射：我的 `AGENT_LLM_MAPPING.md` 里 `switch_provider()` 运行期切换，思路完全一致——**LLM 抽象层 + Provider Registry**。

---

## 4. 与我的 Agent 项目对照：可借鉴的 5 点

1. **技能自动化创建**：我的 SkillRegistry 是手注册；可加「任务完成→LLM 抽取可复用步骤→生成技能文件」。
2. **FTS5 跨会话搜索**：我的 MemoryStore 是结构化画像，缺少全文检索；可加 SQLite FTS5 + 周期性 LLM 摘要。
3. **cron 自然语言调度**：我的编排缺定时触发；可接一个 NL→cron 解析器。
4. **子代理 RPC**：我的编排是单线程 ReAct/StateGraph；重任务可拆子代理 + 脚本 RPC。
5. **serverless 后端**：我的部署重本地；可学 Modal/Daytona 休眠计费模式降本。

---

## 5. 常见面试追问（自测）

- **Q：闭环学习怎么防止技能越改越乱？** A：技能有版本/评审，兼容 agentskills.io 标准，且创建后有使用反馈闭环；变更需经质量闸门。
- **Q：FTS5 + LLM 摘要的召回流程？** A：FTS5 做关键词召回候选 → LLM 对候选做摘要/重排 → 注入上下文，兼顾精度与相关性。
- **Q：子代理 RPC 为什么能「零上下文成本」？** A：主代理只下发脚本与工具调用指令，重活在子代理侧执行，主上下文不承载中间过程 token。
- **Q：多模型路由和 RouteLLM 思路一样吗？** A：Hermes 是「用户主动切」，RouteLLM 是「系统按成本/质量自动选」；两者可结合（默认自动路由 + 手动覆盖）。

---

## 6. 一句话自测

1. Hermes 和我的 Agent 在「自改进」上的最大区别是什么？
2. 它的 6 种终端后端里，哪两种支持 serverless 休眠计费？
3. 技能系统兼容哪个开放标准？为什么要兼容？
4. `hermes model` 切换模型为什么「无代码改动」？底层靠什么设计？

---

## 7. 延伸阅读 / 同类项目

- [nanobot](https://github.com/lvy010/nanobot)（HKUDS 的 Agent 框架，X-Plore 已克隆）
- [nano-graphrag](https://github.com/lvy010/nano-graphrag)（图 RAG，X-Plore 已克隆）
- [agentskills.io](https://agentskills.io)（技能开放标准）
- [plastic-labs/honcho](https://github.com/plastic-labs/honcho)（辩证式用户建模）
