---
{"dg-publish":true,"permalink":"/010-outbox/ai-state-of-ai-open-router-2025-m12/"}
---


## 封面
- 标题：State of AI | OpenRouterOpenRouterDiscordGitHubLinkedInX
- 链接：https://openrouter.ai/state-of-ai
- 发布日期：未知日期
- 总字数：8857
- 预估阅读时长：约 30 分钟
- 生成时间：2025-12-13 12:01:55
- 覆盖时长：00:03:22

## 总结
- 基于 OpenRouter 的超大规模使用数据（100 trillion tokens），作者刻画了过去一年 LLM 使用从单轮生成向多步“agentic inference”跃迁：推理型模型、tool-calling 和长上下文序列全面上升，==编程类工作负载成为主导==。
- ==开源（OSS）稳步增长至约三成总量==，且中国开源模型（DeepSeek、Qwen、MoonshotAI 等）崛起显著，在编程、翻译和多语等领域强势；同时，创意 ==Roleplay 与代码助理构成开源使用的双支柱==。
- 使用分布跨地区差异明显：==亚洲份额从约13%升至约31%==；语言上英语占80%+，简体中文近5%。
- 成本—使用关系呈弱价格弹性特征：==便宜≠高用量==，闭源保有高价值场景定价权，开源承载成本敏感的大体量任务，==呈现分层分段的多模型市场==。
- 留存分析提出“Cinderella Glass Slipper effect”：首个真正解决关键工作负载的“前沿”模型能锁定“基础 cohort”，形成长期粘性；同时观察到 DeepSeek 的“boomerang effect”回流现象。

## 要点
- 数据与方法（Data and Methodology）
  - 使用 OpenRouter 匿名元数据，覆盖数十亿 prompt–completion，合计超过 100 trillion tokens；不接触原始文本，仅用结构化元数据进行行为分析。
  - 类目标注源于 OpenRouter 内部抽样约 0.25% 的请求，通过 GoogleTagClassifier（Google Cloud Natural Language classifyText API）返回层级标签；低于 0.5 置信度的标签被剔除。
  - 将细粒度标签映射为研究用的精简标签（Programming、Roleplay、Translation、General Q&A/Knowledge、Productivity/Writing、Education、Literature/Creative Writing、Adult、Others）。
  - 地理基于账单所在地（billing geography）而非 IP；时间窗为滚动 13 个月（至 2025-11），类别分析起于 2025-05（因标注上线时间）。
  - Token 定义区分 prompt vs. completion；reasoning tokens 计入 completion；时间序列以周度 UTC 聚合。
  - 限制性：平台视角的观察性研究，部分代理指标（如 agentic 推断、地域）存在误差范围。

- 开源 vs 闭源（Open vs. Closed Source Models）
  - 闭源仍承载多数 token，但 OSS 稳步增长，至 2025 年末约达三成；关键开源发布（DeepSeek V3、Kimi K2、GPT-OSS）引发持久增长而非昙花一现。
  - 中国开源模型的份额从可忽略上升到==部分周接近 30%==，全年周均约 13%；与 RoW OSS（13.7%）相比，==闭源 RoW 仍最大（约 70%）==。这由高频迭代与密集发布驱动。
  - 开源市场从 DeepSeek 高度集中转为多元化竞争：到 ==2025 年末，无单一模型占 OSS token 超 25%，5–7 个模型持有可观份额，呈碎片化格局==。
  - 模型尺寸结构演进：==小模型（<15B）整体使用份额下滑；中模型（15B–70B）自 Qwen2.5 Coder 32B 起“找到 model–market fit”；大模型（≥70B）呈“多极化”==，用户主动对比多款开源大模型。

- 类别与使用结构（Categories）
  - 开源使用以 Roleplay 与 Programming 为双核：==Roleplay 超过 50% 的 OSS token==，Programming 居次。
  - 中国 OSS 的类别结构更偏技术：Roleplay ≈33%，Programming+Technology ≈39% 并成多数，反映 Qwen、DeepSeek 等在代码与基础设施类任务上提升。
  - Roleplay 类别在 2025 年内从早期闭源主导（≈70%）==转向开源与闭源几乎均分（RoW OSS ≈43%，Closed ≈42%）==，显示创意对话领域竞争加剧。
  - 全体模型视角，==Programming 在 2025 年稳步上升至近期超过 50% 的总 token 份额；细分供给格局中，Anthropic 的 Claude 系列长期占比 >60% 编程支出，但在 2025-11 首次跌破 60%，OpenAI 自 2% 升至约 8%，Google 稳定在约 15%==，MiniMax 等开源新锐上升明显。
  - 内部结构：==Roleplay 近 60% 流量为 Games/Roleplaying Games，其次 Writers Resources 与 Adult==；Programming 中“Programming/Other”占比高，Development Tools（26.4%）显示结构化工作流尚在成形。翻译、科学等类目的内部结构较平坦，而健康、法律、金融更分散。

