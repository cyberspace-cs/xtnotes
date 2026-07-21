# 精选 GitHub 仓库索引（resources/）

> 精选自 [lvy010/X-Plore](https://github.com/lvy010/X-Plore) 的 238 仓库索引，只保留和 **AI Agent / LLM 推理与训练 / RAG / MCP** 最相关的部分。
> 完整 238 仓库见 X-Plore 原仓库 `repo/github_repos_cn.md`。

---

## LLM 推理与训练

> 大模型推理加速、模型训练、微调与 GPU 编程。

1. [nano-vllm](https://github.com/lvy010/nano-vllm) — vLLM 精简实现，轻量 LLM 推理部署。（已克隆到 `../X-Plore-learn-projects/`）
2. [llama.cpp](https://github.com/lvy010/llama.cpp) — 纯 C/C++ LLM 推理，量化部署。
3. [TensorRT](https://github.com/lvy010/TensorRT) — NVIDIA 高性能推理 SDK（算子融合、量化）。
4. [triton](https://github.com/lvy010/triton) — OpenAI GPU kernel 编程语言与编译器。
5. [cuda-training](https://github.com/lvy010/cuda-training) — NVIDIA CUDA 官方训练教程。
6. [LLMs-from-scratch](https://github.com/lvy010/LLMs-from-scratch) — 从零用 PyTorch 实现 LLM。
7. [nanochat](https://github.com/lvy010/nanochat) — $100 复现高质量对话模型。
8. [LlamaFactory](https://github.com/lvy010/LlamaFactory) — 统一高效微调 100+ LLM/VLM。
9. [verl](https://github.com/lvy010/verl) — 火山引擎 LLM 强化学习框架（RLHF）。
10. [Qwen3.5](https://github.com/lvy010/Qwen3.5) — 阿里通义千问大模型。
11. [candle](https://github.com/lvy010/candle) — Hugging Face Rust 极简 ML 框架。
12. [ort](https://github.com/lvy010/ort) — Rust 下 ONNX 推理与训练。
13. [modular](https://github.com/lvy010/modular) — MAX & Mojo，兼具 Python 易用与 C 性能。
14. [mlflow](https://github.com/lvy010/mlflow) — AI 工程平台（调试/评估/监控）。
15. [stable-diffusion.cpp](https://github.com/lvy010/stable-diffusion.cpp) — 纯 C/C++ 扩散模型推理。
16. [autoresearch](https://github.com/lvy010/autoresearch) — 单 GPU 自动化 AI 研究（nanochat + Agent）。
17. [JiT](https://github.com/lvy010/JiT) — JiT 论文的 PyTorch 实现。
18. [MCITlib](https://github.com/lvy010/MCITlib) — 多模态持续指令微调库（抗遗忘）。
19. [ai-bindgen](https://github.com/lvy010/ai-bindgen) — Rust 过程宏编译期生成 LLM 代码。
20. [MegEngine](https://github.com/lvy010/MegEngine) — 旷视工业级深度学习框架。

## RAG / 知识检索 / 记忆

> 检索增强生成、向量数据库、Agent 记忆系统。

1. [nano-graphrag](https://github.com/lvy010/nano-graphrag) — 简洁易改的 GraphRAG 实现。（已克隆）
2. [mem0](https://github.com/lvy010/mem0) — AI Agent 通用记忆层（跨会话持久化）。
3. [hindsight](https://github.com/lvy010/hindsight) — 可学习的 Agent 记忆（越用越好）。
4. [supermemory](https://github.com/lvy010/supermemory) — 极速可扩展记忆引擎。
5. [memvid](https://github.com/lvy010/memvid) — 单文件 Serverless Agent 记忆层（替代 RAG 管道）。
6. [zvec](https://github.com/lvy010/zvec) — 阿里轻量级进程内向量数据库。
7. [PageIndex](https://github.com/lvy010/PageIndex) — 无向量推理式 RAG 文档索引（减幻觉）。
8. [code-graph-rag](https://github.com/lvy010/code-graph-rag) — 代码仓库知识图谱 RAG。
9. [OpenViking](https://github.com/lvy010/OpenViking) — AI Agent 上下文数据库（文件系统范式）。
10. [notebooklm-py](https://github.com/lvy010/notebooklm-py) — Google NotebookLM 非官方 Python API。

## MCP / Agent 工具链

> Model Context Protocol、Agent 网页交互、数据抓取与路由。

- [MCP 官方规范](https://modelcontextprotocol.io) — Agent 与外部工具的标准协议。
- 我的实践：[shuati-coach 的 MCP 桥接](projects/shuati-coach.md)（声明式注册内置/远程工具）。

## Agent 框架（精选）

> 自改进 / 轻量 Agent 框架，重点看「小代码讲清大原理」。

- [hermes-agent](https://github.com/lvy010/hermes-agent) — 自改进 Agent（技能闭环/FTS5 记忆/多平台网关）。[我的笔记](note/hermes-agent学习笔记.md)
- [nanobot](https://github.com/lvy010/nanobot) — HKUDS 的 Agent 框架。（已克隆到 `../X-Plore-learn-projects/`）
- [nano-graphrag](https://github.com/lvy010/nano-graphrag) — 图 RAG（见上）。
- [RouteLLM](https://github.com/lvy010/RouteLLM) — 智能路由在不同 LLM 间选择，降本保质量。

---

> 使用建议：挑 1-2 个 nano 项目精读源码，再回头看自己的项目，对照收获最大。
