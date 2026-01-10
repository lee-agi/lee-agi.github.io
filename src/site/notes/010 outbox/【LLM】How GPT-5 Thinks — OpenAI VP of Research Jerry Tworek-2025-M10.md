---
{"dg-publish":true,"permalink":"/010-outbox/llm-how-gpt-5-thinks-open-ai-vp-of-research-jerry-tworek-2025-m10/"}
---


## 封面
- 标题：How GPT-5 Thinks — OpenAI VP of Research Jerry Tworek
- 链接：https://www.youtube.com/watch?v=RqWIvvv3SnQ
- 发布日期：2025-10-16
- 总字数：4075
- 预估阅读时长：约 14 分钟
- 生成时间：2026-01-10 18:52:09
- 覆盖时长：01:16:03

## 摘要
- Jerry 将 O3 形容为一次“tectonic shift（构造断层式跃迁）”，并称 ==ChatGPT-5 在某种意义上可看作 O3.1==，但他期待的是下一次更大的跃升。
- 他将 reasoning（推理）界定为“从不知道答案到找到答案的过程”，强调“花更长时间思考通常会让答案更好”，并用 chain-of-thought（CoT）说明“step by step”如何把“thinking”显性化。
- OpenAI 在产品中平衡==“质量-时延”的取舍==，允许“high reasoning / low reasoning”作为同一模型的不同“思考时长”参数；这本质上是用户体验的权衡。
- GPT-4 训练完成时，团队内部一度“并不惊艳”；结合 RLHF（用 PPO + 人类偏好奖励）后，才交付了大家看到的“GPT 时刻”。
- OpenAI 研究文化强调“少数大赌注”和高度协作，研究内部“几乎人人都知道一切”。Jerry 强调“pre-training + RL”是必要的双元范式：==没有 pre-training 的“pure RL”不成立，而没有 RL 的 pre-trained 模型也难以达到目标==。（RL包括Human Feedback和Env Feedback）

## 原始全文
### 开场与宏观判断：O3、ChatGPT-5、以及“唯一一次的历史时刻”（00:00:00 - 00:00:32）
**Jared Faraic（Jerry）**：坦白说，它当时主要还是擅长解谜，更像是一个技术演示。O3 确实像是在 AI 轨迹上的一次“tectonic shift（构造断层式跃迁）”。ChatGPT-5 在某种意义上可以被认为是 O3.1，但我追求的是下一次相当显著的跃升。我们明白，历史上只有这一次 AI 被构建、部署和发展的时刻；我们一起在追求一个大于我们每个人的目标。

### 节目介绍与开场寒暄（00:00:32 - 00:01:05）
**Matt Turk**：我是 Matt Turk，欢迎来到 MAD podcast。今天的嘉宾是 OpenAI 的 VP of Research，Jared Faraic，他也是 MADIS list 的全球顶尖 AI 研究者之一。在这一期里，我会和你深聊模型如何实际进行 reasoning（推理）；也会聊 OpenAI 幕后：少数大赌注如何组队、为什么几乎“人人知道一切”、以及这种文化如何快速“ship”。请大家享受和 Jerry 的对话。嘿，Jerry，欢迎。
**Jared Faraic（Jerry）**：你好，很高兴来到这里。
**Matt Turk**：这次我们会重点聊 reasoning。

### 什么是 reasoning（推理）（00:01:06 - 00:02:31）
**Matt Turk**：高层上，reasoning 到底是什么意思？当我们和 ChatGPT 对话、它说自己在“thinking（思考）”时，幕后到底发生了什么？
**Jared Faraic（Jerry）**：我觉得“思考过程”是个不错的类比。早期我们就梦想教模型会 reasoning：像人一样在难题前会花时间找答案，有时要计算、有时要查资料、有时甚至要先教会自己。reasoning 是走向“我还不知道的答案”的过程，某种意义上可以叫“search（搜索）”，但不是那种天真的搜索。差别在于：answering a question（回答问题）通常是我已经知道答案，只是把它提取出来；而 reasoning 是在不知道答案的情况下，一步步把它找出来，且通常花的时间越长、结果越好。
**加粗要点**：**“reasoning 是从‘不知道’到‘找到’答案，且思考时间越长通常越好。”**