- Agentic Inference 的崛起（The Rise of Agentic Inference）
  - 2025 年推理导向模型的 token 份额快速上升，现已超过 50%；供给端（GPT-5、Claude 4.5、Gemini 3）与需求端（偏好多步逻辑、状态管理与 agent 工作流）双向驱动。
  - 领先推理流量模型轮动加快：近期 ==xAI 的 Grok Code Fast 1== 超越 Gemini 2.5 Pro；开放模型如 gpt-oss-120b 仍被大量调用。
  - Tool-calling 使用向上趋势明显（除去 2025 年 5 月的大户异常峰值）；支持可靠工具协议的模型更易获企业采用。
  - ==平均 prompt token 从约 1.5K 升至 6K+，completion 从约 150 升至 400==；平均序列长度 20 个月内从 <2K 升至 >5.4K。
  - 编程请求主导了长上下文增长，常超过 20K 输入 token；长序列是高复杂度、嵌入式 agent 工作流的“签名”。
  - 运营含义：默认能力门槛提高，==延迟、工具处理、上下文与鲁棒性成为关键==；推断平台需支撑长会话、执行轨迹与敏感权限集成。
  - 强判断：**“agentic inference”将超过（若未已超过）人类推断份额**。

- 地理与语言（Geography and Languages）
  - ==北美仍最大但多数时期已<50%==；==欧洲稳定贡献（21%）；亚洲份额由≈13% 翻倍至≈31%==，既是供给者亦是快速增长的消费者。
  - 英语占 80%+ token；简体中文≈5%；随中国开源模型崛起，多语支持的重要性上升。

- 留存与“Cinderella Glass Slipper effect”（Retention）
  - 留存度量为“活动留存”（activity retention），允许回归后再计入，曲线可能小幅非单调。
  - 观察到“基础 cohort”：早期用户黏着度显著高于后续 cohort，表征模型首次真正解决某类高价值工作负载。
  - **提出“Cinderella Glass Slipper effect”**：当新“前沿”模型与先前未解的高价值工作负载在技术/成本上精确契合，即形成持久留存与锁定。
  - 实证：Gemini 2.5 Pro（2025-06 cohort）与 Claude 4 Sonnet（2025-05 cohort）在第 5 个月仍≈40% 留存。
  - 反例与异象：GPT-4o Mini 的 2024-07 单一“基础 cohort”一枝独秀，后续 cohort 皆快速流失；Gemini 2.0 Flash、Llama 4 Maverick 未建立“基础 cohort”，显示未被视作“前沿”；DeepSeek 观测到**“boomerang effect”**（回流），如 DeepSeek R1（2025-04 cohort，约第 3 月）等。
  - 结论：留存曲线可作为能力拐点与“先解者”（first-to-solve）优势的经验信号；“前沿窗口”短暂而决定性。

- 成本与使用动态（Cost vs. Usage Dynamics）
  - 类别层面 log–log 分布，以 $0.73/1M tokens 为中位数垂线划分四象限：Premium Workloads（高成本高用量，如 Technology 与 Science）、Mass-Market Volume Drivers（高用量低/中成本，如 Programming 与 Roleplay 与 Science）、Specialized Experts（高成本低用量，如 Finance/Academia/Health/Marketing）、Niche Utilities（低成本低用量，如 Translation/Legal/Trivia）。
  - **Technology 是显著离群点：单位成本最高且使用量高**，可能由极高价值或极高供给成本驱动（或兼而有之）。
  - 模型层面呈弱价格弹性：==价格每降 10%，使用量仅增约 0.5–0.7%==；**闭源（OpenAI、Anthropic）占高成本高用量象限，开源（DeepSeek、Mistral、Qwen）占低成本高用量象限。**
  - 四类作者 archetypes：Premium leaders（如 Claude Sonnet 3.7/4，≈$2/1M 仍高使用）、Efficient giants（如 Gemini 2.0 Flash、DeepSeek V3 0324，<$0.40/1M 且高使用）、Long tail models（几美分/1M 但使用低）、Premium specialists（如 GPT-4/GPT-5 Pro，≈$35/1M 低用量高价值）。
  - 观察：宏观看需求不弹性，但微观分层显著；有**Jevons paradox**迹象（降价带来更大量总消耗）；==质量/可靠性常胜于成本==；单“便宜”不足以驱动深集成。
  - 策略：分层产品（Flash vs Pro）按任务路由；聚焦“每次成功的成本”（cost-per-successful-outcome）胜过“每 token 成本”；市场未被商品化，差异化（延迟、上下文、稳定性）仍是优势来源。

