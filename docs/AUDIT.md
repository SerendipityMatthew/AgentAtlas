# Agent Atlas · 现状审查

> 2026-11-18 · 基于线上 https://agentatlas-85r.pages.dev/ + 仓库源码
> 审查者: Claude Opus 4.7 · 结合源码与实际渲染 (未做跨浏览器/真机测试)

## TL;DR

项目目前是一个**单文件 SPA**(`index.html`), 已上线 Cloudflare Pages。
- 5 节"深入课"(chat → runtime)
- 25 词术语库 + 地铁图
- 顶部 tabs + hash 路由(SPA)
- 整体视觉和交互在"宁静编辑体"这条线上是稳的

主要短板集中在三块:
1. **元信息** —— 几乎没做(无 description / og / favicon)
2. **移动端** —— 只有一条 @media 兜底, 未经真机验证
3. **交互深度** —— 当前全部是"讲解", 简报要求的"实验"(Playground / Simulator)一个都没有

详见下方按 P0–P3 列出的 23 条问题。
不含: 真机测试、Lighthouse 跑分、a11y 全扫 —— 这些是 Phase A 第一步该做的事。

---

## P0 · 必须解决(体验或可信度问题)

### [P0-1] 社交分享和爬虫几乎看不到内容
- **现状**: `<meta name="description">` / `og:*` / `twitter:*` / `favicon` 全部缺失
- **问题**: 发到任何聊天工具都是一个空白预览; 搜索引擎抓取只能靠 title; 浏览器 tab 没图标, 多开后认不出
- **为什么要修**: 教学产品第一印象是分享链接。空白分享卡 ≈ 没做
- **方案**: 加 description / og:title / og:description / og:image(从首页截一张) / favicon.svg
- **成本**: < 1 小时
- **收益**: 分享/收藏率明显改善

### [P0-2] 内容准确性有几处需要修
- **现状**: 当前课程和词条写作偏"讲通"而不是"精确", 部分表述需要校正
  - Lesson 1 chat: 把"token"等同"字", 中文大致成立但英文 `"Hello"` 其实多半是 1 token、`"Hello,"` 是 2 token, 现在的例子 `"Hello, world" → ["Hello", ",", " world"]` 很接近真实但该更明确标注"实际由 tokenizer 决定"
  - Lesson 4 agent code: 示例 `model="claude-opus-4"` 不是真模型 ID(实际是 `claude-opus-4-5` 或类似带版本), 容易被读者直接复制
  - 术语 context-window: Claude 4.7 的 1M 是具体数字, 发布版本可能变更 —— 建议标 "~"
- **问题**: 简报明确要求 "不能为了简化教学而牺牲技术准确性"
- **方案**: 过一遍所有课程和词条, 标注实际模型 ID、给数字加 "~"、给过度简化加一行"注脚"
- **成本**: 2–3 小时(校对 + 修改)
- **收益**: 可信度, 以及减少用户复制示例踩坑

### [P0-3] 移动端只有一条兜底规则, 没真机验证
- **现状**: 只有 `@media (max-width: 920px)` 单一断点, 做法是把网格压成单列
- **问题**:
  - `.stat-row` 在很窄的手机上 `grid-template-columns: repeat(2, 1fr)`, 等宽字体 label 会挤
  - 术语库地图 SVG 是 `aspect-ratio: 4 / 3`, 在 375px 宽的手机上文字会小到看不清
  - 顶部 `.top-tabs` 和 lesson 的 `.n-btn` 在窄屏可能溢出 header
  - 课程面板 `.panel` sticky top 72px 在手机上可能挡内容
- **问题**: 简报明确说 "移动端不能只是简单缩小桌面布局"
- **方案**: 在真实 375 / 414 / 768 三种尺寸走一遍所有页面, 该重排的重排, 地图在手机上改成"卡片列表 + 一张缩略图"
- **成本**: 半天
- **收益**: 这是最可能从手机点进来的产品类型, 修了直接影响转化

---

## P1 · 重要改进(用户价值明显)

### [P1-4] 没有阅读进度 / 完成状态
- **现状**: 刷新或关闭页面, 一切从头开始
- **方案**: `localStorage` 存"读到第几节第几步", 首页卡片上标"读过/读到 X 步/未读"; 术语库里标"已查过"
- **成本**: 半天
- **收益**: 回访率, 简报列为必做

### [P1-5] 没有"实验/模拟器" —— 这是简报的主升级点
- **现状**: 当前全部是讲解, 没有用户可动手改的东西
- **方案**: 按简报的 5 个 Simulator 做, 都是 mock 数据(不连真 LLM):
  - Prompt Playground —— 改 system prompt 看预设响应变化
  - Agent Workflow Simulator —— 播放一次 Agent 任务的时间线
  - MCP Simulator —— 可点击的拆解图
  - RAG Simulator —— 调 chunk size / top-K 看检索结果
  - Memory Simulator —— 无记忆 vs 有记忆的对照