### chain-of-thought（CoT）如何工作（00:02:31 - 00:05:23）
**Matt Turk**：自从你们在 2024 年 9 月发布 O1（ASR 原文作 0.1）后，大家熟悉了 chain-of-thought（CoT），也就是 ChatGPT 展示“解题过程”的那些小段落。它到底在做什么？是不是逻辑树、在逐步排除选项？
**Jared Faraic（Jerry）**：语言模型本质上常被称为“next token prediction（下一个 token 预测）”机器（在 RL 时代这定义不再完全准确，但它们仍主要在 token/文本上运作，尽管如今也 multi-modal）。简化说，语言模型会生成文本；而 chain-of-thought 是把“thinking（思考）”过程用人类词汇与概念“口述”出来。模型在互联网上大规模看到人类知识与思维过程后，就部分学会了“像人类那样思考与求解”。CoT 是把这种能力引导出来。早期 CoT 常解决数学谜题，最著名的 prompt 是“let’s solve it step by step（我们一步步解）”。当直接问模型数学表达式或谜题，它往往会失败，因为无法“一步（一个 token）到位”；但如果让我“step by step”，我会先写下第一步、再第二步，串联起来后给出答案。
**加粗要点**：**“chain-of-thought 是把求解步骤显性地‘写’出来，像人类在纸上解题那样 step by step。”**

### 思考时长的权衡与用户体验（00:05:23 - 00:07:20）
**Matt Turk**：既然“思考时间”对 reasoning 如此重要，在 ChatGPT-5 的 auto mode 里，模型如何决定“想多久”？
**Jared Faraic（Jerry）**：这属于我们的优化：一方面长思考可带来更好结果——我们在 O1 也展示了这类 scaling law；另一方面用户不喜欢等待。俗话说==“cheap、fast、good 只能三选二”，在语言模型上也成立==，这是微妙权衡。所以我们暴露出“high reasoning / low reasoning”作为同一模型的不同参数：让你思考更久或更短。我们编码了一些启发式来猜测用户在不同情境下希望的“思考时长”。
**Matt Turk**：所以更多还是用户体验层面的选择？
**Jared Faraic（Jerry）**：最终确实如此。你总能等更久拿到更好答案，但问题是你愿不愿意等。
**加粗要点**：**“在质量与延迟之间做取舍，同一模型通过参数决定‘想多久’。”**

### 从 O1 到 O3 到 ChatGPT-5：一年间 reasoning 的演进（00:07:20 - 00:07:59）
**Matt Turk**：==从 O1 到 O3 再到最近的 ChatGPT-5==（主要是RL的scale，和Pretrain关系不大），你会如何刻画这三者的 reasoning 演进？
**Jared Faraic（Jerry）**：我会把它看作一系列“scale-up runs（放大实验）”，本质上在扩展我们的 reinforcement learning（RL）研究计划与能力。

### 研究主管的一天（00:23:20 - 00:24:02）
**Jared Faraic（Jerry）**：==送完孩子上学后，我一天大部分时间都在和研究者们交流==：对想法做“来回碰撞”，和一个同伴 brainstorm 完，再换另一个；也会有 group meetings。主题从人到人、从会到会不断变化，但核心就是持续把研究计划打磨得更好。

### 研究优先级：自上而下 vs 自下而上（00:24:02 - 00:26:51）
**Matt Turk**：研究优先级如何定？是 top-down 还是 bottom-up？
**Jared Faraic（Jerry）**：我们在“构造与领导研究项目”的“术”上花了很多功夫。不是纯 top-down，也不是纯 bottom-up，而是平衡。OpenAI 的一个原则是“只做很少的项目”，不是做大而全的“portfolio”，而是“少数核心大赌注”，把==大量人力集中到少数极具雄心的项目上（大约 3-4 个）。这意味着大家不是随心所欲想做啥就做啥，而是要面向这些项目的共同目标；在项目之内我们尽量 bottom-up。研究 lead 的关键工作是让研究者们协同朝向同一个共享目标，不要各自分裂==（top-down定核心项目，项目内bottom-up）。完全 top-down 在研究组织里行不通——你不可能雇来最聪明的人然后只告诉他们要做啥；但他们的探索也要在“项目需要”的空间里，去寻找对 OpenAI 研究目标“边际贡献最大”的工作。
**加粗要点**：**“少数大赌注 + 在项目边界内的 bottom-up 创新，是我们组织研究的方式。”**

### 团队协作与内部透明（00:26:51 - 00:29:17）
**Matt Turk**：多项目并行下，协作与 IP 保护如何平衡？
**Jared Faraic（Jerry）**：可能会让你意外——在 OpenAI 的研究团队里（现在略少于 600 人），几乎“人人都知道一切”。完全透明能让每位研究者用最好信息做最好工作。当然这有 IP 风险，但我认为“因信息不全而做错”的风险更大。整体文化非常协作，人性的小摩擦难免，但我们有很强的“共同命运感”：这是正和博弈，OpenAI 的成功并不保证，得靠我们每天做出色工作。
**加粗要点**：**“==everyone knows everything==（人人都知道一切）”与“极高的内部透明度”是研究团队的运行原则。**