- ==供给侧画像（Provider Profiles）==
  - ==Anthropic：Programming+Technology 超 80%，定位复杂推理、编码与结构化任务。==
  - ==Google：用途更分散，Translation、Science、Technology、General Knowledge 比重较高，编程占比在 2025 年末降至≈18%。==
  - ==xAI：长期 Programming >80%；至 11 月后分布拓宽，与免费分发引入的非开发者流量相关。==
  - ==OpenAI：2025 年初 Science >50%，年末降至 <15%；Programming 与 Technology 各≈29%，更贴近面向结构化高价值任务。==
  - ==DeepSeek：以 Roleplay、休闲聊天、娱乐为主，占比常 >2/3；编程占比夏末缓升。==
  - ==Qwen：Programming 长期 40–60%，周波动更高，9–10 月 Roleplay 有一波上升，11 月回落。==

- 讨论与结论
  - 多模型生态将长期存在，无单一模型通吃；选择受能力、延迟、成本与信任多轴影响。
  - 使用多样性超越“生产力”范式：**Roleplay 与娱乐类体量接近或可与专业类并行**，对评测与产品形态提出新要求（连续对话、一致性、情绪）。
  - **Agentic inference** 使评估从语言质量转向“任务完成与效率”。
  - 全球化与本地优化并行：多语与文化适应性将成为竞争关键。
  - 留存即“能力突破”的指纹；抓住“前沿窗口”的 first-to-solve 会沉淀“基础 cohort”。
  - 局限：平台单点视角、时间窗有限、使用代理指标；结论应视为行为模式而非最终因果。

## 译文（为可读性去除时间戳，不改动事实与要点；关键表达保留英文或中英并列；加粗处为本文认为更重要/见解性/非共识的内容）
菜单项：Status / Announcements / Docs / Support / About / Partners / Enterprise / Careers / Pricing / Privacy / Terms

标题：An Empirical 100 Trillion Token Study with OpenRouter

引言（Introduction）
过去一年标志着大型语言模型（LLMs）在演进与真实使用上的转折点。随着 2024-12-05 首个被广泛采用的 reasoning model“o1”（代号 Strawberry）的发布，领域从 single-pass pattern generation 转向 multi-step deliberation inference，加速了部署、实验与新应用类型的涌现。随着这一转变快速展开，我们对模型“真实如何被使用”的经验理解却相对滞后。本文利用 OpenRouter 这一跨多种 LLM 的推断平台，分析覆盖不同任务、地域与时间的超过 100 trillion tokens 的真实交互。我们观察到：开源权重（open-weight）模型的实质性采用；创意 roleplay（不仅是生产力任务）与 coding assistance 的超高热度；以及 agentic inference 的崛起。此外，我们的留存分析识别到“基础 cohort”：早期用户的参与度显著更持久，我们称之为“Cinderella Glass Slipper effect”。这些发现强调：开发者与终端用户在“野外”与 LLM 的交互多元而复杂。我们讨论其对模型构建者、AI 开发者与基础设施提供商的意义，并阐明基于数据的使用理解如何指导更优的 LLM 系统设计与部署。

仅一年前，LLM 版图仍然不同。在 2024 年末之前，最先进系统仍以单次自回归续写为主。多种先驱尝试通过指令增强与工具使用“近似推理”：例如 Anthropic 的 Sonnet 2.1 & 3 在复杂 tool use 与 RAG 表现突出；Cohere 的 Command R 引入结构化 tool-planning tokens；开源项目如 Reflection 在训练中探索 supervised chain-of-thought 与 self-critique。虽然这些技术产生类似推理的输出与更强的指令跟随，核心推断过程仍是单次前向，更多是在再现数据中学到的“表层轨迹”，而非进行内部迭代计算。

这一范式在 2024-12-05 发生变化：OpenAI 发布 o1 reasoning model（代号 Strawberry）完整版（预览版 2024-09-12 已显露不同于传统自回归推断的迹象）。不同于既往系统，o1 在推断时加入扩展的内部多步审议（multi-step deliberation）、潜在规划与迭代改写，最终再生成输出。实证上，这带来数学推理、逻辑一致性与多步决策的系统改进，体现从“模式补全”向“结构化内部认知”的转变。回看去年，那是实质的拐点：此前的方法在“描述推理”，而 o1 是首个在广泛部署中“通过刻意的多阶段计算真正执行推理”的架构。

尽管 LLM 能力进展已广泛报道，“模型在实践中如何被使用”的系统证据仍有限。现有叙述多强调定性演示或基准成绩，而非大规模行为数据。为弥合这一缺口，本文基于 OpenRouter（多模型 AI 推断平台）的 100 trillion token 数据集，开展 LLM 使用的实证研究。

