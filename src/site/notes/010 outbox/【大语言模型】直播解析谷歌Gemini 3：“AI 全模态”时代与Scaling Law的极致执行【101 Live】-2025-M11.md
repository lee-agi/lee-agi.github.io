---
{"dg-publish":true,"permalink":"/010-outbox/gemini-3-ai-scaling-law-101-live-2025-m11/"}
---


## 封面
- 标题：直播解析谷歌Gemini 3：“AI 全模态”时代与Scaling Law的极致执行【101 Live】
- 链接：https://www.youtube.com/watch?v=1pgCnlJRuxE
- 发布日期：2025-11-21
- 总字数：3667
- 预估阅读时长：约 13 分钟
- 生成时间：2025-11-21 17:28:17
- 覆盖时长：01:15:04

## 摘要总结
- 本期围绕 Gemini 3 的实际体验、编码与多模态、Benchmark 可信度与产品落地，以及“超越 LLM Scaling Law”的研究趋势展开。多位嘉宾的共识是：Gemini 3 在多模态与推理上有实感提升，但真实场景和复杂任务仍存短板。
- 工程与算法并进成为关键路径：Google 以原生 multi-modality、MoE、深思（Deep Thinking/Tree-of-Thought）式的架构和“context engineering”能力，叠加完整生态，构成优势。**硬件经济学（TPU 软硬一体 vs GPU 高毛利）或决定谁能更“彻底”执行 scaling**。
- **公榜与真实产品表现存在偏差**：早期内测显示 Gemini 3 在 real-world vision 小基准下降、推理 token 变长但任务表现未必更好；多跳、检索聚合和科学写作等复杂场景仍略逊于最强模型。
- “Beyond LLM”方向：可解释性与第一性原理、数据效率（低 token 学习）、机器人学习与世界模型、以及新范式（非 Transformer、状态空间网络等）均被重视。**不要问模型它是如何工作——那会产生 hallucination；要看 paper、model card。**

## 全文

### 开场与议题铺陈（00:00:00 - 00:01:06）
**主持人**：今天想借机和几位前沿 AI 科学家讨论“除了 LLM 的其他分支”。开头先聊 Gemini，再聊 Beyond Gemini。在各社区“夸夸夸”的声量下，请先谈使用感受：它是否真如各类 ranking 霸榜？先从开发者视角的 Nathan 开始，再到田博士、余北和 David。

### 开发者体验：App/IDE/Slides 三板斧（00:01:07 - 00:05:44）
**Nathan**：
- 我这两天集中用了三个：1）Gemini 主 App（Model: Gemini 3 Pro）；2）面向开发者的 Google Anti-Gravity（Agentic IDE，像 Cursor/Claude Code 的竞品）；3）今天刚发的 Nano Banana Pro。
- 跑分我看了下 Benchmark，“SwitchBank”并非最高，最高是“Cloud Sony S4.5”（可能指 Claude Sonnet 3.5），但 Gemini 已非常接近。主 App 里直接“写前端小程序”，还能调用摄像头识表情、听声音反馈，利于新手快速上手。局限是偏前端，后端部署/调用其他 agentic framework 很难在主 App 完成。
- Anti-Gravity 最大不同是强调 AI Coding Agent 自主完成任务，UI 分为 Manager View 与 Editor/IDLE View。前者像你管理一组 agent（8~10 个），可分派任务、评审、跑 unit test。它还用到了 Gemini 3.0 Pro 的 Browser Use 能力，结合 Chrome 底层 API 做前端自动化测试；其屏幕理解的“Screenshot Pro”分数目前显著领先，开发/测试“一体化”很亮眼。但复杂浏览器操作（如上传文件）仍常需人工介入。
- Nano Banana Pro 的强项是复杂图像/Slides 生成。例如总结 Gemini 3.0 演进路线（1.0→1.5→2.0→2.5→3.0）和各自特性，现在能把逻辑链条理清楚。“Slides 赛道”的一些软件可能会被替代。

