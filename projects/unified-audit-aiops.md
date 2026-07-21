# Unified-Audit-AIOPS · 统一审计运维 Agent 应用

> 路径：`../Unified-Audit-AIOPS/`
> 一句话：**把审计运维平台「对话化」的企业级 Agent 应用**——意图识别、工单拆单、审批路由、知识问答一体。

---

## 架构分层

```
用户层   AgentHub / ApiChannel       多渠道接入分发
编排层   AgentOrchestrator           意图识别 → 规划 → 路由 → 执行
技能层   SkillRegistry / Tools       能力注册与工具封装
LLM层    LLMClient / call_llm        统一调用、缓存、压缩
协议层   OpenAI 兼容 API             多厂商 /chat/completions
```

## 关键能力（对应源码）

- **意图识别**：`app/agent/orchestrator.py` 用 `classify_intent()` 把自然语言映射到服务项 ID。
- **工单拆单**：识别「申请制作Ukey」→ 自动拆单 → 路由审批责任人，生成 WO 工单。
- **知识问答**：RAG 检索后 `answer()` 生成（温度 0.3，防幻觉）。
- **统一 LLM 抽象**：`app/llm/client.py` 的 `LLMClient` 支持 mock / hunyuan / qwen 等，**无 Key 自动降级**为关键词规则。
- **组合缓存**：`app/llm/cache.py` 的 PromptCache（精确命中）+ SemanticCache（语义命中），降低重复调用。

## 工程亮点

- **Mock 模式**：无 API Key 时规则兜底，本地零成本跑通全链路——非常适合演示与测试。
- **缓存可观测**：`kv_cache_hit_rate` / `hit_rate` / `saved_ms_total` 等指标台账。
- **多服务编排**：alerts / assets / calls / catalog / events / knowledge / monitor / security / service_mgmt / sla / workorder 等 11+ 服务模块。

## 面试可讲的点

- 「我做了双层 Agent：用户层多渠接入、编排层状态机、LLM 抽象层统一调用」
- 「无 Key 时规则降级，保证 demo 永远能跑」
- 「缓存用精确 + 语义组合，命中率与省时都可观测」