OpenRouter 提供独特窗口：它编排横跨闭源 API 与开源权重部署的多样模型请求，捕捉开发者与终端用户在不同任务的真实调用。我们据此分析模型—任务匹配、地理与时间的使用变化，以及价格或新模型上线等外因对行为的影响。

我们参考此前关于 AI 采用的实证研究（如 Anthropic 的经济影响与使用分析、OpenAI 的 How People Use ChatGPT），力求中立与数据驱动。本文先描述数据与方法（任务与模型如何分类），再展开多维分析：
- Open vs. Closed Source Models：比较开源模型与专有模型的采用格局，识别开源生态中的关键参与者与趋势；
- Agentic Inference：捕捉多步、工具辅助推断的兴起，体现模型作为更大自动化系统组件的增长；
- Category Taxonomy：按任务类别（编程、roleplay、翻译等）拆解，揭示驱动使用的应用域及不同提供商的差异；
- Geography：比较全球与美国国内的使用差异；
- Effective Cost vs Usage Dynamics：评估使用与“有效成本”的对应（考虑输入输出 token、缓存等）；
- Retention Patterns：刻画头部模型的长期留存，识别定义持续行为的基础 cohort，即“Cinderella Glass Slipper effect”。

数据与方法（Data and Methodology）
分析基于 OpenRouter 的统一 AI 推断层的匿名元数据，覆盖全球用户数十亿对话事件，重点关注最近一年。我们无权访问提示或输出文本，只使用结构、时序与上下文等元数据，保护隐私同时支持大规模行为分析。每条记录包含时间、模型/提供商标识、token 使用、系统性能、地理路由、延迟、上下文（如是否流式、是否取消、是否触发 tool-calling）等；token 计数含 prompt 与 completion。

可视化与分析在 Hex 平台完成，形成可复现的 SQL/转换/制图流程。需强调，这是一个观察性数据集，反映 OpenRouter 平台的真实活动；该平台受模型可用性、定价与用户偏好影响。至 2025 年，OpenRouter 支持 60+ 提供商的 300+ 活跃模型，服务数百万开发者/终端用户，50%+ 来自美国以外。尽管平台外的行为未被覆盖，其全球规模与多样性仍具代表性。

我们不接触用户提示或输出；OpenRouter 仅对约 0.25% 请求的提示/响应做内部分类（GoogleTagClassifier），以 Google Cloud Natural Language 的 classifyText 提供层级标签（如 /Computers & Electronics/Programming），置信度∈[0,1]，低于默认阈值 0.5 的类别不纳入。分类在 OpenRouter 内部完成，不关联具体客户，研究仅使用分类结果元数据。

为规模化使用这些细粒度标签，我们映射到研究自定义的精简桶：Programming、Roleplay、Translation、General Q&A/Knowledge、Productivity/Writing、Education、Literature/Creative Writing、Adult、Others（多数分析中省略 Others）。多标签情况存在，但整体可用于量化“用途而非仅用量”。

若干显式变体：
- Open Source vs. Proprietary：权重公开者标为 OSS（开源），仅限 API 访问者标为闭源（如 Anthropic 的 Claude）。
- Origin（Chinese vs. Rest-of-World）：按主要研发地标注，如中国（含中国大陆、台湾、香港）的 Qwen、Kimi、DeepSeek 等。
- Prompt vs. Completion Tokens：prompt 表示输入，completion 表示输出；总 token 为两者之和；reasoning tokens（推理步骤）算在 completion 内。
- Token volume 如无特别说明，为 prompt+completion 总和。
- 地理：用账单地址（billing）作为地域代理；注意企业/共享账户可能带来偏差。
- 时间窗：大多分析覆盖 2024-11-03 至 2025-11-30；类别分析始于 2025-05（标签稳定上线）；周度 UTC 聚合。

开源与闭源（Open vs. Closed Source Models）
过去一年，闭源（尤其北美大厂）仍占多数 token，但 OSS 稳步增长，到 2025 年末约占三分之一。OSS 使用峰值与重大发布（DeepSeek V3、Kimi K2、GPT-OSS）一致，且峰后留存，显示是真实生产使用而非短期试用。

中国研发的模型贡献显著：自 2024 年末的可忽略基数起步（周度低至 1.2%），稳步增长，部分周接近 30%，全年周均约 13.0%；对比 RoW OSS 为 13.7%，RoW 专有模型均值仍最高（70%）。这反映质量竞争力与快速迭代、密集发布周期（如 Qwen、DeepSeek 的多轮版本）对市场的重塑。

趋势显示一个稳定的“双结构”：专有系统定义企业与受监管工作负载的可靠性/性能上限；OSS 更具成本效率、透明与可定制，在合适工作负载上有吸引力；生态趋于在约 30% 平衡共存，并在多模型栈中互补。

