# Agent Atlas · 实施计划

> 2026-11-18 · 从 AUDIT / CONTENT_ROADMAP / DESIGN_PLAN 汇总的工单
> 原则: P0 先做, P1 跟上, P2 待定, P3 搁置; 每个工单单独 feature 分支 + PR

## 分期总览

| Phase | 目标 | 涉及工单 | 估时 |
|---|---|---|---|
| A | 基础修补(SEO / 移动端 / 准确性) | A1–A4 | 1–2 天 |
| B | 内容补缺(新 5–7 节课) | B1–B6 | 5–7 天 |
| C | 交互实验(5 个 Simulator) | C1–C5 | 7–10 天 |
| D | 阅读体验(进度 / 术语交叉 / 代码复制) | D1–D4 | 2–3 天 |
| E | 案例库(6 个案例) | E1–E6 | 5–7 天 |
| F | 编辑体打磨(暗色 / 减 emoji / 排版) | F1–F3 | 1–2 天 |
| G | 高级(多 Agent / 部署 / 真实 LLM 接入) | 后续 | — |

---

## Phase A · 基础修补 (P0, 1–2 天)

### A1 · 元信息和社交卡 [P0-1]
- 加 favicon.svg(◆ 图形, 单色)
- `<meta name="description">`(首页/深入课/术语库/词条页, 四类 preset)
- og:title / og:description / og:image(首页截图)
- **分支**: `feature/meta-and-social`
- **估时**: 2 小时

### A2 · 内容准确性过一遍 [P0-2]
- 过 5 节课 + 25 词条, 校:
  - 模型 ID(claude-opus-4 → 真实 ID)
  - "token = 字" 的表述加注脚
  - 具体数字(context window)加 "~"
  - 每个厂商特定概念标清出处
- **分支**: `feature/content-accuracy-pass`
- **估时**: 2–3 小时

### A3 · 移动端实测和修补 [P0-3]
- 真 375 / 414 / 768 三种宽度实测
- 顶部导航窄屏改汉堡
- 地图在手机变缩略图或列表
- `.canvas` 不再强制 4:3
- Panel 变 bottom sheet
- **分支**: `feature/mobile-layout`
- **估时**: 半天

### A4 · 基础 a11y + 跑 Lighthouse [P1-8 + 验证]
- 加 `role` landmarks, skip-to-content, SVG `aria-hidden` / role=img+title
- 跑 Lighthouse 记一次基线分数
- 修能 1 小时内处理的低分项
- **分支**: `feature/a11y-baseline`
- **估时**: 半天

---

## Phase B · 内容补缺 (P0–P1, 5–7 天)

每节都是独立 PR (`feature/lesson-<slug>`), 结构沿用 4 步插画 + 右侧 panel + 时间线。

### B1 · Lesson 0 · 什么是 LLM
- Step 1: 从"模型是文件"讲起, 什么是参数 / 权重
- Step 2: 预训练 vs 推理两阶段
- Step 3: token / context 的物理含义(铺后续)
- Step 4: 和 "软件" 的区别 —— 概率输出 / 幻觉的根源
- **估时**: 1 天

### B2 · Lesson · Tool Calling (深入)
- 简报特别强调: **模型请求 vs 运行时执行**要拆清
- Step 1: 模型说"我想调 X"(JSON 请求)
- Step 2: 运行时拦截, 真正跑函数
- Step 3: 结果塞回 messages
- Step 4: 模型下一轮读结果继续
- **估时**: 1 天

### B3 · Lesson · MCP
- Step 1: 为什么要协议(每人一套胶水的痛)
- Step 2: Host / Client / Server 三角
- Step 3: 一个具体示例(Slack server)
- Step 4: 和 function calling 的区别 + 什么时候用
- **估时**: 1 天

### B4 · Lesson · RAG
- Step 1: 场景(闭卷 vs 开卷)
- Step 2: 流水线(chunking → embed → store → retrieve → augment)
- Step 3: 一次完整检索的步骤播放
- Step 4: 常见问题(chunk 太大 / 太小 / reranking / hybrid)
- **估时**: 1 天

### B5 · Lesson · Memory
- Step 1: "没记忆" 是现状(呼应 Lesson 1)
- Step 2: 短期(窗口内)vs 长期(外部存)
- Step 3: 持久化的三种做法: KV / 摘要 / 检索式
- Step 4: 和 RAG 的区别
- **估时**: 1 天

### B6 · Lesson · Planning & ReAct
- Step 1: 直接让模型做复杂事 → 容易跑偏
- Step 2: 显式计划: 先列步骤再执行
- Step 3: ReAct 模式: think → act → observe 循环
- Step 4: 两种模式的代码骨架 + 什么时候选哪个
- **估时**: 1 天

### B7 · Lesson · 权限、安全、Human-in-loop (合一节)
- Step 1: Prompt injection / tool output 不可信
- Step 2: Least privilege / 工具层授权
- Step 3: 人工审批点(危险操作必须人确认)
- Step 4: Sandbox / 审计日志
- **估时**: 1 天