### 小说续写与创作性推理：质感变化（00:05:46 - 00:13:20）
**田博士**：
- 我习惯用“小说续写”作私人 benchmark：既防 overfitting，又能用我的 domain knowledge 看细微变化。Gemini 3 的“基模”比以前强是明显的突破。
- 回顾：1~2 年前模型写小说像“公文体”，风格脱戏；去年起开始 match 上原文本风格与人物连续性，但仍会“傻白甜地”写内心独白。之后 Claude 开始能处理配角互动，较有起色。
- 这次 Gemini 3 让我惊喜：它不仅写“人物+氛围”，还能知道用怎样的情节抓人心，甚至设置合理的反转，尊重人设又有创意。我第一次有“想存下来、用于自己的小说”的念头。Gemini 2.5 的长处是细致写景，但情节平铺；Gemini 3 在情节构造上更抓人。
- 技术上我判断不是靠某个特定后训练任务，而是基模更好，对小说的深层动机与展开更“理解”，能在续写中贯彻。
- Brainstorming 上，Gemini 3 并无特别突破。它像“刚入学的博士生”：知识广，但难做深层次创造，需要人引导路线；它仍是“做题家”。Coding 我只做了轻量测试，能跑出 3D 射击小 demo，但有小 bug（方向键反了）。优点是大大降低前端门槛。

### 全模态与“Deep Thinking”：多模态推理的内生化（00:13:30 - 00:20:31）
**Gavin**：
- 感叹 Google 大厂之力与完整生态。技术上最惊艳是 ARK AGI2（few-shot/meta learning）上的跃升：靠少量例子抽取 pattern 的“人类式智能”，Gen3 的分数从十几（或个位数）到三十几，说明其“human reasoning capability”增强。推测来自 multi-modality reasoning 的贡献。
- 数学推理方面的 Math Arena Apex 也有质变，类似 XLM/Symbolic 方向。
- “Screenshot Pro”让 OS/browser agent 一边“看屏幕”一边 language reasoning，省掉了把屏幕结构化为 JSON 再推理的链路。Gemini 3 是“model-native multi-modality”，不是去年那种“language + image/video/adapter 对齐”的老路，它把多模态数据一起混合预训练。
- 我和它讨论时，它提到“Tree-of-Thought”（非线性多分支思考）和在 model 内部“打分与阈值”丢弃/保留思路；在基模上再包一层 engineering wrapper（search/tree），这与“prompt→context→environment engineering”的演进一致。
- **工程与模型科学结合，突破纯纵向 scaling 的瓶颈：MoE 让 model horizontally scale，工程 feedback 让其 self-validate**。产品上“全家桶”把 IDE、前端“看着网页编程”的体验抬到新层级。我做过类似探索，Google 这样做我很认可。

### “Coding 之争结束了吗？”（00:20:31 - 00:22:29）
**田博士**：
- 现在很多 demo 是“一句话生成一堆代码”，看起来完整漂亮。但真写 code 看重“严密 follow 指令、细节修改”的能力。这个要更深入评测。有人反馈 Gemini 3 的 instruction following 不够强，需再观察。
**Gavin**：
- Benchmark 打得好不代表 coding（尤其后端/生产工程）拿下。现在模型更会前端，后端涉及更多 modalities（production log、code index 等）作为 signal，还没完全进来。很多 repository task 是琐碎的 corner case，robustness 未解。

### 真实世界表现、早期内测与公榜之限（00:22:37 - 00:30:10）
**余北**：
- 我最近忙，Gemini 3 亲试不多，收集了团队多组二手使用体验。优点：General QA 与 agentic/reasoning 的回答“简洁、到点”，2.5 时强 hallucination 的痛感缓解，推测 pre/post-training 都做得更好。
- 但 vision 组内部小基准测到 real-world visual understanding 下降；同时“reasoning 的长度”增加了 2~3 倍，但小基准表现反而下降。样本量小、方差大，不下定论。值得注意的是官方报告里涵盖 real-world 视觉理解的 benchmark 很有限。
- 引申：公榜未必反映产品实际能力。产品团队的内部 measurement 更贴近真实任务，优化方向不同会“走偏”。
- language 组反馈：多跳、外部搜索与信息聚合（如 20 年财报分析）上，Gemini 3 仍达不到 ChatGPT Pro（或 GPT-5.0）的表现。但 Gemini 3 很快，适合中等复杂度任务。
- scientific writing 感觉不如 2.5；coding 集成到 gymnasium（非 web coding）的体验也下降，很多可能是“没用对/未激活优势”的早期现象。总体看，这次 Google 在执行 scaling law 上很彻底。

