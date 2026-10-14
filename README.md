<a href="https://github.com/Trueysh">
  <img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:0f172a,50:1e3a8a,100:3b82f6&height=200&section=header&text=Yu%20Shenghui%20%E4%BD%99%E8%83%9C%E8%BE%89&fontSize=36&fontColor=ffffff&fontAlignY=36&desc=%E5%A4%A7%E6%A8%A1%E5%9E%8B%E5%BA%94%E7%94%A8%E5%BC%80%E5%8F%91%E5%B7%A5%E7%A8%8B%E5%B8%88&descAlignY=58&descSize=18" alt="Yu Shenghui" />
</a>

<div align="center">

![Focus](https://img.shields.io/badge/方向-RAG_/_Agent_/_评测闭环-2563eb?style=flat-square)
![Medical](https://img.shields.io/badge/领域-医疗AI_4年-334155?style=flat-square)
![Finance](https://img.shields.io/badge/领域-金融投研-1e40af?style=flat-square)
![Python](https://img.shields.io/badge/Python-3776ab?style=flat-square&logo=python&logoColor=white)

</div>

---

> 我做的大多是**从 demo 到生产之间的那一层**——检索正确性、评估闭环、推理成本、故障隔离。
> 这一层的共同点是：出了问题**不报错、不崩、监控全绿**，只是结果悄悄错了。发现和修复它们，靠的不是灵感，是基准和消融实验。

---

## ▍我修过的三个「监控看不见」的问题

**01 · 空值陷阱 —— 84% 的命中是假的**

RAG 检索的过滤条件里，空列表在 Python 中判假，组合过滤的 `or` 被短路放行——查「中际旭创」，返回结果看着正常，人工标注后才发现 84% 是空值放行的假命中。这类 bug 不报错、时延正常、监控无告警。修复 `or→and` 恢复交集语义后，**有效召回 +2.4×**，精确率同步上升。这教会我的事：**检索正确性必须用标注基准验证，不能信任"看起来像那么回事"。**

**02 · 打分错位 —— rerank 在中文金融场景的静默劣化**

rerank 模型对中文金融文本评分异常，正确文档被压到召回之外，而总分看着仍然合理。修复不是调参，是换融合范式：`final = rerank^α × vec^(1-α)` 几何融合，乘性加权保证**单侧异常低分不会被另一侧虚高分数救回来**——这是加性融合（RRF / 线性加权）给不了的性质。

**03 · 算力锁死下的并发 —— 不是堆卡，是拆流量**

医院内网算力一次性采购、不可扩容，上线后要扛多科室并发。解法是**动静分离**：可缓存/可预计算的（高频问答、embedding）与必须实时生成的分流，推理侧批处理 + 上下文剪枝 + 调度摊平峰值。系统在固定算力下稳定投产——**To B 内网和做 demo 的本质区别，就在这。**

---

## ▍生产系统

**金融 RAG 知识库**（在职 · 从 0 到 1）

一个人搭完整条链路：采集 → 解析（MinerU）→ 分块 → 入库 → 混合检索 → rerank → 接口服务。43.2 万 chunk / 8 collections / Milvus HNSW（选 HNSW 不选 IVF 的原因：每天增量入库，IVF 要聚类重训，语料漂移即失效；踩过的坑：`ef` 必须 ≥ `top_k`，否则召回不满）。

| 指标 | 结果 |
|---|---|
| 有效召回（修复假命中后） | **+2.4×** |
| RAGas 忠实度 / 上下文精度 | 0.83→**0.91** / 0.87→**0.95** |
| 自动打标成本 / 延迟 | **1/5~1/10** / 2s→0.4-0.7s |
| 影子灰度复核成本 | **< ¥0.4/天**（挂入库流水线旁路，只观测不写库） |

三级缓存按**重建成本**分层（embedding 1h / 检索+rerank 10min / 会话 30min），缓存 key 含全部检索参数——任一参数变化必然击穿，**这是正确性设计，不是性能优化**。

**医疗智慧就医智能体**（内网真实投产）

Dify Workflow 编排 + XInference 内网自建 BGE-m3 / BGE-reranker，纯内网闭环。突破了 Dify 固定「问题分类器」只给意图不给实体的限制，改**意图识别 + 实体提取双路**，导诊/查询才能正确路由到科室和药品。数字人选型做了取舍：对比图片/视频/3D 后选 Live2D 固定动作 + 音频口型驱动——真实感让位于医院锁死的算力预算。

**数字人问答系统**（产品线负责人）

查询链路 **7s → 300ms**——量纲要拆开讲：7s 是端到端链路，300ms 是检索段。手段：文档解析从 PDF Loader+OCR（表格准确率 ~70%）换成 **MinerU 转 Markdown + 多模态回填（>95%）**，加检索链路分层缓存。ASR（SenseVoice）WER ≤ 10%，链路支撑 200+ 并发。

**研究任务编排系统**（在职）

企业微信 + 状态机自动派单，智能表格作单一事实源。11 人团队任务负载日内极差 **6.81pp → 0.20pp**。

---

## ▍方法论：评估驱动

每个方案先建基准、再做决策——**避免"先假设轻量模型够用，再找证据证明它够用"的自证循环**：

| 手段 | 规模 | 决策 |
|---|---|---|
| 人工裁定基准 | **504 条** | 生产模型 72.2% vs 轻量 71.2%，证明"打平"后才启动替换 |
| A/B 实验 | **1,213 条 × 4 臂** | 结构化 hint 让轻量模型 68.7%→71.7%，追平生产模型，成本降至 1/5~1/10 |
| RAGas 评测 | 迭代评测集 | 忠实度 0.83→0.91；评测集小不报满分，**数字有分寸才可信** |
| 影子灰度 | 常驻生产旁路 | 三重隔离：写库之后 / 只落 jsonl / 异常内部消化 |

2026 年行业从 demo 到生产的断层，卡点不在模型能力，在运营层——评估、回归、灰度、成本监控。**我的经验正好在这一层。**

---

## ▍技术栈

<div align="center">

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat-square&logo=pytorch&logoColor=white)
![Milvus](https://img.shields.io/badge/Milvus-00A1C1?style=flat-square&logo=milvus&logoColor=white)
![Elasticsearch](https://img.shields.io/badge/Elasticsearch-005571?style=flat-square&logo=elasticsearch&logoColor=white)
![LangChain](https://img.shields.io/badge/LangChain-1C3C3D?style=flat-square&logo=langchain&logoColor=white)
![Dify](https://img.shields.io/badge/Dify-1C3C3C?style=flat-square&logo=dify&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Neo4j](https://img.shields.io/badge/Neo4j-008CC1?style=flat-square&logo=neo4j&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-DC382D?style=flat-square&logo=redis&logoColor=white)

</div>

RAG 全链路：MinerU 解析 · 动态分块 · BGE / BGE-m3 · 稠密+稀疏混合检索 · 几何融合 / RRF · 语义去重 · LLM 重排 · 引用溯源 · RAGas 评测
模型侧：Qwen / DeepSeek(-R1) · SFT / LoRA · 量化与推理加速 · KV Cache / 批处理调优
工程侧：内网离线私有化部署 · 缓存分层 · 限流熔断 · 幂等写入 · 影子灰度

---

## ▍开源

| 项目 | 亮点 |
|---|---|
| [ICU_Mediapipe](https://github.com/Trueysh/ICU_Mediapipe) **★27** | ICU 患者手部行为监测：21 关键点细粒度行为判定 · 停留时长分级报警（时间维滤误报）· ROI 划定（空间维滤误报）· 单文件→模块化重构 |
| [hybrid-search-demo](https://github.com/Trueysh/hybrid-search-demo) | BM25 / 纯向量 / RRF 三策略消融实验：用数据回答「哪类查询该走哪条检索路径」 |

---

## ▍经历

| 时间 | 公司 | 方向 |
|---|---|---|
| 2025.07 — | 某私募资管机构 | 投研智能化：RAG 知识库 · 任务编排 · 知识图谱双模服务 |
| 2025.03 — 2025.07 | 某医疗信息化公司 | 智慧就医智能体，内网投产 |
| 2023.09 — 2025.03 | 某 AI 应用公司 | 数字人 + RAG 两条产品线负责人 |
| 2021.07 — 2023.09 | 某医疗 AI 公司 | 医疗 NLP · 10B 模型数据与评估模块（10T 语料三层清洗 · 自动化评估替代 80% 人力） |

---

## ▍联系

<div align="center">

[![Email](https://img.shields.io/badge/Email-TrueYSH%40163.com-2563eb?style=flat-square&logo=protonmail&logoColor=white)](mailto:TrueYSH@163.com)
[![Blog](https://img.shields.io/badge/CSDN-@TrueYSH-c20c0c?style=flat-square)](https://blog.csdn.net/TrueYSH)
[![GitHub](https://img.shields.io/badge/GitHub-Trueysh-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/Trueysh)

</div>

<img width="100%" src="https://capsule-render.vercel.app/api?type=waving&color=0:3b82f6,50:1e3a8a,100:0f172a&height=120&section=footer" alt="footer" />

---

<sub>公司与项目名称已脱敏；数字口径与简历一致，可交叉验证。</sub>