按 token 量排序的模型家族，DeepSeek 仍是最大 OSS 贡献者，但主导性被稀释，多家开源家族各自维持显著份额。周度市场份额演进显示：2024 年末的高度集中（DeepSeek V3/R1 超半数 OSS 使用）在 2025 年夏中期拐点后瓦解，新入场如 Qwen、MiniMax M2、Kimi K2、OpenAI GPT-OSS 等快速成长至生产规模，显示低切换成本与强试新意愿。2025 年末，无单一开源模型长期占比>25%，使用在 5–7 个模型间更均衡，生态更碎片化、更具竞争性。

关键洞见：Top-tier 多样化；新模型能在数周内放量；迭代优势关键（DeepSeek 持续更新保持竞争力）。对构建者意味着：SOTA 一经发布可迅速获得使用，但要维持份额需持续投入；对用户而言，选择更丰富，某些场景（如 roleplay）开源可与闭源匹敌或更优。

模型尺寸演进（Small/Medium/Large）
定义：Small <15B，Medium 15–70B，Large ≥70B。小模型供给持续但使用份额整体下滑，碎片度高、 churn 快。中模型自 Qwen2.5 Coder 32B（2024-11）起开创赛道，后续如 Mistral Small 3（2025-01）、GPT-OSS 20B（2025-08）共同成熟，用户寻找能力/效率平衡。大模型并未集中于单一标准，而是多极并存：Qwen3 235B A22B Instruct（2025-07）、Z.AI GLM 4.5 Air、OpenAI GPT-OSS-120B（2025-08-05）等各自维持可观使用。市场趋于二元：用户要么选新一代“强壮”的中模型，要么将工作负载集中到最强单一大模型。

类别：开源用途与结构（Categories: OSS）
OSS 覆盖广泛任务，但在 Roleplay 与 Programming 上形成领先。超过半数 OSS token 属于 Roleplay，其次为 Programming。Roleplay 的优势来自创意互动（故事、角色扮演、游戏等）与相对不受限的内容过滤，适合需要灵活响应、情绪与上下文持久的场景，尤其在角色/同人/互动游戏/模拟等社区应用。

中国 OSS 的类别构成显示技术位移：Roleplay 约 33%，Programming+Technology 合计 ≈39%，说明 Qwen、DeepSeek 等越来越多用于代码生成与基础设施工作负载。若只看 Programming，整体仍以闭源占主（如 Anthropic Claude），但 OSS 内部在 2025 年中期由中国 OSS 领先（Qwen 3 Coder 等），到 2025 年 Q4 西方 OSS（Meta LLaMA-2 Code、OpenAI GPT-OSS）冲高后又回落，呈现高竞争与快速替代。注意：该图为份额，非绝对量，OSS 编程总量整体上升。

Roleplay 细分：截至今年末，RoW OSS ≈43%，Closed ≈42%，与年初闭源≈70% 的格局大步收敛；西方 OSS 从≈22% 升，中文 OSS ≈8% 起步亦增长。结果显示健康竞争，开发者识别到 Chat/Roleplay 需求，进行对话微调与人格一致性优化。尽管 Roleplay 覆盖多子类型，但宏观上 OSS 在该创意场景具优势。

解释：OSS 的重度 Roleplay 使用与“爱好者/独立开发者”画像契合（定制与成本优先于绝对准确）；但边界在模糊：OSS 快速改善技术域，闭源也用于创意任务。

Agentic Inference 的崛起（The Rise of Agentic Inference）
我们将“agentic inference”定义为：模型不仅生成文本，更在多步、工具集成、长上下文中规划、行动与交互。五类代理信号显示这种转变：推理模型份额、tool-calling 使用、序列长度分布、以及编程场景复杂度。

份额：2025 年推理优化模型的 token 份额从可忽略升至>50%。供给端（GPT-5、Claude 4.5、Gemini 3）扩展了用户对逐步推理的期待；需求端愈发偏好能管理任务状态、遵循多步逻辑并支持 agent 工作流的模型。带动该变化的头部模型随时间快速轮动：当前 xAI 的 Grok Code Fast 1（排除免费期）领先于 Google 的 Gemini 2.5 Pro/Flash，几周前还是 Gemini 2.5 Pro 领先，DeepSeek R1 与 Qwen3 也处于头部。xAI 的激进发布、定价与开发者注意力促进其代码向变体；同时开放模型如 gpt-oss-120b 的持续存在说明开发者仍会在可能时选择 OSS。结论：推理导向模型正在成为默认路径，其份额是用户偏好如何与 AI 交互的领先指标。

