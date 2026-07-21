# 路线图（Roadmap）

> 原为本地笔记的云端存储，逐步公开整理为作品集。
> 原则：**先建宏观脉络 → 学核心小项目 → 深入大项目**（来自 X-Plore 的 nano project 学习法）。

---

## 当前主线：AI Agent / LLM 推理优化

| 阶段 | 主题 | 状态 | 产出 |
|------|------|------|------|
| 第 1 周 | Agent 原理 / LLM 调用 / 提示工程 / RAG | ✅ | [第1周笔记](note/第1周学习笔记.md) |
| 第 2 周 | 上下文工程与记忆系统 | ✅ | [第2-12周笔记](note/第2-12周学习笔记.md) |
| 第 3-4 周 | 知识蒸馏（Teacher→Student） | ✅ | 同上 |
| 第 5-6 周 | 量化与剪枝 | ✅ | 同上 |
| 第 7-8 周 | 投机解码 | ✅ | 同上 |
| 第 9 周 | 企业级并行部署 | ✅ | 同上 |
| 第 10-12 周 | 面试冲刺 | ✅ | 同上 |
| 延伸 | Hermes Agent 自改进框架 | ✅ | [hermes-agent笔记](note/hermes-agent学习笔记.md) |
| 延伸 | X-Plore 知识库梳理 | ✅ | [X-Plore梳理](note/X-Plore学习资料梳理.md) |

---

## 实践项目（代码落地）

- [Audit-AIOPS](projects/audit-aiops.md)：Agent 推理优化全栈（蒸馏/剪枝/投机解码/并行）
- [Unified-Audit-AIOPS](projects/unified-audit-aiops.md)：统一审计运维 Agent 应用
- [shuati-coach](projects/shuati-coach.md)：刷题教练 Agent（StateGraph 编排）

---

## 待办 / 下一步

- [ ] 把本仓库接入 GitHub Pages / Vercel 生成个人网站
- [ ] 补充 KIMI / Claude Code 实战经验贴（见 [exp/](exp/README.md)）
- [ ] 刷 LeetCode / 牛客，沉淀题解与八股
- [ ] 简历模板定稿并中英文双版（见 [resume/](resume/README.md)）
- [ ] 深入 Hermes 的 skills / cron / 子代理模块源码
