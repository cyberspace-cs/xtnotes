# KIMI（Moonshot）使用经验

> KIMI 是月之暗面（Moonshot）的模型，长上下文著称。我在 shuati-coach 里把它接成了可选 Provider（`kimi-k2-0905-preview`），也关注它在 Agent 场景的潜力。

---

## 1. 为什么关注 KIMI

| 维度 | 体验 |
|------|------|
| 长上下文 | 超长文档 / 整本教材一次性喂入，适合 RAG 前置压缩 |
| 中文能力 | 中文表达自然，适合中文答疑场景 |
| Agent 友好 | 支持 Function Calling / 工具调用，可做虚拟工具范式 |
| 接入成本 | OpenAI 兼容 `/chat/completions`，切换零成本 |

## 2. 我在项目里怎么用

`server/agent/llm.py` 的 Provider Registry：
```python
"moonshot": {
    "label": "Kimi (Moonshot)",
    "api_base": "https://api.moonshot.cn/v1",
    "key_envs": ["MOONSHOT_API_KEY", "KIMI_API_KEY"],
    "default_model": "kimi-k2-0905-preview",
    "supports_json": True,
}
```
- `switch_provider("moonshot")` 一行切到 KIMI，无代码改动。
- 和智谱 / 混元 / 千问等并列，按「谁配了 Key 用谁」自动适配。

## 3. KIMI 做 Agent 的心得

- **长上下文 + RAG**：把整本考纲/教材放上下文，减少检索召回错误（PageIndex 式的「无向量推理式 RAG」思路可结合）。
- **结构化输出**：用 Function Calling（虚拟工具范式）约束 JSON，比 `json_object` 稳。
- **成本控制**：长上下文 token 贵，配合我的 `compress_context()`（TF-IDF 抽取）先压再喂。

## 4. 和 Hermes Agent 的呼应

Hermes Agent 官方支持 **Kimi/Moonshot** 作为 Provider 之一（`hermes model` 切换）。说明 KIMI 在「多模型路由」场景是被验证可用的——我的多厂商架构和它思路一致。

## 5. 使用建议

1. **长文档场景优先 KIMI**：教材 / 论文 / 整库代码。
2. **中文答疑优先 KIMI**：表达更贴合中文用户。
3. **务必加上下文压缩**：长上下文不等于无脑堆 token，成本要管。

> 延伸：[Hermes Agent 笔记 · 多模型支持](note/hermes-agent学习笔记.md)