### Google 优势在哪？预/后训练、Scaling Law 还没撞墙？（00:30:11 - 00:35:28）
**主持人**：这次如何理解 Google 的优势？生态？算法？多模态？负责人说“秘密是改进了预训练与后训练”，这意味着 scaling law 仍有优化空间？
**田博士**（00:31:47 - 00:33:56）：
- “预+后训练都改进”是工程真实话：数据、架构、训练稳定性、任务设计、RL 是否能 scale up 等都更好，最后模型自然更强。关键是“预训练变强”，后训练问题会更易解。也听过“修了以前的训练 bug”的传言，但无法证实。
**（抛砖引玉）嘉宾**（00:34:01 - 00:35:28）：
- 产品形态反过来“促进模型能力”。公榜好看后，orchestration layer 的 prompt 往往要重写一遍，否则新模型上的 agent 性能会“睡觉缠高”。我们更关心的是：基模提升有没有真正提升 KPI？这其实是 product strategy。
- 现在大多 agent 在“自动化既有事”，蛋糕没做大。需要 new business model/product shape/job positions，比如 web coding 推升“forward deploy engineer”这种“一人顶三人”的岗位。希望看到更多 killer app。

### 版本演进、Deep Thinking 与“context engineering”（00:36:34 - 00:40:22）
**Nathan**：
- Gemini 从 1.0 起就走“native multi-modality”路线：文字/图像/视频/音频在同一 embedding 系统里预训练；1.5 用上 MoE；2.0 加 agentic 改进；2.5 在 deepseek 之后很快跟进了“syncing/reasoning mode”；3.0 迭代“Deep Thinking/Decision Tree”思想——多路径尝试→反思→择优。
- 开发者 API 文档里还有“彩蛋”：context engineering is the way to go。使用上我常让模型先“学习写 viral X/Twitter 的 best practices”，再写具体内容。若模型能自动抓取 context 并融入思维链，效果会更好。这可能是下一轮训练方向。

### 警惕“问模型要原理”与原创性（00:40:23 - 00:41:18）
**田博士**：
- 不要问模型“自己怎么做的”，容易 hallucinate。要看论文与 model card。Google 的 thinking mode 并非“抄 deepseek”，在 2.0 就已有，团队（例如 Danny Zhou）很有原创性。

### 硬件经济学与 multi-modality 困难度（00:41:23 - 00:45:25）
**余北**：
- 这次 Gemini 迭代也反映：Google 能“纠正” Brain 时代的一些问题；执行 scaling law 更“彻底”。根底是硬件：**TPU 的软硬一体让 Google 有经济性把 scaling 做到极致；而 GPU 的高毛利会限制没有自研硬件的团队“坚定地”执行 scaling**。这对 Anthropic 等是挑战。
- **vision 的 scaling 没有 language 那么“听话”**，multi-modality 的难度更高。这也是挑战。顺便安利我的朋友 Brian（今年去 UCSF Biophysics 任教授），很 original，欢迎申请他的组。

### Beyond LLM：可解释性、第一性原理与研究提速（00:46:00 - 01:04:19）
**田博士**（00:47:36 - 00:55:28）：
- Scaling Law 实用且有效，但终有天“地球资源”不足以供给指数计算需求。从学术看，这代表“我们对世界结构一无所知，只能靠模型自己学”，这不令人满足。
- 我聚焦“神经网络如何产生涌现”：知识怎么表示、内部结构如何塑造智能。与其“机械可解释性”海量试验，我更喜欢从 first principles 推到可验证结论，用数学框架解释实验现象。
- 现在用 AI 工具（如 Cursor）做研究，效率提升“成百上千倍”。想法 20 分钟成型，3 分钟改好代码，立刻跑图看效果。工具进步会加速理论突破。也许有一天我们会找到“非梯度下降”的全新范式。
**余北**（01:00:17 - 01:04:19，节选并合并）：
- 我也坚持“白盒/可解释性”，需要“有信仰”的研究者长期投入。如今市场对 SSI、Thinking Machine Labs 等的估值，某种意义上也是“信仰的力量”。
- 应该寻找“另一条（组） law”，而非只押注 scaling law。比如：人类 13 岁前接触的 language token < 10B（可能更少），却能学到强智能；**数据效率**至关重要。
- Robotics 很像 AlexNet 初期：有点 work，又不够 robust、效率低，但正是研究机会。把“control with full model”的旧范式，过渡到“learning-native”的新范式，或许会像自然智能一样：越高级的智能越需要学习。
- 我希望机器能“重发明一种文明”（reinvent civilization），而不是只是“比我们好一点点”。

