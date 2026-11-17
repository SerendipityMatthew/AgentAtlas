# Agent Atlas · 知识架构规划

> 2026-11-18 · 对照简报的 25 个主题, 定出完整学习路径
> 原则: 不机械 1:1 做 25 节; 按知识依赖合并 / 拆分

## 当前覆盖率

### 深入课 (5 节 · 每节 4 步插画)
| 节 | 标题 | 覆盖的核心概念 |
|---|---|---|
| 1 | 什么是 chat | 对话 = 消息列表, token 流式生成, context 增长 |
| 2 | 什么是 prompt | system vs user 消息, prompt engineering, hallucination |
| 3 | 什么是 skill | Skill 的结构, skill 库, 和 prompt 的差别 |
| 4 | 什么是 agent | agent = LLM + tools + loop, 工具调用 |
| 5 | agent 怎么跑起来 | while 循环, 三种翻车, 最小 runner |

### 术语库 (25 词 · 7 组)
| 组 | 词 |
|---|---|
| 模型基础 (5) | token, tokenizer, embedding, attention, context window |
| 提示工程 (4) | system prompt, few-shot, CoT, hallucination |
| 推理参数 (3) | temperature, top-p, streaming |
| 检索 RAG (3) | RAG, vector db, chunking |
| 微调训练 (3) | fine-tuning, LoRA, RLHF |
| 智能体与工具 (4) | tool call, function calling, MCP, agent loop |
| 评估与安全 (3) | benchmark, jailbreak, eval |

---

## 对照简报的 25 个主题

### Foundation (基础)
| # | 简报主题 | 现状 | 差距 |
|---|---|---|---|
| 1 | 什么是 LLM | ❌ 直接从 chat 讲起, 没铺"模型是什么" | **新开 Lesson 0**(含 pretraining/推理两阶段) |
| 2 | Token / Context Window | ✓ 词条有 | 词条已够; 不新开课, 融进 Lesson 0 一个步骤 |
| 3 | Prompt Engineering | ✓ Lesson 2 | 可加 "常见反模式" 补强 |
| 4 | System / User Prompt | ✓ Lesson 2 Step 2 | 覆盖 |
| 5 | Structured Output | ❌ 没有 | **新词条 + Lesson 2 加一步讲 JSON mode** |
| 6 | Embeddings | ✓ 词条 | 够深; 若做 RAG 深入课可借用 |

### Building Blocks (组件)
| # | 简报主题 | 现状 | 差距 |
|---|---|---|---|
| 7 | Skill | ✓ Lesson 3 | 覆盖 |
| 8 | Tool Calling | ✓ 词条 + Lesson 4 | **把"模型发出调用 vs 运行时执行"拆清楚** (简报特别强调) |
| 9 | MCP | ✓ 词条 | **升级为 Lesson** (词条讲不透协议) |
| 10 | Memory | ❌ 没有 | **新开 Lesson** (重大缺口; 简报要求区分短期/长期/检索) |
| 11 | RAG | ✓ 词条 | **升级为 Lesson** (和 vector-db/chunking/embedding 一节讲) |
| 12 | Agent Runtime | ✓ Lesson 5 | 覆盖 |

### Agent Architecture (架构)
| # | 简报主题 | 现状 | 差距 |
|---|---|---|---|
| 13 | 什么是 AI Agent | ✓ Lesson 4 | 覆盖 |
| 14 | Planning | ❌ 没有 | **新开 Lesson** (和 ReAct 合并) |
| 15 | ReAct | ❌ 没有 | 和 Planning 合一节, 2 步讲模式 + 2 步讲代码 |
| 16 | Agent 如何执行 | ✓ Lesson 5 | 覆盖 (就是 runtime) |
| 17 | 失败处理 | ✓ Lesson 5 Step 3 (三种翻车) | 够; 可扩成独立的"容错模式"词条 |
| 18 | Human-in-the-loop | ❌ 没有 | **新开 Lesson** (或作为 Permissions 的一步) |
| 19 | 权限与安全 | ❌ 没有 | **新开 Lesson** (融入简报 Phase 7 的安全内容) |

### Advanced
| # | 简报主题 | 现状 | 差距 |
|---|---|---|---|
| 20 | Multi-Agent | ❌ | **新开 Lesson** (强调 "不一定比 single 更好") |
| 21 | Orchestration | ❌ | 和 Multi-Agent 合, 或独立一节 |
| 22 | Communication | ❌ | 可并进 Orchestration |
| 23 | Evaluation | ✓ 词条 | **升级为 Lesson** (讲 LLM-as-judge / 人工评 / 对抗测) |
| 24 | Observability | ❌ | 新词条 + 可能一节 |
| 25 | Deployment | ❌ | 新词条 + 可能一节 |

