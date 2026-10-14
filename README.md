# 余胜辉 · Yu Shenghui

**大模型应用开发工程师** ｜ 5 年 AI 工程经验（4 年医疗 AI · 1 年金融投研）

Base 石家庄，可到一线城市。方向：**金融领域大模型应用 / RAG / AI Agent / 智能投研工具**。

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![RAG](https://img.shields.io/badge/RAG-FF6F00?style=flat&logo=elastic&logoColor=white)
![Milvus](https://img.shields.io/badge/Milvus-00A1C1?style=flat&logo=milvus&logoColor=white)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=flat&logo=elasticsearch&logoColor=white)
![Dify](https://img.shields.io/badge/Dify-1C3C3C?style=flat&logo=dify&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3D?style=flat&logo=langchain&logoColor=white)

---

## 在做什么

把大模型落到具体业务里。从 0 到 1 搭过投研知识库、数字人、医疗智能体，也做数据采集与治理。关注检索质量、推理延迟和工程稳定性，每次优化都用可量化指标验证。

## 代表工作

**金融 RAG 向量知识库**（在职项目）

- 规模 43.2 万 chunk / 8 collections / 1024 维（BGE Embedding + Milvus HNSW + COSINE）
- 修复 84% 假命中问题，有效召回 **+2.4×**
- RAGas 评测驱动迭代：忠实度 **0.83 → 0.91**，上下文精度 0.87 → 0.95
- 大模型自动打标降本 **1/5 ~ 1/10**，单条延迟 2s → 0.4~0.7s，准确率追平人工基线
- 三级缓存 + 影子灰度复核（日均成本 < ¥0.4）

**数字人系统**

- 查询链路 **7s → 300ms**，支撑 200+ 并发
- 全链路：语音唤醒（100% / 300ms）→ SenseVoice ASR（WER ≤ 10%）→ LLM 问答 → TTS → 视频数字人（Latentsync 口型同步）

**研究任务编排系统**（在职项目）

- 企业微信 + 状态机自动派单，11 人团队任务负载日内极差 **6.81pp → 0.20pp**（零损失）
- 智能表格作单一事实源：任务分发 → 交付回收 → 人工验收闭环

**研报情报采集系统**（在职项目）

- 覆盖 19 家券商研报源，接口逆向 + 双通道分发，连续月度零缺口

## 技术栈

| 方向 | 内容 |
|---|---|
| 语言与框架 | Python · PyTorch · FastAPI · Django 4.2 · Vue3 / React |
| RAG 全链路 | MinerU / OCR 解析 · 动态分块 · BGE / BGE-m3 Embedding · Milvus / FAISS · 稠密 + 稀疏混合检索 · Query 改写 · 语义去重 · LLM 重排 · 引用溯源 · RAGas 评测 |
| Agent 与编排 | Dify Workflow · 意图识别 + 实体提取 · MCP 协议 · 工具调用 · 企业微信 / 状态机编排 |
| 模型与训练 | Qwen / DeepSeek(-R1) / GLM · SFT / LoRA · RLHF(PPO) 认知 · 量化与推理加速 |
| 数据与存储 | Elasticsearch · Milvus · Neo4j · Redis · SQLite / PostgreSQL / MySQL · 爬虫与接口逆向 |
| 工程化 | Docker · Celery · 私有化内网离线部署 · 缓存分层 · 限流熔断 · 幂等写入 |

## 开源项目

| 项目 | 说明 |
|---|---|
| [ICU_Mediapipe](https://github.com/Trueysh/ICU_Mediapipe) | ICU 手部行为监测系统：MediaPipe + OpenCV，多摄像头并行，多级报警，★27 |
| [hybrid-search-demo](https://github.com/Trueysh/hybrid-search-demo) | ES BM25 / 纯向量 / RRF 混合检索三策略对比实验，含中文分词降级方案 |

## 经历

| 时间 | 公司 | 角色 | 方向 |
|---|---|---|---|
| 2025.07 — 至今 | 某私募资管机构 | 大模型应用开发 | 投研智能化：RAG 知识库、任务编排、情报采集 |
| 2025.03 — 2025.07 | 某医疗信息化公司 | 大模型应用开发 | 医疗 To B 智能体，内网私有化部署 |
| 2023.09 — 2025.03 | 某 AI 应用公司 | NLP 算法工程师 | 数字人 + RAG 两条产品线 |
| 2021.07 — 2023.09 | 某医疗 AI 公司 | NLP 算法工程师 | 医疗 NLP；10B 模型数据处理与自动评估 |

## 联系

- 邮箱：TrueYSH@163.com
- 博客：https://blog.csdn.net/TrueYSH

---

*公司与项目名称已脱敏；文中数字口径与简历一致。*