### 观众问答1：三到五年会换掉 Transformer 吗？（01:04:19 - 01:09:12）
**Gavin**（01:05:18 - 01:05:35）：
- 有竞争者：Mamba、Otherworld KV 等，思路是把记忆压缩为 states，用 O(1) 或 O(log n)/O(n) 的复杂度检索，内部也记录 hidden states，利于长期记忆，与 Transformer 的 trade-off 将增多。
**田博士**（01:06:07 - 01:06:48）：
- 这些已在混合使用（如 Sliding Window Attention + Full Attention，或“每三层插一层”），提升效率不损 long context 表现。完全不同于 Transformer 的新架构大规模落地暂少。竞争激烈时，大家会优先做“小改动的确定性收益”。
**余北**（01:06:52 - 01:09:00，节选）：
- 在“deep distill civilization”还远未完成前，优化架构未必是 leading labs 的最高 ROI。长期看，在逼近极限时，Transformer 不会是“all you need”。状态空间网络、diffusion policy 在物理/动作先验上有明显优势，演进空间很大。

### 观众问答2：生成式 UI 的看法（01:09:12 - 01:11:16）
**Nathan**：
- 生成式 UI 让交互体验和信息吸收更强。例如我把一篇上千字的经济分析帖，交给 Nano Banana 生成分步 Illustrator 式的图解，理解速度比读文字快很多。它适合快速“按需个性化”；但要“规模化复制同样体验”并不容易，通常得按用户需求动态重生 UI。

### 观众问答3：视频模型都在做自回归吗？以及 3D 表达路线（01:11:16 - 01:14:03）
**Gavin**：
- 现在大致三条路线：1）video-based（2D 表达 3D 世界，提供沉浸感）；2）mesh gen（有物理碰撞体积的 object）；3）Gaussian Splat（点云表达，适合游戏美术/UE）。
- 语言模型架构会趋稳（同时发展 SLM 在端侧），空间智能需与传感器/硬件结合，促进可解释性与“AI for everyone”。我强烈支持 open source，把“智能”从数据中心释放到端侧：过去我们为电力/算力买单，现在还得为智能买单；端侧开源可避免“数字集权”。

### 收尾（01:14:04 - 01:15:04）
**主持人**：感谢各位的前沿分享。世界模型很受关注，我们将做专期。欢迎订阅频道、留言，我们尽力再请嘉宾回来深入交流。

——

加粗摘选（部分非共识/高价值观点）：
- **Google 的“native multi-modality + Deep Thinking/ToT 内建 + context engineering”叠加生态，是其当前优势关键。**
- **硬件经济学决定 scaling 的可执行性：TPU 软硬一体 vs GPU 高毛利，影响谁能“彻底”执行 scaling law。**
- **公榜 ≠ 产品实效：real-world vision 内测下降、推理 token 变长但表现未必更好，需以内部 measurement 牵引优化。**
- **不要问模型“自己怎么做的”，要看论文和 model card——否则易被 hallucination 误导。**
- **Scaling law 不是唯一出路：低 token 学习、可解释性第一性原理、机器人学习/世界模型，可能孕育新范式。**

## 欢迎交流与合作
目前主要兴趣是探索agent的落地，想进一步交流可加微信（cleezhang），一些[自我介绍](https://lee-agi.github.io/85ed64eda0/)。

> 本文发表于 2025-11-22_周六。