Tool-calling：我们统计 finish reason 为 Tool Call 的 token 份额。与“Input Tool”不同（后者仅表示被提供工具，而不一定调用；该指标 2025-09 才引入，本文不报）。5 月的可见峰主要由一个大账户短期拉动，除外后采用趋势向上。早期主要由 OpenAI gpt-4o-mini、Anthropic Claude 3.5/3.7 系列承载；年中更多模型支持提供工具；9 月末起 Claude 4.5 Sonnet 快速上升；Grok Code Fast、GLM 4.5 等也在渗透。启示：高价值工作流中启用 tool use 正在上升，缺乏稳定工具协议的模型会在企业采用中落后。

序列形态：平均 prompt token 从约 1.5K 增至 6K+，completion 从约 150 增至 400，显示任务更复杂、上下文更丰富。典型请求从“开放生成”（写篇文章）转向对代码库、文档、转录、长会话的推理分析，输出更短而高价值的见解，模型更像“分析引擎”。类别数据揭示驱动因素：编程工作负载是 prompt 增长的主引擎，常见输入 >20K tokens，其他类别较平稳。平均序列长度在 20 个月内从 <2K 升至 >5.4K；编程相关的 prompt 长度是通用类的 3–4 倍，是嵌入式 agent 工作流的显著特征。结论：**LLM 的“中位请求”不再是简单问答，而是结构化的 agent-like 回路**。对模型提供者而言，延迟、工具处理、上下文、以及对不良/对抗性工具链的鲁棒性是新基线；对基础设施而言，平台需承载长对话、执行轨迹、权限敏感集成。**Agentic inference 将在总推断中占据多数（若未已如此）**。

类别：整体 LLM 用途（Categories across all models）
我们按类别刻画所有模型的使用（闭源/开源）。Programming 在 2025 年持续扩张，从年初约 11% 的 token 份额升至近期超过 50%，与 LLM 助力开发环境与工具集成同步；任务从探索式对话转向代码生成、调试、数据脚本等实务。对模型开发意味着更多代码中心训练数据、更深推理与与 IDE 的更紧耦合。

竞争动态：Anthropic 的 Claude 系列长期在编程支出占比 >60%，但 2025-11 周首次跌破 60%；OpenAI 自≈2% 升至≈8%；Google 稳定≈15%；Z.AI、Qwen、Mistral AI 等开源阵营稳步上升，MiniMax 近期增长迅猛。结论：编程是最具战略性的赛道，细微的质量与延迟变化即可周度改写份额，基础设施方与开发者需持续基准评测。

类别内部结构：十二个常见类别显示强“头部意图”。Roleplay 的近 60% token 属于 Games/Roleplaying Games，表明用户以 LLM 为结构化的角色/引擎而非随意聊天；Writers Resources（15.6%）与 Adult（15.4%）强化了互动小说、场景生成与个人幻想的混合。Programming 则有超过 2/3 标记为 Programming/Other，显示需求广泛而非特定语言/工具；Development Tools（26.4%）与脚本语言小份额暗示细分化将提升。翻译内部结构近均衡（Foreign Language Resources 51.1% vs Other），体现多语检索/改写/轻度互译；科学类中 Machine Learning & AI 占 80.4%，显示科学查询多为“元 AI”问题而非广义 STEM；健康类最为碎片化，无单子标签 >25%，分布在医学研究、咨询、治疗指导、诊断查询，提示高变异与安全挑战。金融/学术/法律也较分散，或因领域复杂，或因缺乏成熟工作流。结论：真实使用并非均匀探索，而是集中在少数可复用、高体量任务（Roleplay、Programming、Personal assistance），而科学、健康、法律等仍在酝酿期，有望随模型鲁棒性与工具集成提升而收敛成更明确的高体量应用。

不同提供商的使用画像（Provider usage profiles）
- Anthropic Claude：Programming+Technology 总和 >80%，Roleplay 与通用 Q&A 较少，验证其定位复杂推理、编码与结构化任务。
- Google：用途更散，Translation、Science、Technology、General Knowledge 可见；法律/政策≈5%，科学≈10%；到 2025 年末编程占比降至≈18%，更像通用信息引擎。
- xAI：大部分时期 Programming >80%；到 11 月后科技、Roleplay、Academia 明显上升，与特定消费应用的免费分发相关，带来非开发者流量涌入。
- OpenAI：2025 年初 Science >50%，年末降至 <15%；Programming 与 Technology 各≈29%，向高价值、结构化任务倾斜。
- DeepSeek：Roleplay、闲聊与娱乐占比常 >2/3，少量结构化任务；夏末编程略升。
- Qwen：Programming 长期 40–60%，科技、科学、Roleplay 波动更大；9–10 月 Roleplay 上升，11 月回落，体现异质用户群与快速应用迭代。
结论：不同提供商的分布与战略聚焦一致，无单一模型能最优覆盖所有用例；多模型生态的系统性好处凸显。