---

## 建议结构(重组后)

**原则**: 新增 7 节左右, 不是 25 节。砍重合, 按依赖分章。

### 第一章 · Foundation (5 节)
- **Lesson 0**(新)—— 什么是 LLM: 预训练 / 推理两阶段, token, context window
- Lesson 1 chat (保留)
- Lesson 2 prompt (保留 + 加一步 "Structured Output")
- **Lesson 2.5**(新)—— Embeddings: 语义向量化, 为后面 RAG 铺路
- (可选) "常见误区": prompt 不等于 skill, LLM 不等于 agent, ...

### 第二章 · Building Blocks (5 节)
- Lesson 3 skill (保留)
- **Lesson 4**(新)—— Tool Calling 深入: **把"模型请求调用"和"运行时执行"拆成 4 步分别讲**(简报强调点)
- **Lesson 5**(新)—— MCP: 为啥要协议 + Host/Client/Server 三者 + 对比直接 function calling
- **Lesson 6**(新)—— RAG: embedding + chunking + vector db + retrieval + 如何塞进 prompt
- **Lesson 7**(新)—— Memory: 短期上下文 vs 长期持久化 vs 检索式记忆 vs 更新

### 第三章 · Agent (6 节)
- (原 Lesson 4 → 第三章 Lesson 1) 什么是 agent
- **新 Lesson** —— Planning & ReAct: 两种执行范式
- (原 Lesson 5 → 第三章 Lesson 3) Agent 运行时
- **新 Lesson** —— 失败处理与恢复: 把 Lesson 5 Step 3 扩成独立一节, 加重试模式 / Fallback / Circuit breaker
- **新 Lesson** —— Human-in-the-loop: 审批点设计 / 中断-恢复
- **新 Lesson** —— 权限与安全: least privilege / tool output validation / prompt injection / sandboxing

### 第四章 · Advanced (3-4 节, 可按需扩)
- Multi-Agent 概览(强调不是银弹)
- Orchestration & Communication(合一节)
- Evaluation(扩自词条)
- Observability + Deployment(合一节)

### 术语库扩展(v2)
目标: 60–80 词。按分组补:
- 模型基础: parameters, pretraining, fine-tuning data, perplexity, logprobs
- 提示: prompt injection, output format, stop sequence, structured output, JSON mode
- 推理: max tokens, frequency penalty, presence penalty, logit bias
- RAG: hybrid search, reranking, HyDE, graph RAG
- 微调: SFT, DPO, PPO, quantization, distillation
- 智能体: planning, ReAct, ReWOO, tree search, reflection, self-consistency, scratchpad, memory, state machine
- 安全: prompt injection, jailbreak details, sandbox, permission, audit, guardrail, red team
- 评估: LLM-as-judge, pairwise, MT-Bench, SWE-bench, task success rate
- 部署: KV cache, batching, latency/TPS, cold start

---

## 需要在多处明确区分的概念对

(简报重点强调)

| A | ≠ | B | 在哪里解释 |
|---|---|---|---|
| Prompt | ≠ | Skill | Lesson 3 开头 + 两个术语卡"相关词"补 "不要混为一谈" |
| Skill | ≠ | Tool | Lesson 4 (Tool Calling) 开头 |
| MCP | ≠ | Agent | MCP 新课的 Step 1 |
| LLM | ≠ | Agent | Lesson 0 结尾 + Lesson 4 对比 |
| Tool Calling | ≠ | 工具执行 | Tool Calling 新课的核心: 4 步分别讲 |
| RAG | ≠ | Memory | Memory 新课开头明确对比 |
| Multi-Agent | ≠ | 更好 | Multi-Agent 一节的前置免疫: "什么时候真的需要" |

---

## 厂商归属

简报要求: 区分"通用概念"和"厂商特定实现"。

- **通用** (概念本身): LLM, token, context, prompt, embedding, RAG, tool call, agent loop, planning, ReAct
- **厂商特定**(标清是哪家):
  - MCP = Anthropic 发起的协议(有标准化意图, 多方实现)
  - Function Calling = OpenAI 命名, 现成通用做法
  - Structured Output / JSON mode = OpenAI & Anthropic & Google 各有实现, 细节不同
  - Claude Projects / ChatGPT GPTs / Gemini Gems = 产品层包装, 别和通用 "skill" 概念混
  - LoRA / QLoRA = Microsoft/UW 论文, 但 HuggingFace PEFT 是常用实现

每节/每条涉及厂商实现的地方, 加一行 "参考文档" 链接到官方 docs。
