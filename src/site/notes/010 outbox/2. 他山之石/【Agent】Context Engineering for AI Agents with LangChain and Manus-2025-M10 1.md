---
{"dg-publish":true,"permalink":"/010-outbox/2/agent-context-engineering-for-ai-agents-with-lang-chain-and-manus-2025-m10-1/"}
---


## 封面
- 标题：Context Engineering for AI Agents with LangChain and Manus
- 链接：https://www.youtube.com/watch?v=6_BcCthVvb8
- 发布日期：2025-10-14
- 总字数：4799
- 预估阅读时长：约 16 分钟
- 生成时间：2025-11-08 19:47:29
- 覆盖时长：01:00:39
- 识别说话人：A, B, C

## 模型总结
## 摘要总结
- Lance 从“prompt engineering”演进到“context engineering”阐明了一个悖论：agent 因为工具调用而需要更多上下文，但模型性能会随上下文增长而下降，故必须主动管理上下文。
- Pete 分享了 Manus 的一线实践：把“缩减上下文”拆成可逆的“compaction”和不可逆的“summarization”，并以==“context rot”阈值驱动==；用“schema 作为契约”既用于总结也用于 agent–sub-agent 通信，稳定可靠。
- 一个非共识但很实用的观点：与其动态上加载大量工具，不如通过==“分层动作空间”（function calling / sandbox utilities / packages&APIs）来“下沉”大多数能力到沙箱和脚本里==，既减小上下文又避免工具混淆。
- 另一个反直觉洞见：==开源模型并不总是更省钱==；在超长输入、需要分布式 KV cache 的 agent 场景里，前沿模型提供商的基础设施有时更经济。
- 最重要的是：请避免“过度上下文工程”。**“Build less, understand more.”** 简化架构经常带来更大跃升。

## 全文
### 开场与议程（00:00:06 - 00:01:07）
**Lance（LangChain）**：大家好，我是 Lance（LangChain 创始工程师之一）。今天和 Manus 的 Pete 一起。我会先基于 Manus 几个月前那篇让我受益很深的博客，快速概览我眼中的“context engineering（上下文工程）”，然后把时间交给 Pete 分享一些博客未覆盖的新想法，最后 Q&A。

### 从 prompt engineering 到 context engineering（00:01:10 - 00:03:42）
**Lance**：你可能听过“prompt engineering”，它在 ChatGPT（2022 年 12 月）之后受到关注，核心是如何更好地提示聊天模型。到了今年 5 月，“context engineering”开始在趋势上升，这与“agents 之年”呼应。构建 agent（即把一个 LLM 绑定若干 tools，并可自循环地调用工具）时会发现：每次工具调用都会产生 observation 并追加进消息列表，消息会指数膨胀。Manus 在文中提到典型任务约需 50 次工具调用；Anthropic 也说生产级 agent 会有数百轮对话。与此同时，有报告显示“context drop”：随着上下文变长，性能下降。于是出现一个悖论：agent 因频繁工具调用而积累大量上下文，但性能却随之下降。“context engineering”就是要在每一步为模型塞进“刚好够用”的信息，从而对抗上下文爆炸。