地理与语言（Geography and Languages）
全球 LLM 使用呈显著地域差异。花费分布显示：北美仍最大，但多数期已不足 50%；欧洲稳定在“中段”；亚洲从≈13% 升至≈31%，既生产前沿模型，也快速扩大消费。语言上，英语占 80%+；中文、俄语、西班牙语构成长尾，简体中文≈5%，与 DeepSeek、Qwen 等中文 OSS 成长相称。对模型与基础设施方而言，跨语种、合规与部署形态的可用性成为“基本盘”。

留存分析（Analysis of LLM User Retention）
留存以“活动留存”计（即间歇后回归仍计入），故曲线可能轻微非单调。总体高 churn、cohort 快速衰减之下，存在“基础 cohort”的细微且关键信号：并非单纯“早期用户”，而是实现了深层“工作负载—模型契合”的用户群，具有经济与认知惯性的双重粘性。我们提出“Cinderella Glass Slipper effect”（灰姑娘的玻璃鞋效应）：在快速演进的 AI 生态中，存在一组高价值工作负载在多代模型中未被解决；每一代“前沿”模型推出时，都会与这些未解问题“试脚”；当某模型恰好满足此前未能满足的技术/经济约束，即==“玻璃鞋”合脚，形成持久留存==。实证上，Gemini 2.5 Pro（2025-06 cohort）与 Claude 4 Sonnet（2025-05 cohort）在第 5 个月约 40% 留存，明显高于后续 cohort，可能对应推理保真或工具使用稳定性的突破。推论：==当模型率先解决关键负载，会沉淀“基础 cohort”；后来的相近者难以撼动==。相反，若从未建立初始契合（如 Gemini 2.0 Flash、Llama 4 Maverick），各 cohort 均差，显示其并未成为某个“高价值、可黏住”的前沿选项。DeepSeek 的曲线出现“回流跳升”（boomerang effect）异常，如 **DeepSeek R1 的 2025-04 cohort 在第 3 月回升、DeepSeek Chat V3-0324 的 2025-07 cohort 在第 2 月回升**，显示部分流失用户在尝试替代后回归 DeepSeek，归因于其在特定任务的更优技术/成本组合。总体启示：**基础 cohort 是“真实技术进步”的指纹**；识别其早现，可能是判断长期优势的最佳先行信号。

成本与使用动态（Cost vs. Usage Dynamics）
我们在 log–log 坐标下观察类别在成本—使用平面上的分布。垂线为中位成本 $0.73/1M tokens，形成四象限。注意图上的“有效成本”区别于标价：高频负载享受缓存带来的实际价格下降；指标为 prompt+completion 混合费率；排除 BYOK 活动避免自建架构的干扰。

- Premium Workloads（右上）：高成本、高使用（Technology、Science 位于交界）。Technology 成为显著离群点，单位成本最高且使用量高，可能源于系统架构/高级技术问题对模型能力的极高要求。
- Mass-Market Volume Drivers（左上）：高使用、低/中成本（Roleplay、Programming、Science）。Programming 以“职业杀手级”类别脱颖而出，高使用且成本优化到中位附近；Roleplay 使用量接近 Programming，显示面向消费者的对话娱乐可与专业生产力并行，开源在此象限具优势。
- Specialized Experts（右下）：高成本、低使用（Finance、Academia、Health、Marketing），高风险小众领域，付费意愿高但调用频率自然较低。
- Niche Utilities（左下）：低成本、低使用（Translation、Legal、Trivia），功能性与高度优化/商品化倾向。
- 整体弱价格弹性：回归线几乎水平，价格下降对使用的正向拉动很小（10% 降价 ≈ 0.5–0.7% 量增）。闭源（OpenAI、Anthropic）主要集中在高成本高使用；开源（DeepSeek、Mistral、Qwen）集中在低成本高使用。
- 作者 archetypes：Premium leaders（如 Claude Sonnet 3.7/4，≈$2/1M 仍高使用）、Efficient giants（如 Gemini 2.0 Flash、DeepSeek V3 0324，<$0.40/1M 且高使用）、Long tail models（如 Qwen 2 7B Instruct、IBM Granite 4.0 Micro，几美分/1M 但使用低）、Premium specialists（如 GPT-4、GPT-5 Pro，≈$35/1M、低用量的高价值场景）。
- 结论：更便宜的模型可通过效率/集成驱动规模，但高端产品在高风险/高价值任务上仍有定价权，市场未商品化，差异化（延迟、上下文、输出质量）仍具战略价值。
- 运营观察：宏观需求不弹性但微观分层明显；可见 **Jevons paradox** 的影子（成本下降导致总消耗上升）；质量/能力常胜于价格；单纯“廉价”不够，需可区分且足够可靠；“分层产品线 + 基于任务的路由”行之有效；关注“每次成功的成本”，而非“每 token 成本”。

