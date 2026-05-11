---
title: Article Reader Helper
date: 2026-05-11
status: APPROVED
mode: Builder
---

# Design: Article Reader Helper

日期: 2026-05-11
状态: APPROVED
模式: Builder

## 问题陈述

读完文章就忘 — 看的时候觉得好，几天后想不起来内容，更想不起来跟自己的思考有什么关系。同时也想"有效阅读"：在投入读之前能判断这篇值不值得深读，读的过程不是被动消费。

核心痛点是**主动加工强度不够** + **没有跟个人知识形成闭环**。Readwise / Matter 解决了"存"，但没解决"消化"和"连接"。

## 这个东西酷在哪

不是"另一个稍后读"，而是一个**会逼你思考、还会从你过去的笔记里捞出相关内容来碰撞**的读伴。它的独特性来自两件事的合体：

1. **主动消化（real-time forcing）：** 边读边被 AI 问 forcing questions —— 不是被动接收信息，而是当场被逼着输出理解、立场、反驳。
2. **过往激活（vault context）：** 读到某个观点时，工具自动告诉你"你上个月读的 X 在这点上跟它矛盾"或"你的笔记里有个未回答的问题这篇刚好回答了"。

单看每个都不新；合体在 Obsidian 原生环境里跑、用你自己的 vault 当大脑 —— 这是属于"vault-thinker"的专属工具。

## 约束

- **Side project，自己用为主。** 不为多用户优化产品体验。
- **结构上不阻碍未来开源。** 别把架构焊死成单人脚本。
- **输入/输出非对称：**
  - **输入：扔个链接就开始读** — 在 Claude Code 终端、未来可能浏览器/手机，但**不强制从 Obsidian 启动**
  - **输出：写回 Obsidian vault** — vault 是大脑，是终点
- **读发生在对话界面里**（Claude Code chat），不在 Obsidian 内嵌读 pane。
- **时间预算：** Phase C 一周内出能用版本。

## 前提

1. 真正的瓶颈是"主动加工强度"，不是"保存与回顾"。光高亮+回顾不够。
2. Obsidian vault 既是记忆也是反应面。工具价值随 vault 增长而增长（飞轮）。
3. 浏览器和 Obsidian 是双入口，但应汇入同一套处理逻辑 —— core 与 UI 解耦。
4. 自己是唯一用户。结构上不阻碍未来开源，但**不为多人优化**。

## 考虑过的方案

### 方案 A: Obsidian 原生插件 — "逼问读"模式
在 Obsidian 里粘贴 URL → 抓正文 → 渲染在 reading pane → 边读边弹 forcing questions（基于内容 + vault 相关 note）→ 回答和高亮整理成新 note，双链到相关已有 note。

工作量 S-M，单一环境最快做出 daily-usable 版本。冷启动也有价值（forcing questions 不依赖 vault）。但不能在浏览器原生场景拦截，mobile 弱。

### 方案 B: 双入口 + 共享 core
本地 CLI/server 引擎（抓取、LLM、vault RAG、写入），上面挂 browser extension + Obsidian plugin 两个 UI。工作量 L，架构正确但容易在 plumbing 上烧光时间，side project 经常死在这种架构上。

### 方案 C: "Agent 陪读" — 零 UI 原型
不做 UI。Claude Code skill 或 Claude.ai project，把 vault 作为 context，工具调用取网页 / 写 markdown 回 vault。对话即陪读。工作量 XS（几小时到一天）。验证核心 loop 用，不是终态产品。

## 推荐方案：**Phase C 即终态**（先前的 A 阶段已不必要）

**澄清后的关键转折：** 用户期望的是"扔链接就读，读发生在对话里，输出写回 vault" —— 这意味着 skill 本身就是完整产品形态。Obsidian 插件（原 Phase A）只是给同一件事换个壳，**不增加核心价值**，砍掉。

**Phase C（第一周）：用 skill 跑活体原型**

目标是回答两个核心未知：
- AI 逼问真的能让你"沉淀下来"吗，还是只是看起来酷？
- vault 里的过往笔记被自动召唤出来，是 delight 还是噪音？

**Phase C+（如果验证通过）：迭代深化，而非换形态**

省下来的"做插件"的精力投入到：
1. **Forcing questions 的设计** — 语气、密度、触发时机、何时该 challenge 何时该让你过
2. **vault 召唤的智能化** — 从 dumb grep 升级到 embedding，做跨笔记关联
3. **输出格式打磨** — 跟你 vault 里现有 article note 格式完美对齐，让它"看起来不像 AI 写的"
4. **入口扩展（可选）** — 浏览器 bookmarklet 把 URL 喂给 skill；iPhone share sheet 同理。**都不是新 UI，只是新触发方式**

**永远不需要 Obsidian 插件** —— 除非有一天你发现"对话式读"本质上输给了"页面内嵌读"。但这是一个未来才需要回答的问题。

## 先例 (Prior Art)

`lijigang` 在 GitHub 上发布了三个高度相关的 Claude Code skills：