### Context engineering 的常见主题（00:03:45 - 00:10:05）
**Lance**：
- Offloading（上下文外置）（00:03:56 - 00:05:11）：不必把所有上下文都留在对话消息里。把大 payload（如网页搜索结果）落盘到文件系统，只回传最小引用信息（比如路径），需要时再取。我们在 Deep Agents、OpenDeep Research、Claude Code 等项目里都大量看到这个做法。
- Reducing（缩减）（00:05:13 - 00:06:30）：与 offloading 类似，但通过“总结/压缩”。可总结工具输出、剪枝旧的 tool calls（Claude 4.5 甚至原生支持），或在占用比例到阈值时进行历史 compaction（Claude Code 的 compaction），Cognition 也谈到 agent-to-agent handoff 时的 summarization。
- Retrieving（检索）（00:06:32 - 00:07:16）：两派并存——**索引+语义搜索 vs. 文件系统+简单搜索（glob/grep）**。两者都有效，各有利弊，Q&A 可再聊。无论如何，按需检索是构建有效 agent 的核心。
- Isolation（隔离）（00:07:17 - 00:08:12）：多 agent 分治，每个子 agent 有独立上下文，利于“关注点分离”。从 Manus 宽代理到我们的 Deep Agents、OpenDeep Research、Claude 的研究都在用 sub-agents。
- Caching（缓存）（00:08:05 - 00:08:12）：Manus 在上下文缓存上的实践很有意思，稍后请 Pete 展开。
接着以我们的 OpenDeep Research 开源实现为例（00:08:14 - 00:10:05）：三阶段——scope 研究、multi-agent 研究、single-shot 写作。我们会把“研究简报/计划”offload（放在 LangGraph state 或文件系统），需要时再拉回到消息队列尾部；对“高 token 搜索输出”在研究期做总结；研究中也做 sub-agent 的上下文隔离。总之就是围绕 offload / reduce / retrieve / isolate / cache 这几大主题在不同项目里的组合拳。现在把时间交给 Pete。

### 为什么需要 context engineering（00:10:26 - 00:15:00）
**Pete（Manus）**：我想谈谈为什么要做“context engineering”，尤其当微调/后训练变得容易时。我的前一家公司从零训练过自家 LM 做开放信息抽取/知识图谱/语义搜索，痛点是产品创新速度被训练-评估周期卡死（1-2 周一轮），而且在没找到 PMF 前，优化的基准不一定对产品有意义。对初创企业而言，应该尽量依赖通用模型+context engineering，而不是过早做“专用模型”。后来我们也尝试过基于强 base model 做 RL/后训练，但这又是个陷阱：RL 需要固定 action space、明确 reward 并大量 on-policy rollouts；而 AI/agent 处在强变动期，比如 MCP 的发布就把 Manus 从“紧凑静态动作集”推向“无限可扩展”的开放动作空间——这类开放域优化极难。==除非你要重做一遍模型公司已经做过的基础层，否则请明确边界：现在“context engineering”是“应用/模型”的最清晰、最务实的分界线==。够了，下面上技术。

### Context reduction：Compaction vs. Summarization（00:15:03 - 00:19:22）
**Pete**：我们把“缩减”分成两类：compaction（紧凑化，可逆）与 summarization（总结，不可逆）。
- Compaction：每个工具调用及结果都有 full/compact 两种格式。举例：写文件工具结果里有 path 和 content 字段；返回后文件已存在，因此 compact 里可以安全丢掉很长的 content，只保留 path；agent 需要时再按 path 读取。这样信息没有丢，只是外置。可逆性很关键：agent 的后续推理常依赖早先的动作/观测，无法预先知道哪一步会在 10 步后变关键。
- 但 compaction 终究有天花板，于是我们在触顶前结合 summarization，但会先把关键上下文 offload 成文件，甚至把“预摘要上下文”整体以 log/text 存盘，模型可用 glob/grep 取回。差异在于：**compaction 可逆、summarization 不可逆**，两者都降长度但行为不同。
- 我们会维护若干阈值：模型硬上限（如 1M tokens），以及更早出现的“context rot”阈值（常见在 128K-200K），一旦接近 rot 就触发缩减，且先做 compaction 而不是 summarization。compaction 不等于全史压缩，我们会优先“把最老 50% 的 tool calls 紧凑化，保留最近的完整细节”，给模型留“新鲜的 few-shot 工具用例”，否则模型会学坏（输出缺字段的“紧凑格式”）。
- 多轮 compaction 后如果释放空间仍然有限，就改做 summarization。但注意：总结时使用“完整版本”数据，不用 compact 版本；并且保留“最后几次工具调用与结果”的完整细节，帮助模型“续写”保持风格与连贯性。

