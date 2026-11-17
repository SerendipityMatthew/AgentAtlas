# Agent Atlas · 术语库(Glossary) 设计

> 2026-11-18 · 对话中确定的方案, 开工前一致性文档

## 目标

把当前 AI/LLM 圈常用的 50-80 个术语, **结构化地**、带视觉地、给非程序员和开发者都能看地, 讲一遍。接在 Agent Atlas 现有的 5 节"深入课"之后, 作为一个独立板块。

## 这是什么 · 这不是什么

**是**:
- 一本可查的术语小书
- 每个词有统一的卡片格式
- 一张"地铁图"做入口, 看清所有词之间的关系

**不是**:
- 训练理论教科书(梯度下降、反向传播、loss 函数细节不讲)
- benchmark 排行榜(数字会过时)
- 前沿论文复述
- 完整 LLM 开发教程(讲到"是什么", 不教"怎么训")

## 架构

### 1. 怎么挂进 Agent Atlas

顶部导航加一项:

```
◆ Agent Atlas    [ 深入课 ]  [ 术语库 ]        主页 ◇
```

路由:
- `#/glossary` → 术语库首页(地图 + 分组列表)
- `#/glossary/<slug>` → 单个术语页

现有 5 节(chat/prompt/skill/agent/runtime)保留在"深入课", 不动。

### 2. 术语分 7 个主题组

| 组 | 覆盖范围 | 代表词 |
|----|---------|-------|
| 模型基础 | 模型本身的结构和训练出来的东西 | token, embedding, attention, transformer, parameters, pretraining, context window |
| 提示工程 | 怎么跟模型说话 | system/user/assistant, few-shot, CoT, prompt injection, hallucination |
| 推理参数 | 调模型行为的旋钮 | temperature, top-p/top-k, streaming, max tokens, logprobs, stop sequence |
| 检索 (RAG) | 让模型用上外部知识 | embedding (作为检索)、vector db, chunking, retrieval, reranking, hybrid search |
| 微调与训练 | 定制模型 | SFT, RLHF, DPO, LoRA/QLoRA, distillation, quantization |
| 智能体与工具 | 让模型动手 | tool call, function calling, MCP, skill, agent loop, memory, planning |
| 评估与安全 | 衡量和保护 | benchmark (MMLU/HumanEval), jailbreak, red-team, guardrail, eval harness |

每组用一个固定颜色(复用 Agent Atlas 现有的 7 色: 绿/黄/粉/蓝/紫/米/暖灰)。

### 3. 单词卡结构(统一)

```
┌──────────────────────────────────────┐
│  [ 组标签 ]                 中文名   │
│  English Name                        │
│  ─────────────────────────────────   │
│  💡 一句话定义                       │
│  (给非程序员的, 不带术语的那种)     │
│                                      │
│  🔀 比喻                             │
│  "就像 X 一样..."                    │
│                                      │
│  📐 具象化                           │
│  (一张图 or 一段短代码)              │
│                                      │
│  🔗 常一起出现                       │
│  → [term A]  → [term B]  → [term C]  │
└──────────────────────────────────────┘
```

4 块固定不变, 顺序固定, 保证扫视速度快。

### 4. 地图(术语库首页)

地铁图式:
- 中心: "LLM" 大节点
- 7 条支线分别连向 7 个主题组
- 每条支线上串着该组下所有术语节点(圆点, 彩色)
- 鼠标悬停某个术语 → 弹出一句话定义
- 点击术语 → 跳到该术语的卡片页

视觉:
- 底色沿用现有 `--bg`
- 支线颜色沿用对应组的主题色
- 圆点大小标识重要度(入门必懂 vs 专业)

### 5. 和"深入课"的互链

- 术语卡的"常一起出现"链接里, 如果某个词已有"深入课"(比如 `agent`), 多一个按钮 "📖 深入看这一节 →" 跳去对应的 chat/prompt/skill/agent/runtime 页面。
- 深入课的开发者注脚里, 术语第一次出现用 `<a>` 链到术语卡, 不打断阅读但能查。

## 分期

### v1(先做的)

- 地图骨架 + 7 组分区 + 导航挂进去
- **25 个核心词**, 每组 3-4 个, 保证所有组都有东西:
  - 模型基础: token, embedding, attention, context window
  - 提示工程: system prompt, few-shot, chain-of-thought, hallucination
  - 推理参数: temperature, top-p, streaming
  - 检索: RAG, vector db, chunking
  - 微调: fine-tuning, LoRA, RLHF
  - 智能体: tool call, function calling, MCP, agent loop
  - 评估: benchmark, jailbreak, eval

跑通一天工作量。拿到你反应之后决定是否 v2。

### v2(可选, 后续)

- 扩到 60-80 词(主要补冷门/深入词)
- 加搜索(ctrl+K, 像 Linear/Cmd+K 那种)
- 术语之间的"相关度"连线(增强地铁图)

## 工作量

- v1: 1-1.5 天(地图 SVG 最花时间, 卡片是批量铺)
- v2: 1-2 天

## 决策记录(开工前确认)

- [x] 做 C(词典) + A(地图), 不做 B(更多课程章节)
- [x] 术语 50-80 个, v1 先做 25
- [x] 卡片格式: 4 块固定(定义/比喻/具象/相关词)
- [x] 中英双名(标题)
- [x] 卡片和深入课双向互链
- [ ] 分期 v1 → v2, 等你反应再扩

## 留给实现的

- 词条文案(以你说过的"非程序员+开发者"双层写法为准, 不再问)
- 具象化用"图 vs 代码"由词本身决定(偏抽象的用图, 偏操作的用代码)
- 地图具体布局、线的弯折走向(spike 时视觉上调, 不预先定)
