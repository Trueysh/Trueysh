<a href="https://github.com/Trueysh">
  <img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,50:1e3a8a,100:3b82f6&height=180&section=header&text=Yu%20Shenghui%20%E4%BD%99%E8%83%9C%E8%BE%89&fontSize=36&fontColor=ffffff&fontAlignY=36&desc=%E5%A4%A7%E6%A8%A1%E5%9E%8B%E5%BA%94%E7%94%A8%E5%BC%80%E5%8F%91%E5%B7%A5%E7%A8%8B%E5%B8%88&descAlignY=58&descSize=17" alt="Yu Shenghui" />
</a>

<div align="center">

[中文](#chinese) · [English](#english)

</div>

---

<a id="chinese"></a>

## 关于

余胜辉，大模型应用开发工程师。5 年 AI 工程经验：前 4 年医疗 AI，目前在一家私募资管机构做投研智能化。主要做 RAG 和 AI Agent，几个系统都是从 0 到 1 搭的：43.2 万 chunk 的金融研报知识库、医院内网投产的智能体、数字人问答产品线。

Base 石家庄，可到一线城市。

## 项目

### 金融 RAG 知识库（在职）

43.2 万 chunk，8 个 collection，Milvus HNSW 索引。选 HNSW 不选 IVF，是因为每天都有增量入库，IVF 要聚类重训，语料一变索引就失效。采集、解析（MinerU）、分块、入库、混合检索、rerank、接口，整条链路是我一个人写的。

上线后印象最深的 bug：过滤条件里的空列表在 Python 里判假，组合过滤的 `or` 被短路放行，查「中际旭创」时 84% 的命中是空值放进来的假结果。这种 bug 不报错，时延正常，监控也不响，是人工标了 504 条评测集才暴露的。把 `or` 改回 `and` 恢复交集语义后，有效召回提升 2.4 倍。

另一个问题：rerank 模型在中文金融文本上评分异常，正确文档被压到召回外。换了几何融合 `final = rerank^α × vec^(1-α)`，乘性加权，单侧分数异常时不会被另一侧的虚高救回来——加性融合（RRF、线性加权）做不到这一点。

降本的决策过程：先标 504 条做基准，生产模型 72.2%，轻量模型 71.2%，差距 1 个点，才决定往下推；然后跑了 1,213 条 × 4 臂的 A/B，加结构化 hint 后轻量模型从 68.7% 提到 71.7%，成本降到原来的 1/5~1/10，单条耗时从 2s 降到 0.4-0.7s。RAGas 忠实度 0.83 → 0.91，上下文精度 0.87 → 0.95。

可靠性上挂了影子灰度：新逻辑放在入库流水线写库之后，只落 jsonl 不写 Milvus，异常内部消化。实测单条 0.69 秒，全链路多 1-2 分钟，一天成本不到 4 毛钱，所以可以常驻。

### 医院智慧就医智能体（内网投产）

医院内网，算力一次性采购不能扩容，数据不能出网。Dify 编排工作流，XInference 自托管 BGE-m3 和 BGE-reranker，纯内网闭环，真实投产。

Dify 的问题分类器只能给意图、给不出实体，而导诊和查询要路由到具体科室和药品，所以自己实现了意图识别 + 实体提取双路。数字人对比了图片、视频、3D 三种形态，最后选 Live2D 固定动作加音频口型驱动——视频方案观感更好，但医院那套算力跑不动。

### 数字人问答系统（产品线）

端到端链路 7s → 300ms（检索段）。主要手段：文档解析从 PDF Loader + OCR（表格准确率约 70%）换成 MinerU 转 Markdown 加多模态回填（95% 以上），加上分层缓存。SenseVoice ASR，WER 10% 以内，链路支撑 200+ 并发。

### 研究任务编排（在职）

企业微信 + 状态机自动派单，智能表格做单一事实源。11 人团队的任务负载日内极差从 6.81pp 降到 0.20pp。

## 技术栈

<div align="center">

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![Milvus](https://img.shields.io/badge/Milvus-00A1C1?style=flat-square&logo=milvus&logoColor=white)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=flat-square&logo=elasticsearch&logoColor=white)
![Dify](https://img.shields.io/badge/Dify-1C3C3C?style=flat-square&logo=dify&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)

</div>

RAG：MinerU · BGE / BGE-m3 · 混合检索 · 几何融合 / RRF · LLM 重排 · RAGas
模型：Qwen / DeepSeek · SFT / LoRA · 量化推理加速
工程：Django · Neo4j · 内网离线部署 · 缓存分层 · 幂等写入

## 开源

| 项目 | 说明 |
|---|---|
| [ICU_Mediapipe](https://github.com/Trueysh/ICU_Mediapipe) ★27 | ICU 手部行为监测：MediaPipe 21 关键点，停留时长分级报警，ROI 滤误报 |
| [hybrid-search-demo](https://github.com/Trueysh/hybrid-search-demo) | BM25 / 向量 / RRF 三种检索策略对比实验 |

## 经历

| 时间 | 公司 | 方向 |
|---|---|---|
| 2025.07 — | 某私募资管机构 | 投研智能化：RAG · 任务编排 · 知识图谱 |
| 2025.03 — 2025.07 | 某医疗信息化公司 | 就医智能体，内网投产 |
| 2023.09 — 2025.03 | 某 AI 应用公司 | 数字人 + RAG 产品线 |
| 2021.07 — 2023.09 | 某医疗 AI 公司 | 医疗 NLP · 10B 模型数据与评估 |

## 联系

邮箱 [TrueYSH@163.com](mailto:TrueYSH@163.com) · 博客 [blog.csdn.net/TrueYSH](https://blog.csdn.net/TrueYSH)

---

<a id="english"></a>

## About

Yu Shenghui — LLM application engineer, 5 years in AI: 4 years medical AI, currently building investment-research intelligence at a private asset manager. I build RAG systems and AI agents from scratch — a 432K-chunk financial research knowledge base, a care-navigation agent deployed on a hospital's air-gapped intranet, and a digital-human Q&A product line.

Based in Shijiazhuang, open to tier-1 cities.

## Projects

### Financial RAG Knowledge Base (current)

432K chunks across 8 collections on Milvus with HNSW — chosen over IVF because we ingest daily and IVF needs cluster retraining whenever the corpus drifts. I wrote the whole pipeline myself: ingestion, parsing (MinerU), chunking, hybrid retrieval, rerank, API.

The bug I remember most: empty lists evaluate falsy in Python, so a filter condition got short-circuited through an `or` — 84% of hits for one company query were false positives that had passed an empty filter. No errors, normal latency, silent monitors. It only surfaced after manually labeling a 504-item evaluation set. Restoring the `and` (intersection semantics) raised effective recall by 2.4×.

Another one: the reranker mis-scored Chinese financial text and pushed correct documents below the cut. I switched to geometric fusion, `final = rerank^α × vec^(1-α)` — multiplicative weighting, so an anomalous score on one side cannot be rescued by an inflated score on the other. Additive fusion (RRF, linear) can't do that.

On cost: I labeled 504 items first — production model 72.2% vs lightweight 71.2%, a 1-point gap — before committing to the swap. Then a 1,213-item × 4-arm A/B: structured hints took the lightweight model from 68.7% to 71.7%, at 1/5–1/10 the cost, 0.4–0.7s per item (down from 2s). RAGas faithfulness 0.83 → 0.91, context precision 0.87 → 0.95.

For reliability, a shadow pipeline runs after the write step: logs to jsonl only, never touches Milvus, swallows its own exceptions. 0.69s per item, adds 1–2 minutes to the run, under ¥0.4 a day — cheap enough to leave on permanently.

### In-Hospital Care Navigation Agent (deployed)

Hospital intranet: GPUs bought once, no scaling, no data leaving the network. Dify workflow orchestration, XInference self-hosting BGE-m3 and BGE-reranker. Deployed and running.

Dify's question classifier yields an intent but no entities — and navigation must route to a specific department or drug — so I built a dual-track intent-recognition + entity-extraction path. For the digital human I compared images, video and 3D, and picked Live2D with fixed animations plus audio-driven lip sync: video looks better, but the hospital's fixed compute couldn't run it.

### Digital-Human Q&A (product line)

End-to-end latency 7s → 300ms (retrieval segment). Main lever: document parsing moved from PDF Loader + OCR (~70% table accuracy) to MinerU Markdown with multimodal backfill (>95%), plus layered caching. SenseVoice ASR at ≤10% WER; the pipeline handles 200+ concurrent sessions.

### Research Task Orchestration (current)

WeCom + state-machine task dispatch, smart-sheet as the single source of truth. Daily load spread across an 11-person team dropped from 6.81pp to 0.20pp.

## Stack

<div align="center">

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![Milvus](https://img.shields.io/badge/Milvus-00A1C1?style=flat-square&logo=milvus&logoColor=white)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=flat-square&logo=elasticsearch&logoColor=white)
![Dify](https://img.shields.io/badge/Dify-1C3C3C?style=flat-square&logo=dify&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)

</div>

RAG: MinerU · BGE / BGE-m3 · hybrid retrieval · geometric fusion / RRF · LLM rerank · RAGas
Models: Qwen / DeepSeek · SFT / LoRA · quantization & inference acceleration
Engineering: Django · Neo4j · air-gapped deployment · layered caching · idempotent writes

## Open Source

| Project | Notes |
|---|---|
| [ICU_Mediapipe](https://github.com/Trueysh/ICU_Mediapipe) ★27 | ICU hand-behavior monitoring: MediaPipe 21 landmarks, dwell-time tiered alarms, ROI false-positive filtering |
| [hybrid-search-demo](https://github.com/Trueysh/hybrid-search-demo) | BM25 vs vector vs RRF retrieval strategy comparison |

## Experience

| When | Company | Focus |
|---|---|---|
| 2025.07 — | Private asset manager | Investment research: RAG · orchestration · knowledge graph |
| 2025.03 — 2025.07 | Medical informatics vendor | Care-navigation agent, intranet deployment |
| 2023.09 — 2025.03 | AI application company | Digital human + RAG product lines |
| 2021.07 — 2023.09 | Medical AI company | Medical NLP · data & eval for a 10B model |

## Contact

[TrueYSH@163.com](mailto:TrueYSH@163.com) · [blog.csdn.net/TrueYSH](https://blog.csdn.net/TrueYSH)
