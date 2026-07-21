# shuati-coach · 刷题教练 Agent

> 路径：`../shuati-coach/`
> 一句话：**用 Agent 帮人科学刷题的教练**——诊断薄弱点、整理错题本、生成学习计划、RAG 答疑。

---

## 编排：StateGraph（LangGraph 同构）

```
classify → diagnose / wrongbook / plan / rag_qa → reflect
```

| 节点 | 是否调 LLM | 说明 |
|------|-----------|------|
| `classify` | ✅ | 意图分类（Supervisor 模式） |
| `diagnose` | ❌ | SQL 聚合计算薄弱度（0 token） |
| `wrongbook` | ❌ | SQL 查询高频错题 |
| `plan` | ✅ | 生成冲刺计划 JSON |
| `rag_qa` | ✅ | 基于检索知识回答，标注引用 [n] |
| `reflect` | ❌ | 规则生成跟进建议 |

## 关键技术（对应源码）

- **多厂商 LLM 客户端** `server/agent/llm.py`：注册表支持 智谱 / Kimi(Moonshot) / 混元 / 豆包 / 千问 / DeepSeek / OpenAI，`switch_provider()` 运行期切换。
- **五段式上下文预算** `memory.py`：身份 / 长期画像 / 诊断摘要 / 对话历史 / 当前，复用稳定前缀。
- **上下文压缩** `inference.py`：`compress_context()` 用 TF-IDF 抽取高权重句子，省 token。
- **RAG 防幻觉** `tools.py`：低相关直接拒答，不编造。
- **MCP 桥接** `mcp.py`：声明式注册内置/远程工具。
- **虚拟工具范式** `llm.py`：用 Function Calling 的 `tool_choice` 约束结构化输出（幽灵工具技巧），比 `json_object` 更稳。
- **7 项推理优化** `inference.py`：KV 前缀缓存 / 上下文压缩 / 投机解码 / 知识蒸馏 / 连续批处理 / 工具替代生成 / 量化。

## 工程亮点

- **规则兜底优先**：diagnose / wrongbook 用 SQL 聚合，0 token、确定性强、秒回。
- **结构化输出用虚拟工具**：所有 OpenAI 兼容端点统一支持，参数有 JSON Schema 硬约束。
- **成本可观测**：LEDGER 统计 prompt_tokens_total、batch_merged_requests 等。

## 面试可讲的点

- 「我用 StateGraph 把刷题流程拆成 6 个节点，规则能搞定的不让 LLM 出手」
- 「多厂商切换靠 Provider Registry + 策略模式，一行代码换模型」
- 「结构化输出用 Function Calling 而非 json_object，稳定性更好」