- **成本**: 每个 1–2 天, 优先做 Playground 和 RAG
- **收益**: 从"读"升级到"玩", 是产品定性变化

### [P1-6] 课程 → 术语的反向链接缺失
- **现状**: 术语卡 → 深入课(单向, 4 个词有)
- **问题**: 用户在课程里看到"token"、"embedding"这些词, 没法 hover 看到简释或跳到词条
- **方案**: 在课程文案里把术语名加上 `<a data-term="token">` 标记, hover 显示词条一句话 + 跳转
- **成本**: 1 天
- **收益**: 阅读连贯性, 简报明确要求

### [P1-7] 代码块无"复制"按钮
- **现状**: 所有 `<pre>` 都要用户手动选中复制
- **方案**: `navigator.clipboard.writeText()`, 每个代码块右上角一个按钮
- **成本**: 1 小时
- **收益**: 示例可用性

### [P1-8] 可访问性基线缺
- **现状**: 页面没有 landmark role / skip-to-content / 装饰性 SVG 没标 `aria-hidden`
- **方案**: 加 `<main role="main">` / `<nav>`、给 SVG 图加 `aria-hidden="true"` 或 `role="img"` + `<title>`
- **成本**: 半天
- **收益**: 屏幕阅读器可用性 + Lighthouse 加分

### [P1-9] FOUC(字体加载闪烁)
- **现状**: Google Fonts 直接 `<link>`, 首屏会从系统字体切到 Noto Serif SC/Fraunces, 有可见闪烁
- **方案**: 用 `font-display: swap`(已自动, 可能够了)+ `<link rel="preload">` 关键字体 + fallback metric 匹配
- **成本**: 1 小时
- **收益**: 感知加载速度

### [P1-10] 内容知识盲点
- **现状**: 简报列的 25 个概念, 当前覆盖率约 50%(见 CONTENT_ROADMAP.md 对照表)。缺口最大的: Memory / RAG 深入课 / MCP 深入课 / Human-in-loop / Permissions & Security / Planning / ReAct
- **方案**: 见 CONTENT_ROADMAP.md
- **成本**: 见 IMPLEMENTATION_PLAN.md
- **收益**: 覆盖完整才能叫"全貌"

---

## P2 · 体验优化(可延后)

### [P2-11] "深入课" / "术语库"两 tab 并列, 用户不一定知道该先点哪个
- 加一行短文: "新手建议从深入课开始; 术语库用来查"

### [P2-12] 首页 CTA 卡 + 顶部 tab 都叫"术语库 →", 有点重复
- 卡片可改成 "25 个关键术语, 带比喻 →" 之类更具体

### [P2-13] 整体 emoji 偏多 (💡 🔑 ⚙ 📐 🔀 等), 跟简报希望的"克制编辑体"有张力
- 替换为纯排版的 label 或极简 ASCII glyph(`▸` `▾`)

### [P2-14] 术语库地图在 Safari 对 `<a>` 包 SVG 子节点的支持要确认
- 换成 `onclick` + `cursor: pointer` 更稳

### [P2-15] 术语卡的"相关词"链接是单层, 没显示"距离"或"属于哪一类"
- 相关词链接前可加组色圆点

### [P2-16] 课程里"KEY IDEA"卡片风格不统一, 有的用 emoji 开头, 有的用 "KEY IDEA" 字样
- 统一成: 一个小色块 + 全大写标签, 不用 emoji

### [P2-17] Timeline 步骤条只支持 4 步 —— 未来某节如果 6 步会挤
- 一次性扩成 grid-template-columns: repeat(var(--count), 1fr)

### [P2-18] SVG 插画是几何形(矩形+多边形),不是真"手绘感"
- 后面若有美术资源, 可逐个替换; 现版本够用

---

## P3 · 未来可以考虑

### [P3-19] 多语言 (zh / en) 并行
- 代码上 i18n 不难, 翻译是人力。先做中文版用稳

### [P3-20] 真 LLM 接入(Playground 升级版)
- 简报说第一版用 mock, 这是 v2 的事。要做需加 API key 流程

### [P3-21] PDF / EPUB 导出
- 给想离线看的用户

### [P3-22] 用户笔记(每页加 note)
- 加 localStorage 存

### [P3-23] 测试(端到端)
- 目前零测试, 全靠手动点。Playwright + 几个 smoke 就够

---

## 没验证到的(下一轮审查该做)

- Lighthouse 跑分(性能/a11y/SEO/最佳实践数字)
- 真机(iPhone/Android/iPad)实测
- 跨浏览器(Safari / Firefox / Edge)渲染一致性
- 大段键盘/屏幕阅读器 flow
- 每个 term 页和 lesson 页逐个点一遍回归

这些留给 IMPLEMENTATION_PLAN.md 的 Phase A 第一个工单。