### Context isolation：沟通 vs. 共享记忆（00:19:22 - 00:22:15）
**Pete**：我同意 Cognition 指出多 agent 同步很难。但这不是新问题，可借鉴并发编程的智慧。Go 社区有句名言：不要通过共享内存来通信，而要通过通信来共享内存。翻译到上下文就是两种模式：
- By communicating（沟通式）：主 agent 下发明确指令，子 agent 的上下文只有这段任务说明。适合“目标清晰、只要最终产出”的任务（如在代码库里找片段）。Claude Code 就常这么做。
- By sharing memory（共享式）：子 agent 能看到完整历史（工具使用、笔记等），但有自己的 system prompt 和 action space。适合像 deep research 这类“最终报告严重依赖中间过程”的场景。注意共享很贵：输入更大、不同 system prompt/action space 导致 KV cache 不能复用，需要全价支付。

### Context offloading 的新维度：把“工具”也离线（00:22:18 - 00:28:26）
**Pete**：系统变大（尤其集成 MCP）后你会发现：工具定义本身也占据大量上下文，工具太多会导致“工具混淆”（调用错工具/不存在的工具）。常见做法是**对“工具描述”做动态 RAG**（影响cache），按任务加载，但这会引发两点：1）注入在前缀的工具定义频繁重置，2）上下文里仍保留对“已移除工具”的历史 few-shot，会诱导再次调用无效工具。
我们在 Manus 里实验“分层动作空间（layered action space）”：
- Level 1：function calling（函数调用）。只保留极少“原子函数”（读写文件、执行 shell、搜索文件/互联网、浏览器操作等），边界清晰，可组合复杂流程，其他能力下沉到下一层。
- Level 2：sandbox utilities（沙箱工具）。每个会话跑在我们自研 Linux 沙箱内，Manus 通过 shell 调用预装的 CLI 工具（格式转换、ASR，乃至“Manus 的 MCP CLI”），不把 MCP 工具注入到 function calling。优点：高吞吐/大输出走文件或分页，并用 grep/cat/less 等链式处理；缺点：与前端的低延迟交互更难可视化。
- Level 3：packages & APIs（包与 API）。写 Python 脚本调用“预授权 API/自定义包”（如 3D 建模库、金融 API），我们代购 API（订阅内含 API keys）。适合需要大量计算与内存但无须把原始数据塞进模型上下文的任务（如按年分析行情，用代码计算，仅把摘要回填到上下文）。代码/API 高可组合性可以“一步串多步”（类似 CodeAct 的思想），但它不是 schema-safe，约束解码难度高，所以要选对场景。
好处是：对模型而言，三层都经由“标准 function calling”入口，**接口简单、cache 友好、彼此正交**。==shell/文件函数是模型最熟悉的原语==，认知负担低。

### 五维的平衡与反思（00:28:23 - 00:30:03）
**Pete**：Offload/Reduce/Retrieve/Isolate/Cache 并非独立：offload+retrieve 让 reduce 更高效；可靠的 retrieve 让 isolate 更安全；isolate 也会减缓上下文增长、降低缩减频率；但 isolate 与过度 reduce 又会影响 cache 效率与输出质量。**上下文工程说到底，是在多目标冲突下寻找平衡的艺术与科学。**
最后给一个反面提醒：请避免“context over-engineering”。我们过去 6-7 个月最大的跃升，来自简化（而不是堆更多花哨层）。每次简化，系统就更快、更稳、更聪明。目标是“让模型的工作更简单”，不是更难。若只记一句话：**“Build less, understand more.”**

### Q&A：LLM 如何发现并调用沙箱工具（00:30:15 - 00:31:39）
**Lance**：LLM 如何知道有哪些 shell 工具、怎么调用？
**Pete**：我在 system prompt 里给 Manus 一个提示：有一批预装 CLI 工具在特定目录（类似 /usr/bin）。常用的我们会“仅列名不详解”，并明确“可以安全使用 --help”，因为这些工具由我们统一实现、格式一致。

### Q&A：索引 vs. 文件系统（00:31:42 - 00:32:41）
**Lance**：是否会临时起 vector store 做索引？
**Pete**：我们像 Claude Code 一样更依赖 grep/glob，不在会话里临时建索引（冷启动时间不够）。但若做企业长期记忆，或对接外部知识库，还是需要外部向量库，这取决于规模与时延预算。