### 快速迭代的文化与动因（00:29:17 - 00:31:34）
**Matt Turk**：你们如何保持如此高频的发布节奏？
**Jared Faraic（Jerry）**：我把 OpenAI 看作“时代型公司”：有动能、有最聪明的人才愿意来，单人产出极高；组织方式上也借鉴了 ==Silicon Valley 的“how to get things done quickly（如何快速把事做成）”==。大家对工作既兴奋又有“历史重量感”，所以普遍非常努力。我们也时刻记得：==**“历史上只有一次 AI 被构建与部署的时刻”**==，大家都想把它做对、做到最好。

### 内部自用工具：CodeX 与 ChatGPT（00:31:34 - 00:32:35）
**Matt Turk**：你们是否大量使用自家工具？Simo 之前还发过推，说 Dev Day 上很多内容由 CodeX 生成。
**Jared Faraic（Jerry）**：我们确实大量用 CodeX 写代码。我自己也重度使用 ChatGPT，不过不是用来“想点子”，而是回答我许多问题——我现在乐意每月为它付 200 美元，因为配额更慷慨不容易被限流。
**Matt Turk**：他们还让你付费？
**Jared Faraic（Jerry）**：是的，我也觉得挺合理。

### 现代训练范式：pre-training + RL（00:32:35 - 00:35:02）
**Matt Turk**：今天（2025 年 10 月）的 OpenAI 系统是不是“pre-training + RL”的组合？
**Jared Faraic（Jerry）**：可以这么理解：先 pre-train，再做 reinforcement learning。没有 pre-training，RL 不会奏效；同样没有 RL，很多 pre-trained 模型的局限也难以突破。算法与架构会演化，但组合思路会在相当时间内共存。我想致敬 Ilya 的前瞻性：我 2019 年初进 OpenAI 时，他在研究全员会上描述研究计划：“train large generative model on all data we can, then do reinforcement learning on it（把能拿到的全部数据训练成大规模生成模型，然后再做 RL）。”算法与架构后来变了，但这条主线，一直没变。
**加粗要点**：**“pre-training + RL”是 OpenAI 自 2019 年即确立的研究主线。**

### RL 101：狗训练类比、policy、environment（00:35:12 - 00:39:54）
**Matt Turk**：给 10 岁小朋友解释一下 RL？
**Jared Faraic（Jerry）**：就像训练狗：口袋里要有“treat（零食）”。看到好行为就奖励，不好的就“撤回注意力”作为负反馈。我们对模型也做类似的事：放到具有挑战的环境里，好的行为给“cookie（奖励）”，不好的给“惩罚（负奖励）”。好的 RL 会平衡好奖惩（这背后有数学性）。pre-training 训练的是“next token prediction（下一个 token 预测）”；而 RL 学到的是“行为”。在术语上，policy 是“从 observation（观测）到 action（动作）的函数”，本质上模型权重决定它在不同输入下的行为；agent 是模型；reward 衡量好坏；environment（环境）是模型所见的一切，关键是它要“可交互”：世界会对你的动作作出反应。就像学吉他，你拨动琴弦，听到反馈，再调整。要教会 agent 对环境变化做出合理反应，**RL 是关键路径**。
**加粗要点**：**“policy 是把‘看到的东西’映射为‘做出的动作’的函数；RL 让模型学‘行为’，不是只学‘下一个 token’。”**

### RL 的演化：从 deep RL 到与 LLM 结合（00:39:54 - 00:45:28）
**Jared Faraic（Jerry）**：主要的“地壳运动”发生在把 neural networks 与 RL 结合，也就是 deep reinforcement learning（deep RL）。这是 DeepMind 在 DQN（Deep Q-Learning，ASR 原文作 DQL）时刻带来的突破。随后 RL 在“游戏”上很活跃，但当时没有 pre-training，模型虽被强化学到很多“行为”，却并不“聪明”（我不想说“穴居人智能”，但有那种感觉）。这其实是**没有 pre-training 的 RL 的死胡同**。GPT 时代的大规模 pre-training 让“在 LLM 上做 RL”成为可能。我一拿到 GPT-3 就开始做 RL，但系统很笨重、算法与问题设定都在摸索。一开始我们“cargo-cult（照搬）”了游戏/机器人那套（比如 PPO）到 LLM 上，效果不算惊艳。
训练 GPT-4 时，很多人现在觉得它很强，但我们内部起初并不惊艳：==在“一句话评测（one-token eval）”上它看起来挺聪明，一旦让它说得更长，就不连贯。我们问：如何让一个看起来“有点聪明”的模型，在长文本里也“真正聪明”？答案是 RLHF==（reinforcement learning from human feedback）：用 PPO 在大语言模型上做 RL，reward 来自“人类偏好”——比如比较两段文本、做“thumbs up / thumbs down”。
**加粗要点**：**“没有 pre-training 的 RL 是死胡同；‘pre-training + RL（尤其 RLHF）’才点亮了 GPT-4 的真实能力。”**

