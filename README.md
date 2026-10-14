<a href="https://github.com/Trueysh">
  <img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,50:1e3a8a,100:3b82f6&height=200&section=header&text=Yu%20Shenghui%20%E4%BD%99%E8%83%9C%E8%BE%89&fontSize=36&fontColor=ffffff&fontAlignY=36&desc=%E5%A4%A7%E6%A8%A1%E5%9E%8B%E5%BA%94%E7%94%A8%E5%BC%80%E5%8F%91%E5%B7%A5%E7%A8%8B%E5%B8%88&descAlignY=58&descSize=18" alt="Yu Shenghui" />
</a>

<div align="center">

![RAG](https://img.shields.io/badge/RAG-知识工程-2563eb?style=flat-square)
![Agent](https://img.shields.io/badge/LLM_Agent-应用编排-1d4ed8?style=flat-square)
![Eval](https://img.shields.io/badge/评估驱动-RAGas/A:B-1e40af?style=flat-square)
![MedicalAI](https://img.shields.io/badge/医疗AI-4年-334155?style=flat-square)
![Python](https://img.shields.io/badge/Python-主力语言-3776ab?style=flat-square&logo=python&logoColor=white)

</div>

---

## ▍About

5 年 AI 工程经验（4 年医疗 AI · 1 年金融投研），专注把大模型落到具体业务：从 0 到 1 搭建过 **43.2 万 chunk** 规模的金融 RAG 知识库、支撑 **200+ 并发**的数字人问答系统、11 人团队的研究任务编排平台。

我关注的不是"能不能跑起来"，而是**检索质量、推理延迟、成本与工程稳定性**——每次优化都用可量化的指标收口。

- 📍 Base 石家庄 · 可到一线城市
- 🔭 在职：某私募资管机构 · 投研智能化（RAG 知识库 / 任务编排 / 情报采集）
- 🎯 求职方向：大模型应用开发 · RAG / Agent · 智能投研工具

---

## ▍代表性工作

### 01 · 金融 RAG 向量知识库

**规模**：43.2 万 chunk · 8 collections · 1024 维（BGE Embedding + Milvus HNSW + COSINE）

| 指标 | 优化前 | 优化后 |
|---|---|---|
| 有效召回 | — | **+2.4×** |
| RAGas 忠实度 | 0.83 | **0.91** |
| RAGas 上下文精度 | 0.87 | **0.95** |
| 自动打标成本 | 人工基线 | **1/5 ~ 1/10** |
| 打标单条延迟 | 2s | 0.4 ~ 0.7s |

通过检索消融实验定位 **84% 假命中**的根因并修复；三级缓存 + 影子灰度复核，日均推理成本控制在 **¥0.4** 以内。

### 02 · 数字人问答系统

全链路：语音唤醒（准确率 100% / 唤醒延迟 300ms）→ SenseVoice ASR（WER ≤ 10%）→ LLM 问答 → TTS → 视频数字人（Latentsync 口型同步）。

查询链路 **7s → 300ms**，稳定支撑 **200+ 并发**。

### 03 · 研究任务编排系统

企业微信 + 状态机自动派单，覆盖 11 人团队的任务分发 → 交付回收 → 人工验收全闭环。智能表格作为单一事实源，任务负载日内极差 **6.81pp → 0.20pp**（零损失）。

### 04 · 研报情报采集系统

覆盖 19 家券商研报源，接口逆向 + 双通道分发，连续月度零缺口。

---

## ▍方法论：评估驱动

我不依赖"感觉变好了"判断优化效果，每轮迭代先定义度量，再动手：

| 验证手段 | 规模 | 结论 |
|---|---|---|
| RAGas 自动评测（忠实度 / 上下文精度） | 迭代评测集 | 0.83→0.91 / 0.87→0.95 |
| 检索消融实验（单变量对照） | 逐环节下钻 | 定位 84% 假命中根因，有效召回 +2.4× |
| A/B 实验 | **1,213 条 × 4 臂** | 自动打标追平人工基线，成本降至 1/5~1/10 |
| 人工裁定抽检 | 504 条 | 校准自动评估偏差，建立可信基线 |

---

## ▍技术栈

<div align="center">

**语言与框架**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Django](https://img.shields.io/badge/Django-092E20?style=flat-square&logo=django&logoColor=white)

**RAG 与检索**

![Milvus](https://img.shields.io/badge/Milvus-00A1C1?style=flat-square&logo=milvus&logoColor=white)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=flat-square&logo=elasticsearch&logoColor=white)
![BGE](https://img.shields.io/badge/BGE_Embedding-1e3a8a?style=flat-square)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3D?style=flat-square&logo=langchain&logoColor=white)
![RAGas](https://img.shields.io/badge/RAGas-2563eb?style=flat-square)

**模型与工程**

![Qwen](https://img.shields.io/badge/Qwen-615CED?style=flat-square)
![DeepSeek](https://img.shields.io/badge/DeepSeek-4D6BFE?style=flat-square)
![Dify](https://img.shields.io/badge/Dify-1C3C3C?style=flat-square&logo=dify&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)

</div>

完整链路：MinerU / OCR 解析 · 动态分块 · 稠密+稀疏混合检索 · Query 改写 · 语义去重 · LLM 重排 · 引用溯源 · SFT / LoRA · 量化与推理加速 · 内网私有化离线部署 · 缓存分层 · 限流熔断 · 幂等写入

---

## ▍开源

| 项目 | 说明 |
|---|---|
| [ICU_Mediapipe](https://github.com/Trueysh/ICU_Mediapipe) | ICU 手部行为监测系统：MediaPipe + OpenCV，多摄像头并行 · 多级报警 · **★27** |
| [hybrid-search-demo](https://github.com/Trueysh/hybrid-search-demo) | ES BM25 / 纯向量 / RRF 混合检索三策略对比实验，含中文分词降级方案 |

---

## ▍经历

| 时间 | 公司 | 角色 | 方向 |
|---|---|---|---|
| 2025.07 — 至今 | 某私募资管机构 | 大模型应用开发 | 投研智能化：RAG 知识库 · 任务编排 · 情报采集 |
| 2025.03 — 2025.07 | 某医疗信息化公司 | 大模型应用开发 | 医疗 To B 智能体 · 内网私有化部署 |
| 2023.09 — 2025.03 | 某 AI 应用公司 | NLP 算法工程师 | 数字人 + RAG 两条产品线 |
| 2021.07 — 2023.09 | 某医疗 AI 公司 | NLP 算法工程师 | 医疗 NLP · 10B 模型数据处理与自动评估 |

---

## ▍联系

<div align="center">

[![Email](https://img.shields.io/badge/Email-TrueYSH%40163.com-2563eb?style=flat-square&logo=protonmail&logoColor=white)](mailto:TrueYSH@163.com)
[![CSDN](https://img.shields.io/badge/Blog-CSDN%20%40TrueYSH-c20c0c?style=flat-square)](https://blog.csdn.net/TrueYSH)
[![GitHub](https://img.shields.io/badge/GitHub-Trueysh-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/Trueysh)

</div>

<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:3b82f6,50:1e3a8a,100:0f172a&height=120&section=footer" alt="footer" />

---