### Q&A：长期记忆（00:32:42 - 00:34:12）
**Lance**：你们如何做跨会话的 memory？
**Pete**：我们有“knowledge（显式记忆）”。比如我可以说“以后把结果都导成 Excel”，系统会弹窗“学到了 X，是否写入记忆？”需要用户确认。我们也在探索参数无关的“在线自改进”：用户在数据可视化里常纠正“中文/日文/韩文字体渲染”的问题（比如要用 Noto Sans CJK），我们会聚合这类“共识纠正”，逐步学会默认处理。

### Q&A：模型变强后如何“减脚手架”（00:34:14 - 00:36:26）
**Lance**：模型变强后会删掉哪些 scaffold？如何评估？
**Pete**：我们今年 3 月以来已重构 5 次。做法是“固定架构，切换强/弱模型做 A/B”，如果同一架构能从模型升级中大幅获益，它就更具“未来适应性”。此外我们每 1-2 个月回顾一次，并用开源/专有的早期模型做预研，提前为下一波升级做架构准备。

### Q&A：数据存储格式（00:36:32 - 00:37:28）
**Lance**：偏好哪些文件格式？
**Pete**：更偏好“行导向”的纯文本（line-based），便于 grep/按行范围读取。Markdown 虽强，但容易诱发模型输出过多“项目符号”，不总是我们想要的。

### Q&A：如何提示出“高召回”的总结（00:37:29 - 00:38:39）
**Lance**：summarization 不可逆，如何避免丢信息？
**Pete**：别用“自由生成”的摘要，改用“结构化 schema（表单）”：固定字段（修改了哪些文件、用户目标、当前进度等），让模型填表。输出稳定、可迭代。

### Q&A：Compaction 机制与标识（00:38:39 - 00:39:30）
**Lance**：紧凑化具体怎么回传？
**Pete**：不仅紧凑 tool call，也紧凑 tool result。大多数动作都有天然“唯一标识”：文件操作有 path，浏览器有 URL，搜索有 query。回传标识就够了，需要时再外部取回。

### Q&A：大结果的搜索怎么既“可见”又不撑爆上下文（00:39:30 - 00:41:32）
**Lance**：搜索结果很大，该回传全文、摘要，还是先回再紧凑？
**Pete**：看复杂度。复杂检索（多查询、筛选、对比）可用“agent-as-tool”：主模型只调用“advanced_search”函数，实际触发一个有固定 schema 的子 agent workflow，回传的是结构化结果。简单检索则回传完整结果并依赖后续 compaction。同时，我们鼓励模型把“中间洞见”写入文件，防止紧凑发生得比模型预期更早。

### Q&A：agent–agent 通信与产出收敛（00:41:35 - 00:43:26）
**Lance**：多 agent 如何既传够信息又不超前填？
**Pete**：我们最近上线“wide research”，借鉴 map–reduce。由于所有子 agent 共享同一沙箱，传递“路径”即可。更关键的是“输出收敛”：主 agent 在 spawn 子 agent 前先“定义输出 schema”，子 agent 用一个“submit_result”工具在约束解码下回传，最终就像填一张受 schema 约束的表。
（旁白：你会看到我们在 summarization 和 agent–agent 通信里都用“schema 作为契约”。）

### Q&A：模型选择与成本（00:43:53 - 00:46:00）
**Lance**：用不开源模型？会做微调吗？
**Pete**：我们目前不在生产用开源模型，原因不是质量而是成本结构：agent 输入远大于输出，分布式 KV cache 很关键，而前沿模型提供商的“输入缓存/分布式缓存”基础设施更成熟，算总账反而更省。我们会做“任务/子任务级路由”：coding 任务更偏 Claude，多模态偏 Gemini，复杂数学/推理看 OpenAI 新模型。
（补充）KV cache 我指的是提供商的“输入缓存（input caching）”能力。