### RLHF 与 GPT-4 的转折（00:45:37 - 00:47:01）
**Matt Turk**：产品里“点赞/点踩”只是 UI，真正的 RLHF 是在训练后（post-training）吗？需要很多人类标注者吗？
**Jared Faraic（Jerry）**：是的，post-training。RLHF 是个持续多年的研究计划，我们在 GPT-2 时就做过。关键是构造对“奖励模型（reward model）”有用的数据：现在叫“AI trainers”的人会看模型输出、给分，我们再“学一个能预测这些分的模型（reward model）”，然后用它来训练策略。

### 数据标注产业的演进（00:47:01 - 00:47:26）
**Matt Turk**：这和数据标注产业（比如 Scale AI 等）关系密切吧？
**Jared Faraic（Jerry）**：是的。不过随着模型越来越聪明，这部分在逐渐退居二线：**前沿会不断前移**，AI 已能做的就不该再让人标，标注产业需要不断“自我重塑”。

### 未尽之问：pre-training 是否 unsupervised（片段）（00:47:51 - 00:48:00）
**Matt Turk**：我们聊了很多 RL，但第一阶段的 pre-training 是不是 unsupervised learning（无监督学习）……（问题未完待续）

### 迈向 AGI：必要要素与自我改进的阈值（01:10:00 - 01:12:19）
**Jared Faraic（Jerry）**：我认为我们今天做的 pre-training 是必要的，我们今天做的 RL 也是必要的，将来还会有更多要素加入。有人会说“这些离目标很近/很远”，我不太想做标签之争；关键是我们会持续把训练方式变得更像“我们认为正确的 intelligence（智能）”与“最有用的学习形式”。如果把今天的 ChatGPT 给 10 年前的人看，他们可能已经叫它 AGI，但我们知道它还有很多限制，而且我们有把握能逐步解决。最大的难题是：**何时模型可以在较少外部输入下“自我改进”？**一旦进入那个阶段，它对能做什么的预测就会变得更模糊。

### 关于 Richard Sutton“pure RL”路径的回应（01:12:19 - 01:15:05）
**Matt Turk**：Richard Sutton 最近在 Dworkin/Dworkish（节目名 ASR 可能有误）里说“通向 AGI 的唯一道路是 pure RL；LLM 是对现实的模仿，而 RL 才是对现实的‘强制’”。你怎么看？
**Jared Faraic（Jerry）**：我还没完整听完那期，但就我看来，“pure RL”不太成立：**RL 需要 pre-training，pre-training 也需要 RL**。OpenAI（以及我相信其他实验室）都在很认真地对 LLM 做大规模 RL。有些人争论“LLM 是 on-ramp 还是 off-ramp”，往往指的其实是 pre-training。但也很清楚：==我们当前做事的方式还不够，未来会继续加东西、也会替换掉不再需要的旧部分，架构可能会更显著地变化。我的看法是：我们走在正确的路径上，这更像是“持续增配/换挡”，而不是 180° 调头==。
**加粗要点**：**“pure RL 不现实：‘pre-training + RL’是互为必要的双支柱。”**

### 收尾（01:15:20 - 01:16:03）
**Matt Turk**：这是一段很棒的对话，感谢你揭示了 OpenAI 幕后、pre-training 与 scaling RL 的要点。Jerry，谢谢！
**Jared Faraic（Jerry）**：谢谢，我很享受这次对话。
**Matt Turk**：感谢收听 MAD podcast。如果喜欢，欢迎订阅、点评，这能帮助我们做出更好的节目并邀请到更好的嘉宾。我们下期见。

## 欢迎交流与合作
目前主要兴趣是探索agent的落地，想进一步交流可加微信（cleezhang），一些[自我介绍](https://lee-agi.github.io/85ed64eda0/)。

> 本文发表于 2026-01-11_周日。