讨论（Discussion）
1. 多模型生态：无一统模型；闭/开源各擅所长，开源合计份额常超 30%。开发者应保持灵活，按任务择优路由；模型提供商需持续改进与差异化。
2. 使用多样性超预期：**Roleplay 与娱乐导向使用巨大**，对评测（连贯性、人格一致性、对话吸引力）与产品（长程记忆、个性化）提出新命题，并为 AI × 娱乐/游戏/IP 打开空间。
3. Agents vs Humans：**agentic inference** 兴起，评估从语言质量转向“任务完成与效率”。
4. 地理：使用全球化与去中心化，亚洲从 13% 升至 31%；中国既是模型生产者也是消费增长极。下一阶段竞争将取决于多语与文化适应，而非仅规模。
5. 成本–使用：市场尚未商品化，价格非唯一决定因素；闭源抓高价值任务，开源承接低成本大体量；开源在推理与编码域快速逼近压缩闭源溢价。
6. 留存与“Glass Slipper”：能力的跨越式进步使“留存”成为护城河度量；首个“解题者”在短暂窗口内形成深度工作负载—模型契合，后续替换成本高。应关注留存曲线中的“基础 cohort”。

限制（Limitations）
本研究仅基于单平台与有限时间窗，缺失企业内网、本地自托管等维度。多项分析基于代理指标（agentic 通过多步/工具调用代理、地域用账单代理），结果应解读为“行为模式”而非对底层现象的最终测量。

结论（Conclusion）
LLMs 正嵌入全球计算基础设施。过去一年催化了对“推理”的再定义：o1-class 模型让延展 deliberation 与 tool use 常态化，评估转向过程与成本—延迟权衡以及编排下的成功率。生态结构性多元：用户按能力、延迟、价格与信任多轴选择，促进快速迭代并降低单一栈依赖。推断也在变化：从静态 completion 到动态 orchestration——**agentic inference 将超过（若未已超过）人类推断**。地域上更分布化，亚洲份额持续增长，中国的 Moonshot AI、DeepSeek、Qwen 等显示开源权重的全球竞争力。实质上，o1 不仅未终结竞争，反而扩展了设计空间：领域正走向系统化思维、仪表化与以真实使用分析为依据的决策。下一年将聚焦运营卓越：真实任务完成、分布偏移下的方差抑制、与生产级需求对齐。

参考文献（References）
- Anthropic economic index report: Uneven geographic and enterprise AI adoption. arXiv:2511.15080, 2025.
- How people use chatgpt. NBER Working Paper 34255, 2025.
- WildChat: 1M ChatGPT interaction logs in the wild. arXiv:2405.01470, 2024.
- OpenAI o1 system card. arXiv:2412.16720, 2024.
- Chatbot Arena: An open platform for evaluating LLMs by human preference. arXiv:2403.04132, 2024.
- Chain-of-thought prompting elicits reasoning in large language models. NeurIPS 2022.
- ReAct: Synergizing reasoning and acting in language models. ICLR 2023.
- The Llama 3 Herd of Models. arXiv:2407.21783, 2024.
- DeepSeek-V3 technical report. arXiv:2412.19437, 2024.
（原文各条参考的完整作者、标题与链接已在上文所列）

致谢与贡献（Contributions）
本工作得益于 OpenRouter 团队的平台、基础设施、数据集与技术愿景（Alex Atallah、Chris Clark、Louis Vichy 等）。Justin Summerville 提供实现/测试/实验支持；Natwar Maheshwari、Julian Thayn 提供发布与设计支持。Malika Aubakirova（a16z）为第一作者，负责实验设计、实现、数据分析与论文撰写；Anjney Midha 提供战略与框架；Abhi Desai 在实习期间支持早期探索与系统搭建；Rajko Radovanovic、Tyler Burkett 提供技术洞见与实务协助。所有贡献者参与讨论、反馈与审稿。

附录与补充（Appendix）
文中多处“上图/下图/表格”提及的结构（如：开源份额演进、工具调用趋势、序列长度增长、类别内部子标签、成本—使用散点等）在原文可视化中给出。三大领域（Roleplay、Technology、Programming）各自呈现明确的子类别结构，反映真实交互中的偏好与工作流形态。

## 欢迎交流与合作
目前主要兴趣是探索agent的落地，想进一步交流可加微信（cleezhang），一些[自我介绍](https://lee-agi.github.io/85ed64eda0/)。

> 本文发表于 2025-12-13_周六。