### Q&A：工具数量与“分层动作空间”的取舍（00:46:15 - 00:49:32）
**Lance**：不做工具索引动态加载时，多少工具算太多？
**Pete**：依模型而异，我的“拍脑袋上限”是 ≈30，但我们更强调把“原子函数”控制在 10-20 个，把复杂能力“下沉”到沙箱或脚本。
**Lance**：混合“函数直调 + 沙箱脚本”吗？
**Pete**：是。纯 CodeAct（直出代码即执行）会失去“约束解码”的安全网，容易走偏；但在“大数据量计算、只回填摘要”的场景里，代码很强。我们采用混合：可 schema-safe 的走 function calling，其余在沙箱/脚本里做，再把“少量摘要/引用”塞回上下文。
（旁白）这也呼应前面观点：计算机是图灵完备的，有“shell + text editor”，agent 理论上就能完成“人类实习生”能做的绝大多数屏幕工作。

### Q&A：规划从 todo.md 到规划 agent（00:49:34 - 00:51:19）
**Lance**：Manus 早期的 ToDo 工具现在还用吗？
**Pete**：我们把“to-do.md”换成了“结构化规划器”，本质上是“agent-as-tool”的一个 planner agent。过去更新清单很费 token，现在由专门 agent 管理计划，必要时也可用不同模型做“外部审阅”。

### Q&A：多 agent 组织方式（00:51:20 - 00:53:02）
**Lance**：你们按“角色”（设计/编程/经理）拆 agent 吗？
**Pete**：不。那是人类公司因人类上下文限制采取的分工。我们只保留很少的核心 agent：一个“通用执行器”、一个“规划器”、一个“知识管理器”、以及少量如“数据/API 注册”的 agent。更多能力通过“agent-as-tool”的工作流封装，减少跨 agent 通信成本。

### Q&A：安全与 Guardrails（00:53:30 - 00:55:16）
**Lance**：怎么做安全/风控？
**Pete**：有网的沙箱“样样都可能危险”。我们做了多层出站检查，防止“token/隐私”泄出沙箱；网页端我们支持登录态持久化，但网页本身可能做 prompt injection，这一层也在与“computer-use models”（如 Anthropic）合作迭代。当前策略是：**敏感操作都需要用户确认**，必要时让用户接管；随着模型原生 guardrails 变强，再逐步减少打扰。

### Q&A：Evals（00:55:21 - 00:57:07）
**Lance**：你们如何做评测？
**Pete**：我们从学术基准（如 GAIA）出发，但很快发现“与真实用户偏好不一致”。现在三件事并行：1）用户评分（1-5 星）是金标准；2）自动化可验证任务（我们自建“偏执行型”的数据集，不止“读-only”）；3）大量人工评审（例如网页/可视化的“审美/样式”很难用自动 reward 判断）。

### Q&A：RL with verifiable rewards vs. 直接用工具型模型（00:57:07 - 00:58:50）
**Lance**：自己做 RL 还是用提供商的工具增强模型？
**Pete**：我做过很多年 RL/后训练，但现在如果要全面支持 MCP，就不是“固定 action space”了，reward 设计与数据分布都会失衡，你基本是在“自建基础模型”。我们的方向是：**更多“参数无关”的在线个性化学习**（比如聚合纠偏），把模型训练留给模型公司。

### Q&A：复用提供商工具名以“解锁”习得能力？（00:58:52 - 01:00:02）
**Lance**：把自家工具命名成与提供商一致，能“解锁”他们的 RL 能力吗？
**Pete**：我们反而尽量避免同名，因为我们的函数签名/参数约束可能不同，同名会“误 few-shot”，造成混淆。

### 收尾与行动号召（01:00:05 - 01:00:46）
**Lance**：今天到点了。录播和幻灯片会提供。
**Pete**：欢迎大家试用 Manus，我们有免费层。
**Lance**：谢谢 Pete！希望下次再聊。

——

加粗处为我标注的少量关键非共识/高价值见解。以上内容保留原时间线，并尽量在翻译中保留关键英文术语（如 agent、LLM、KV cache、RAG、Claude Code、MCP、CodeAct 等）以保证技术语义的准确性。

## 欢迎交流与合作
目前主要兴趣是探索agent的真正落地，想进一步交流可加微信（微信号：cleezhang），一些[自我介绍](https://lee-agi.github.io/Insights/16695c5e82/)。

> 本文发表于 2025-11-09_周日。