### B-review · 对照 CONTENT_ROADMAP 做一次"区分对"扫描
- Prompt ≠ Skill, Skill ≠ Tool 等 7 对, 在合适位置补"常见误区"小卡
- 1 个 PR 一起扫
- **估时**: 2 小时

---

## Phase C · 交互实验 (P1, 7–10 天)

每个实验独立 PR (`feature/sim-<name>`), 都使用 mock 数据。按用户价值排序:

### C1 · Prompt Playground
- 预置 3 个 system prompt, 1 个 user prompt, 对应 3 个模拟输出
- 可自由改 prompt, 但改了只返回 "用预设试试" 提示
- **估时**: 1 天

### C2 · RAG Simulator
- 预置 10 段中文 corpus + 预计算 embedding(内嵌 JSON)
- 浏览器里 cosine similarity 排序
- 滑块调 chunk size / top-K 看结果差异
- **估时**: 1.5 天

### C3 · Agent Workflow Simulator
- 预录 2 个 "跑 trace" (成功 + 翻车)
- 时间线 + 逐步播放 + 查看每步 message
- **估时**: 2 天

### C4 · MCP Simulator
- 交互 SVG 图, 四个区域可点
- 每个区域右侧显示职责 + 代码 + 误区
- **估时**: 1 天

### C5 · Memory Simulator
- 三栏对照("无记忆 / 短期 / 长期"), 同一对话跑三次
- 可按步骤看每栏 messages 差异
- **估时**: 1 天

---

## Phase D · 阅读体验 (P1–P2, 2–3 天)

### D1 · 阅读进度持久化 [P1-4]
- localStorage 存 "lesson X step Y", 首页卡片显示进度条
- **估时**: 半天

### D2 · 课程 ↔ 术语双向互链 [P1-6]
- 课程 body 里术语自动包 anchor
- hover 显示 one-liner, 点击跳词条
- **估时**: 1 天

### D3 · 代码复制按钮 [P1-7]
- 全站所有 `<pre>` 右上角加 copy
- **估时**: 1 小时

### D4 · 章节间"你会学到" / "前置建议"
- 每节顶部多一行, 提示先后顺序
- **估时**: 2 小时

---

## Phase E · 案例库 (P2, 5–7 天)

每个案例独立 PR (`feature/case-<slug>`), 固定模板 (需求 / 架构 / 流程 / 核心概念 / 代码 / 错误 / 安全 / 延伸)。

- **E1** 邮件助手 — 0.5 天
- **E2** PDF 分析 — 1 天
- **E3** 浏览器自动化 — 1 天
- **E4** MCP 天气 — 0.5 天
- **E5** 代码审查 — 1 天
- **E6** 多 Agent 协作 — 2 天

Python + TypeScript 两版, tab 切换。

---

## Phase F · 编辑体打磨 (P2, 1–2 天)

### F1 · 暗色模式
- 已有 `prefers-color-scheme` 变量准备好, 加 toggle 按钮
- **估时**: 半天

### F2 · 减 emoji
- callout 的 emoji 换成 label (保留 💡 一个)
- **估时**: 1 小时

### F3 · 排版 / 间距一致性扫
- 定义 4 档 spacing (4 / 8 / 16 / 24 / 48), 全站应用
- **估时**: 2 小时

---

## Phase G · 搁置到有需求再做

- 多 Agent / Orchestration / Observability / Deployment 的进阶课
- 真实 LLM 接入(需要 API key 流程)
- 多语言(en 版)
- PDF / EPUB 导出
- 自动化 e2e 测试(Playwright 几个 smoke)

---

## 本轮建议的执行顺序

1. **先 A1 + A2 + A3**(SEO + 准确性 + 移动端, 一天的事, 直接影响每个新用户的第一印象)
2. **再 D1 + D3**(进度 + 复制, 技术简单回报立竿见影)
3. **再 C1 + C2**(Playground + RAG Simulator, 产品定性升级的第一步)
4. **再 B1 + B5**(What-is-LLM + Memory, 是最大的两个知识缺口)
5. **其他按需**

每一步都是独立 `feature/*` 分支 → PR。

---

## 不做 / 暂不做

- 不重写整个项目 —— 现有架构(单文件 SPA)够用, 别换栈
- 不接付费 API —— 所有 Simulator v1 用 mock
- 不加没测过的插画 —— 如果美术资源到位了再换
- 不删现有功能 —— 键盘导航 / 时间线 / 术语卡 4 块结构保留

---

## 风险与依赖

- **FOUC / Google Fonts 加载**: 实测再定, 可能需要 `font-display: swap` + fallback metric
- **SVG 在 Safari 的行为**: 术语库地图 `<a><g></a>` 在 Safari 15 之前可能不响应点击 —— 改为 group onclick 兜底
- **Cloudflare Pages**: 现在每次 push 到 develop 都自动部署。做 PR 流程后应把生产分支切到 main, 防止 WIP 直接上线
- **单文件膨胀**: 当前 index.html 已 3100 行; 到 Phase C 完成会超 6000 行。那时考虑拆模块(至少 CSS / JS / HTML 分离)