- [`ljg-skill-xray-article`](https://github.com/lijigang/ljg-skill-xray-article) — 给一篇文章做"X光"：提取真问题、论证骨架、ASCII 拓扑图，再跟个人知识（soul.md + memory.md）做 cognitive collision，输出 insight cards
- [`ljg-skill-xray-book`](https://github.com/lijigang/ljg-skill-xray-book) — 三轮认知压缩（骨架/解剖/抽魂），"餐巾纸极限压缩：公式 + 草图 + 一句话"
- [`ljg-skill-paper`](https://github.com/lijigang/ljg-skill-paper) — 学术论文版本，输出"连贯的认知旅程"

**关键相同点：** 架构（Claude Code skill）、个人知识锚定（他的 soul.md ≈ 你的 vault）、输出回笔记系统、核心哲学（"它说了什么 + 对我意味着什么"，跟你的 framing 几乎一字不差）。

**关键差异点 — 这是你的真正 wedge：** lijigang 的工具是 **batch / one-shot**：AI 做完所有分析 → 给你一份报告。你的工具是 **interactive forcing**：AI 不替你读，AI 逼你读 —— 读的过程中实时问你、challenge 你、让你不能假装读懂。

这两条路是同一空间里的不同 bet：
- lijigang 的 bet：高质量分析报告本身就能促进沉淀
- 你的 bet：沉淀必须经过"你被迫输出"这一步 —— 别人给你的报告再好也不够

你说的"傻读"恰好就是在反对前一种模式 —— 即使 AI 给你完美 xray，本质上你还是被动接收。

**对 Phase C 计划的影响：** 不要从零写。Fork `ljg-skill-xray-article` 作为起点，改造它的核心循环 —— 把"一次性输出报告"改成"分段读 + 每段后弹 forcing question + 等你回答 → 根据**你的回答**而非原文总结"。保留它已经做好的：正文抓取、vault 锚定、笔记输出。这样第一周就能跑起来，并且直接在已被验证的设计上做差异化实验。

## 未解决的问题

1. **Forcing questions 的语气和密度：** 苏格拉底式追问？还是更直接的"你怎么看 X"？每段一问还是读完总结一问？这是 C 阶段要试出来的。
2. **vault 召唤的触发时机：** 读完才连接，还是读到关键句子时实时弹出？后者更酷但可能打断阅读节奏。
3. **"有效阅读"（triage）这条线是否要进 Phase C：** "这篇值不值得读"是另一个独立 loop。建议 Phase C 先专注"沉淀"，triage 留到 A 阶段考虑。
4. **正文抓取的鲁棒性：** Substack / Twitter thread / PDF / 微信公众号 —— 各种渠道抓正文的失败率？C 阶段可以容忍手动粘贴正文。

## 成功标准

**Phase C 第一周成功 =** 你**自己**主动用了至少 5 次，至少 3 次产出的 note 后来被你引用 / 链接到别的笔记中。

**长期成功 =** 4-6 周后，你"扔 URL → 走完一篇文章"的频率 ≥ 你以前往 vault 里写一篇 article note 的频率。换句话说：**这个工具变成了你写 article note 的默认入口**，而不是一个偶尔好玩的实验。

不衡量"别人用没用" —— 那是未来的事。

## 下一步

具体一步可执行的行动：

**今天/本周：** 起 Phase C 原型。最小路径：

1. **Fork `ljg-skill-xray-article`** 作为起点 — 不要从零写
2. **改造它的核心循环：** 从"一次性输出报告" → "分段读 + 每段后弹 2 个 forcing questions + 等你回答 → 根据**你的回答**总结"
3. **把它的 `soul.md` + `memory.md` 锚定路径** 替换为你的 Obsidian vault 路径（先 dumb grep / 文件名匹配，不上 embedding）
4. **保留** 它已做好的：正文抓取、cognitive collision cards 模板、Org → 改成 Obsidian markdown 输出
5. **试一篇你真想认真读的文章** — 记下哪些瞬间 work、哪些瞬间 awkward
6. 第一个 session 结束后回到这份 doc，更新"未解决的问题"

如果一周后你**自己**主动用了 ≥5 次，且至少 3 次产出 note 被引用 — 进 Phase A。否则回头改 forcing question 的语气/密度/触发时机。

## 过程中我注意到的

- 你在回答"最酷的版本"时选了"1和2感觉都不错"。我读到的是：你不喜欢二选一式的窄化 —— 你更倾向把多个能力合起来看完整 loop。这是 vault-thinker 的特征，但需要警惕：side project 第一版如果同时做两件事，常常两件都做不深。Phase C 里先专注"逼问消化"这一条，"vault 召唤"可以最简陋开始（甚至先不做），把另一条的 magic 留到验证完第一条再加。

- 你说"就是我的 obsidian 本身，现在相当于是我很多工作的 vault"。这句话告诉我：vault 对你不是工具，是工作的本体。这意味着任何写回 vault 的格式都不能乱 —— 文件名、目录结构、双链格式要跟你既有习惯一致。Phase C 跑之前，建议先看一眼你现有 article-related note 长什么样，让 agent 产出 mimic 那个格式。

- 你补了"提高读文章的效率，不是自己在傻读"。"傻读"这个词很重 —— 它说明你对"被动消费"是有羞愧感的。这个工具的情感价值其实不只是功能性的"帮我记住"，也是"让我对自己读的东西负责"。这条值得留在心里，因为它会影响 forcing questions 应该长什么样：不是 quiz 式的考查，而是"让你不能假装读懂了"的那种逼问。

- 你说"也想公开出来，如果别人愿意用可以直接采用"。这是慷慨但不执着 —— 你不为别人做，但不介意别人受益。这种心态最适合做 vault 类工具：完全按自己的认知偏好调，反而能吸引到认知偏好相近的人。**不要为了"通用性"妥协你的偏好** —— 那会让工具对你和对别人都变平庸。
