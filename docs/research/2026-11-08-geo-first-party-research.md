# GEO 第一手资料技术研究：Google AI 功能、OpenAI 搜索爬虫、飞书长连接与 GEO 测量边界

- 文档版本：v1.0
- 研究日期：2026-11-08
- 研究方式：公开一手资料只读检索与交叉核对
- 适用项目：GEO Agent / GEO 工作台（跨境电商出海品牌场景）
- 文档位置：`docs/research/2026-11-08-geo-first-party-research.md`
- 阅读限制：本文主要研究 Google、OpenAI 和飞书消息事件，并补充 Bing 官方聚合数据来源，不是全平台清单；飞书卡片交互回调及项目内部资产以 [总体计划](../superpowers/plans/2026-11-08-geo-agent-workbench-master-plan.md) 和其链接的一手资料为准。
- 范围：Google Search Central AI features 官方指引；OpenAI 网络爬虫与搜索发布官方文档；飞书官方消息长连接、事件订阅、机器人、权限、上线文档；GEO 测量中可审计边界；参考框架本身的第一手资料。
- 不在本文件范围：`pmo.luteos.com`、`main.luteos.com` 的内部资料；Momcozy 私有文档；飞书实际配置操作；站点真实数据审计。上述内容需另行研究或实测。

**安全与合规声明**

- 本文不包含、不复述、不推断任何用户提供的 App ID、App Secret、token、cookie、密钥或私有资料。
- 本次研究仅只读访问公开官方文档；未登录飞书、未调用飞书 API、未修改任何飞书配置、未向任何外部系统写入数据。
- 本文所有“官方”判断均以文末来源清单中的公开页面为准；页面内容与日期可能随平台更新而变化，引用时须带访问日期。

**状态标记**

| 标记 | 含义 |
|---|---|
| 【官方】 | 官方文档或官方博客明确写出，可直接引用 |
| 【官方/多语言页】 | 官方帮助中心的多语言页面；英文页在访问时可能指向其他文章，已在正文注明 |
| 【推断】 | 由多条官方事实推导出的工程或产品结论，官方未逐字写出 |
| 【待实测】 | 官方文档未给出确定答案，必须在项目环境验证 |
| 【未发现】 | 截至访问日，在官方文档中未找到相应能力、接口或声明；不等同于官方否认 |
| 【建议】 | 本研究给出的产品或架构建议，不是官方立场 |

**引用规则**

正文以 `[S1]`、`[S2]` 等编号标注来源；编号对应文末“来源清单”。每个来源给出标题、发布/更新日期（如有）、URL、访问日期。官方原文引用保留英文，其后给中文释义；中文释义不是官方翻译。

---

## 0. 执行摘要

1. **Google 的官方立场很明确：AI Overviews / AI Mode 仍然属于 Search，SEO 基础仍然适用；没有额外的“AI 专属优化”要求。** Google 明确说不需要特殊的 schema、不需要把内容切碎、不需要为 AI 重写内容，也不需要 `llms.txt`；Google Search 不使用 `llms.txt`，会忽略它。[S1][S2]
2. **Google 当前的“AI 可见性”第一方测量入口是 Search Console 的 Generative AI performance report，核心指标是 impressions（展示），维度只有页面、国家、日期、设备；官方口径未提供查询词、引用位置、答案文本，AI 专属点击也无法从该报告单独获得。** 该报告于 2026-06-03 发布，2026-08-31 全球全量；同时 Search Console 提供 Search generative AI control，默认纳入，站点可主动排除。[S5][S6][S7]
3. **OpenAI 的搜索收录控制核心是 `OAI-SearchBot`，训练控制是 `GPTBot`，用户主动抓取是 `ChatGPT-User`，三者相互独立。** 官方建议允许 `OAI-SearchBot` 并放行其公开 IP 段；被 `OAI-SearchBot` 排除的站点不会出现在 ChatGPT 搜索答案中，但仍可能以导航链接形式出现；robots.txt 更新约 24 小时生效。[S11]
4. **OpenAI 不保证展示；截至访问日，未发现 OpenAI 提供发布者侧的 impressions / 引用份额分析。** 官方明确说“Placement is not guaranteed”，搜索引用可能不完整、过时或错误；可审计的第一方信号主要是 `utm_source=chatgpt.com` 带来的引荐点击。[S13][S14]
5. **飞书长连接适合本项目的回调方式，但有硬约束：仅企业自建应用；服务器只需出网、不需要公网 IP/域名；单应用最多 50 个连接；消息需 3 秒内处理完成；多客户端为集群模式、不广播、随机一个客户端收到；至少一次投递、必须幂等；不支持代理；SDK 在短时断网后自动重连。** [S15][S16][S22]
6. **飞书消息事件 `im.message.receive_v1` 的权限、单聊/群聊范围、去重键和上线流程都有官方明确要求。** 单聊与群聊 @ 需要分别申请权限；群聊全部消息属于敏感权限；官方特别提示该事件重复推送时用 `message_id` 去重，不要依赖 `event_id`；权限、事件订阅、应用功能变更都需要发布新版本并经管理员审核。[S17][S19][S21]
7. **GEO 测量需要区分平台整体事实与可审计样本：** Google AI impressions、站点日志和配置状态等可按其各自口径复核；保留原始答案、引用、时间与处理版本的受控样本同样可审计，但只能支持样本范围内的结论，不能代表平台全部曝光或用户体验。Bing AI Performance 提供自身覆盖范围的引用聚合；其 Citation Share 为特定 grounding query 的站点引用占比，2026-06 发布时为 preview，不能等同 Google 指标或总体市场份额。[S1][S6][S13][S24][S27][S28]
8. **参考框架的第一手资料支持“证据分级”而不是“排名保证”：** GEO 学术框架（KDD 2024）给出了可复现的 visibility 指标与 GEO-bench，但其生成引擎是合成环境和 Perplexity，不是 Google AI Overviews / ChatGPT；`llms.txt` 是提案而非标准，Google 明确忽略；schema.org/Product 与 Merchant Center 是电商商品信息的第一方通道，不是“AI 排名黑客”。[S2][S10][S24][S25]
9. **对 GEO Agent 的核心设计含义：** 应构建“可审计的 GEO 证据工作台”，而不是“保证 AI 排名/引用”的工具；所有对外结论必须区分平台第一方事实、抽样估计和第三方评分；飞书接入应按“3 秒 ack + 异步队列 + 幂等 + 单消费者/共享队列 + 发版审核”设计。[S15][S16][S22]

---

## 1. Google Search Central：AI 功能官方指引

### 1.1 AI Overviews 与 AI Mode 的官方定义

【官方】Google 把 AI Overviews 和 AI Mode 视为 Search 的生成式 AI 体验：AI Overviews 用于快速给出复杂主题的要点并提供继续探索的链接；AI Mode 更适合需要进一步探索、推理或复杂比较的查询，给出带支持链接的综合性 AI 回答。[S1]

【官方】两者都可能使用 “query fan-out” 技术：针对原始查询并发发出多个相关子查询，从更多子主题和数据源检索，再生成回答；因此它们可能展示比传统搜索结果更广、更多样的支持链接。[S1]

【官方】AI Mode 与 AI Overviews 可能使用不同的模型和技术，因此它们展示的回答和链接集合会不同；AI Overviews 只在系统判断其相比经典 Search 有额外价值时才展示，很多时候不会触发。[S1]

> 原文：“AI Mode and AI Overviews may use different models and techniques, so the set of responses and links they show will vary. AI Overviews are only shown when our systems determine that it is additive to classic Search, and as such, often don't trigger.” [S1]

**对 GEO 的含义**【推断】：同一查询在 AI Overviews、AI Mode、传统 Search 中的结果不可互相替代；任何“AI 可见性”测量都必须记录具体产品面（AI Overviews / AI Mode / ChatGPT / Perplexity 等）、时间、地域、语言和设备，不能把它们合并成一个“AI 排名”。

### 1.2 进入 AI 功能的技术资格

【官方】要被展示为 AI Overviews 或 AI Mode 的支持链接，页面必须已被索引，并且有资格在 Google Search 中以 snippet 形式展示，满足 Search 的技术要求；没有额外的技术门槛。[S1]

【官方】Google 在 2026-07-10 更新的生成式 AI 优化指南中进一步写明：除了 Search 技术要求外，站点还必须被包含在 Search Console 的 “Search generative AI features” 控制中，才有资格出现在 Google Search 的生成式 AI 功能中；该控制默认是“包含”。[S2][S7]

【官方】即使页面满足所有要求、最佳实践和 policy，也不保证 Google 会抓取、索引或展示它；索引和展示都不是保证的。[S1][S2]

**注意文档演进**：2025-12-10 更新的 “AI Features and Your Website” 页面说“没有额外要求”；2026-07-10 更新的生成式 AI 优化指南加入了“必须被包含在 Search generative AI features 控制中”的表述。两者并不矛盾：控制默认包含，但站点若主动排除，就会失去资格。[S1][S2][S7]

### 1.3 Google 明确说“不需要做”的事情

【官方】Google 明确列出了围绕生成式 AI 搜索的常见误区，以下事项对 Google Search 不需要：

- **`llms.txt` 和其他“特殊”标记**：不需要创建新的机器可读文件、AI 文本文件、标记或 Markdown 来出现在 Google Search（包括其生成式 AI 能力）中，因为 Google Search 本身不使用它们；Google 可能发现、抓取、索引 HTML 之外的许多文件，但这不代表该文件会被特殊对待。Google 说，为其他服务维护 `llms.txt` 完全可以，但“既不会伤害也不会帮助”在 Google Search 中的可见性或排名，因为 Google Search 忽略它们。[S2]
- **“Chunking” 内容**：不需要把内容切成很小的片段来让 AI 更好理解；Google 系统能理解一个页面上多个主题的细微差别，并向用户展示相关部分；没有理想页面长度。[S2]
- **为 AI 系统重写内容**：不需要用特定方式写作来迎合生成式 AI 搜索；AI 系统能理解同义词和一般含义，不必覆盖所有长尾关键词变体。[S2]
- **追求不真实的 “mentions”**：在博客、视频、论坛等地方制造不真实的提及，并不比看起来更有用；Google 的核心排名系统关注高质量内容，其他系统拦截 spam，生成式 AI 功能同时依赖两者。[S2]
- **过度关注结构化数据**：结构化数据不是生成式 AI 搜索的必需项，也没有需要添加的特殊 schema.org 标记；但继续使用结构化数据作为整体 SEO 策略的一部分是好的，因为它有助于获得 Search 的 rich results。[S2]

> 原文：“LLMS.txt files and other "special" markup: You don't need to create new machine readable files, AI text files, markup, or Markdown to appear in Google Search (including its generative AI capabilities), as Google Search itself doesn't use them... Google Search ignores them.” [S2]

**对 GEO 的含义**【推断】：任何把 `llms.txt`、内容切块、特殊 schema、批量“提及”包装成 Google AI 排名利器的方案，都与 Google 官方指引冲突；GEO Agent 不应把这些作为核心卖点，最多作为面向其他 agent/文档系统的辅助产物，并明确标注“对 Google Search 无直接影响”。

### 1.4 内容质量与电商相关指引

【官方】Google 说生成式 AI 功能根植于其核心 Search 排名和质量系统；SEO 最佳实践仍然适用。内容应独特、有价值、以人为先，提供独特观点、第一手经验、非商品化内容，组织清晰，适当使用高质量图片和视频。[S2][S4]

【官方】不要为了操纵排名或生成式 AI 回答，针对每个可能的查询变体批量生产页面；这可能违反 Google 的 scaled content abuse spam policy。使用生成式 AI 工具辅助内容创作时，必须满足 Search Essentials 和 spam policies；AI 生成内容可能包含不准确信息，发布前必须人工事实核查和审阅，包括 title、meta description、结构化数据、图片 alt 文本等元数据。[S2][S9]

【官方】对电商站点：Google 的生成式 AI 回答可能包含商品列表、商品信息和本地商家信息；使用 Merchant Center（例如 Merchant Center feeds）和 Google Business Profiles 可以帮助商品和服务在 AI 回答及其他 Google Search 结果中可见。Google 还提到 Business Agent 等商家体验。[S2]

【官方】Product 结构化数据支持 product snippets 和 merchant listings；商品信息可以展示价格、库存、评分、配送等信息。Google 建议：可以在网页添加 Product 结构化数据、向 Google Merchant Center 上传数据 feed 并在 Merchant Center 中选择免费 listing，或两者都做；两者同时提供可以最大化商品可见性。[S10]

【官方】对电商的 AI 生成内容，Google Merchant Center 有专门政策：AI 生成图片必须包含 IPTC `DigitalSourceType` 的 `TrainedAlgorithmicMedia` 元数据；AI 生成的商品数据（如 title、description 属性）必须单独标注为 AI 生成。[S9]

**对 Momcozy 这类跨境电商品牌的含义**【推断】：商品页的第一方抓手是 Product 结构化数据 + Merchant Center feed + 准确的库存/价格/配送信息，而不是“AI 专用 schema”；AI 生成商品文案必须解决标注与事实核查，否则会带来 Merchant Center policy 风险。

### 1.5 抓取与内容控制：别把三种控制混为一谈

【官方】Google 说 AI 已内建于 Search，是 Search 运作的一部分；因此 **Googlebot 的 robots.txt 指令**是站点所有者管理“站点如何被 Search 抓取”的控制手段。要限制页面在 Search 中展示的信息，可以使用 `nosnippet`、`data-nosnippet`、`max-snippet` 或 `noindex`。[S1]

【官方】`Google-Extended` 是一个独立的产品 token，不是单独的 HTTP user-agent 字符串；抓取仍由现有 Google user-agent 完成，`Google-Extended` 只在 robots.txt 中以控制 token 的形式使用。它用于管理 Google 抓取的内容是否可以用于训练未来代际的 Gemini 模型（驱动 Gemini Apps 和 Vertex AI API 的 Gemini），以及用于 Gemini Apps 和 Vertex AI 上 “Grounding with Google Search” 的 grounding。[S3]

【官方】`Google-Extended` 不影响站点在 Google Search 中的收录，也不作为 Google Search 的排名信号。Google 还表示打算在 2027 年初支持 AIPREF 共享机器可读训练偏好信号（`user-agent: __usage_ai-training`）。[S3]

【官方】Search Console 的 “Search generative AI control” 管理站点是否出现在 Google Search 的以下生成式 AI 功能中：AI Overviews、AI Mode、Google Discover 中的生成式 AI 功能。默认是“包含”；选择“排除”后，站点链接和内容不会出现在这些功能中，也不会作为生成 AI 回答的 grounding 输入；该控制不会作为这些生成式 AI 功能之外其他 Search 的排名或收录信号；不影响 AI 训练（限制训练用 `Google-Extended`）；要完全阻止内容出现在 Google Search，用 `noindex`。[S7]

【官方】控制变更生效时间：一般在几天内；内容会在控制生效后 1–2 天内被排除，但由于缓存和 Google 系统间传播，部分内容可能更久。[S7]

**三种控制的区别**【推断，基于 S1/S3/S7】：

| 控制 | 作用面 | 是否影响 Google Search 收录/排名 | 是否影响 AI 训练 |
|---|---|---|---|
| `Googlebot` robots.txt / `noindex` / snippet 控制 | Google Search 抓取与展示 | 是 | 不是主要控制 |
| `Google-Extended` | Gemini 训练与部分 grounding | 官方明确说不影响 Search 收录和排名 | 是 |
| Search Console “Search generative AI control” | Google Search 的 AI Overviews / AI Mode / Discover 生成式 AI 功能 | 只影响这些 AI 功能，不作为其他 Search 的排名信号 | 官方明确说不影响训练 |

> 原文：“Google-Extended does not impact a site's inclusion in Google Search nor is it used as a ranking signal in Google Search.” [S3]

**对 GEO 的含义**【推断】：GEO Agent 的“AI 可见性开关”不能只写一个 robots.txt 规则；必须区分“Search 抓取”“Search 生成式 AI 功能纳入”“Gemini 训练/grounding”三条线，并分别核对。把 `Google-Extended` 当作“退出 AI Overviews”的开关是错误的。

### 1.6 Google 的第一方测量：Generative AI performance report

【官方】2026-06-03，Google 宣布在 Search Console 推出 Search Generative AI performance reports，包含 Search 和 Discover 的专用报告，用于查看站点在生成式 AI 功能中的可见性；2026-08-31 全球全量推出。[S5][S8]

【官方】Search 的生成式 AI 表现报告包含 AI Overviews 和 AI Mode 的 impressions；报告可按 Pages、Countries、Dates、Devices 维度查看；可以按 search type 过滤（Web: text-based、Web: multimodal）；不包含 Search Labs 实验数据；报告有常规 Search performance 的 1,000 行限制、时间范围限制；最新数据可能是 preliminary，会变化。[S6]

【官方】报告只描述 impressions：即站点链接在生成式 AI 功能中被展示给用户的次数；同一站点两个结果在同一 AI 功能中出现，在图表总计中算一次 impression（按 property 聚合）；按页面维度时按页面聚合；图表总计和表格总计可能因聚合方式不同而不一致。[S6]

【官方】该报告的数据来自 Performance report（Search results）的 Web search type；AI 功能的展示也仍然包含在整体 Search Console 的 Web search type 中。[S1][S6]

【官方】Google 明确警告：要警惕那些承诺排名成功、或声称使用 Google “内部”指标/数据的第三方工具；没有任何第三方工具能访问 Google 的内部排名或 AI 系统。可以用这些工具辅助工作流，但要对照 Google 官方指引评估其建议。[S2]

**官方没有给出的东西**【未发现/推断】：

- 没有查询词维度；报告维度只有 Pages、Countries、Dates、Devices。[S6]
- 没有引用位置、引用段落、答案文本、引用份额、竞品对比。[S6]
- 官方文档只描述 impressions，没有描述 AI 功能专属 clicks；整体 Web search type 有 clicks，但无法从中单独拆出 AI 功能 clicks。[S1][S6]
- Google Analytics 可以跟踪转化和停留时间；Google 说来自带 AI Overviews 的搜索结果页的点击质量更高；但官方没有说 Analytics 能把“AI Overview 点击”与普通 Google organic 点击自动分开。[S1]【推断：默认不能直接拆分，需要实验/落地页/UTM 等额外设计】

> 原文：“No third-party tool has access to our internal ranking or AI systems.” [S2]

**对 GEO 的含义**【推断】：本次查阅的 Google 发布者文档中，Search Console 的 AI 专项报告提供的可核对 AI 可见性指标是 impressions；它是聚合展示数据，不是“排名”。该报告没有提供 AI 引用份额、答案位置或排名，不能把自行估计的这些结果称为该报告的第一方数据。

### 1.7 Google 侧的关键结论

- 【官方】AI 功能仍以 Search 的索引、snippet 资格、核心排名与质量系统为基础；没有额外技术要求，没有特殊 schema，没有 `llms.txt` 红利。[S1][S2]
- 【官方】AI Overviews / AI Mode 的结果会因模型和技术不同而变化；不保证触发、不保证展示。[S1]
- 【官方】测量用 Search Console 的 Generative AI performance report；它只给 impressions，维度有限；不要相信声称能拿到 Google 内部 AI 指标的第三方工具。[S2][S6]
- 【官方】控制要分清：Googlebot/robots.txt、`Google-Extended`、Search generative AI control、`noindex`/snippet 控制，各自作用不同。[S1][S3][S7]
- 【官方】电商用 Product 结构化数据 + Merchant Center feed + Business Profile，而不是“AI 专用 markup”。[S2][S10]
- 【官方】Google 把 AEO/GEO 视为 SEO 的一部分：从 Google Search 的视角，为生成式 AI 搜索优化就是为搜索体验优化，仍然是 SEO。[S2]

---

## 2. OpenAI：网络爬虫与搜索发布官方文档

### 2.1 OpenAI 的 user agent 分工

【官方】OpenAI 使用多个 web crawler / user agent 为产品执行自动或用户触发的操作；`OAI-SearchBot` 和 `GPTBot` 是站点通过 robots.txt 管理内容用途的两个主要 tag；每个设置相互独立。[S11]

| User agent | 官方用途 | robots.txt 行为 | 是否决定 Search 收录 | 是否用于训练 |
|---|---|---|---|---|
| `OAI-SearchBot` | 用于搜索，在 ChatGPT 搜索功能中展示网站 | 允许则可被抓取用于搜索；排除后不会出现在 ChatGPT 搜索答案中，但仍可能作为导航链接出现 | 是，Search 的主要控制 | 官方说独立于训练；允许 `OAI-SearchBot` 是为了出现在搜索结果 |
| `GPTBot` | 抓取内容，可能用于训练生成式 AI 基础模型 | 禁止 `GPTBot` 表示内容不应被用于训练 | 不是 Search 收录控制 | 是 |
| `ChatGPT-User` | 用户在 ChatGPT / Custom GPT 中的某些动作触发访问网页 | 由于是用户发起，robots.txt 规则可能不适用 | 官方明确说不用它决定内容是否出现在 Search | 不是自动网页抓取 |
| `OAI-AdsBot` | 验证提交为 ChatGPT 广告的落地页安全性；可能用落地页内容判断广告相关性 | 仅访问提交为广告的页面 | 不是 Search 收录控制 | 官方说收集的数据不用于训练基础模型 |

> 原文：“OAI-SearchBot is for search. OAI-SearchBot is used to surface websites in search results in ChatGPT's search features. Sites that are opted out of OAI-SearchBot will not be shown in ChatGPT search answers, though can still appear as navigational links.” [S11]

> 原文：“ChatGPT-User is not used to determine whether content may appear in Search. Please use OAI-SearchBot in robots.txt for managing Search opt outs and automatic crawl.” [S11]

【官方】如果站点同时允许 `OAI-SearchBot` 和 `GPTBot`，OpenAI 可能只使用一次抓取结果同时服务两个用途，以避免重复抓取；robots.txt 更新后，搜索结果侧大约需要 24 小时调整。[S11]

【官方】OpenAI 公布了各 crawler 的 IP 段 JSON：`https://openai.com/searchbot.json`、`https://openai.com/gptbot.json`、`https://openai.com/chatgpt-user.json`、`https://openai.com/adsbot.json`。[S11]

**对 GEO 的含义**【推断】：`OAI-SearchBot` 是“是否允许进入 ChatGPT 搜索”的开关；`GPTBot` 是“是否允许训练”的开关；`ChatGPT-User` 是用户主动访问，不应被当作搜索收录控制。GEO Agent 的审计报告必须分别检查三类 agent，不能把“允许 GPTBot”当成“进入 ChatGPT 搜索”。

### 2.2 进入 ChatGPT 搜索的官方条件

【官方】OpenAI 在 2024-10-31 发布 ChatGPT search：ChatGPT 可以基于用户问题选择搜索网页，回答中带来源链接，用户可打开 Sources 侧栏查看引用；任何网站或发布者都可以选择出现在 ChatGPT 搜索中。[S12]

【官方】ChatGPT 搜索使用第三方搜索提供方，以及合作伙伴直接提供的内容；搜索结果和引用可能不完整、过时或错误；用户应打开引用来源核对。[S12][S13]

【官方】ChatGPT 搜索有时会把用户查询改写成一个或多个更具体的查询发给第三方搜索提供方；例如先查 “CCR8 immunotherapy drug development 2025”，再根据结果发更具体的查询。[S13]

【官方】要让网站有资格被纳入 ChatGPT 搜索结果：允许 `OAI-SearchBot` 抓取站点，并确认网站 host 或 CDN 允许来自 OpenAI 公布的 searchbot IP 地址的流量。[S13]

【官方】ChatGPT 使用多个因素对搜索结果排序，目的是帮助用户找到相关、可靠的信息；**Placement is not guaranteed**（展示位置不保证）。[S13]

> 原文：“ChatGPT ranks search results using multiple factors intended to help users find relevant, reliable information. Placement is not guaranteed.” [S13]

**对 GEO 的含义**【推断】：ChatGPT 搜索不是“提交 URL 就收录”的索引系统；没有官方手动提交入口的公开文档。GEO Agent 能做的是确保可抓取、可被第三方搜索提供方发现、内容可被引用，不能承诺“进入 ChatGPT 答案”。

### 2.3 发布者控制与可测量信号

【官方/多语言页】OpenAI 帮助中心的 Publishers and Developers FAQ（多语言页）说明：任何公开网站都可以出现在 ChatGPT 搜索中；要让站点内容被纳入 ChatGPT 的摘要和片段，确保不要阻止 `OAI-SearchBot`，必要时更新 robots.txt 给 `OAI-SearchBot` 访问权限。[S14]

【官方/多语言页】如果 OpenAI 从第三方搜索提供方获得一个被禁止抓取的页面 URL，或通过抓取其他页面获得该 URL，并且有信号表明该页面与用户查询相关，OpenAI 可能在 ChatGPT Atlas 中只展示链接和页面标题；如果不想这样，使用 `noindex` meta tag；但 crawler 必须被允许抓取相关页面，才能读到 meta tag。[S14]

【官方/多语言页】允许 `OAI-SearchBot` 访问内容的发布者，可以使用 Google Analytics 等分析平台跟踪来自 ChatGPT 的引荐流量；ChatGPT 会自动在引荐 URL 中添加 UTM 参数 `utm_source=chatgpt.com`，便于跟踪和分析来自 ChatGPT 搜索结果页的入站流量。[S14]

> 原文（多语言页，英文释义）：“ChatGPT automatically adds the UTM parameter `utm_source=chatgpt.com` to referring URLs, enabling clear tracking and analysis of incoming traffic from ChatGPT search results.” [S14]

**英文页访问异常说明**：2026-11-08 访问 `https://help.openai.com/en/articles/12627856-publishers-and-developers-faq` 时，英文页返回的是 “ChatGPT Atlas retirement” 文章，而不是发布者 FAQ；同一 article ID 的多个其他语言页面仍保留 Publishers and Developers FAQ 内容（页面显示 Updated: 13 days ago）。因此本文对 UTM 等事实引用多语言官方页，并保留此异常说明；引用前应再次核对英文页是否恢复。[S14]

【未发现】截至访问日，在 OpenAI 官方 crawler 文档、ChatGPT search 帮助页、Publishers FAQ 中，没有发现面向发布者的 impressions、引用次数、引用位置、引用份额分析控制台或 API；官方只给出 UTM 引荐流量这一可测信号。[S11][S13][S14]

【未发现】截至访问日，没有发现 OpenAI 官方的“手动提交网站到 ChatGPT 搜索”的公开入口或表单；官方文档描述的路径是允许 `OAI-SearchBot` 抓取并放行 IP 段。[S11][S13]

**对 GEO 的含义**【推断】：本次查阅的资料支持用站点分析中的 ChatGPT UTM 引荐和经验证的爬虫请求日志作为可核对信号；前者不等同官方引荐流量全量，后者是站点侧日志，不是 OpenAI 的引用统计。所查资料未发现发布者展示/引用分析，受控样本或第三方估计出的 ChatGPT 引用份额不能称作平台总体数据。

### 2.4 OpenAI 侧的关键结论

- 【官方】`OAI-SearchBot` 控制搜索展示；`GPTBot` 控制训练；`ChatGPT-User` 是用户主动访问，不是搜索收录控制。[S11]
- 【官方】允许 `OAI-SearchBot` 并放行公开 IP 段；robots.txt 更新约 24 小时生效；被排除仍可能以导航链接出现。[S11]
- 【官方】搜索使用第三方搜索提供方和合作伙伴内容；结果与引用可能不完整、过时或错误；展示位置不保证。[S12][S13]
- 【官方/多语言页】ChatGPT 引荐 URL 带 `utm_source=chatgpt.com`，可用 GA 等分析工具跟踪点击。[S14]
- 【未发现】没有发布者侧 impressions / citation analytics API；没有公开的手动提交入口。[S11][S13][S14]
- 【推断】通过受控样本或第三方方法估计的“ChatGPT 引用份额”，必须记录时间、账号/地区、模型/产品面、查询集、抽取规则和适用范围，不能当作平台总体份额。

---

## 3. 飞书：长连接、事件订阅、机器人、权限、上线

### 3.1 长连接机制与硬约束

【官方】长连接是飞书 SDK 提供的能力：在本地服务器集成飞书 SDK，与开放平台建立一条 WebSocket 全双工通道（服务器需要能够访问公网）；当应用订阅的事件发生时，开放平台通过该通道向服务器发送事件消息。[S15]

【官方】相比传统 Webhook，长连接的优势：无需公网 IP 或域名、无需内网穿透工具；内置通信加密和鉴权逻辑，事件数据在网络传输时加密；开发者无需额外解密、验签，也无需部署防火墙和配置白名单。[S15]

【官方】长连接的硬约束：

- 仅支持企业自建应用；商店应用的事件订阅方式参考“将事件发送至开发者服务器”。[S15]
- 收到消息后需在 3 秒内处理完成且不抛出异常，否则触发超时重推机制。[S15]
- 每个应用最多建立 50 个连接；配置长连接时，每初始化一个 client 就是一个连接。[S15]
- 消息推送为集群模式，不支持广播；同一应用部署多个客户端时，只有其中随机一个客户端会收到消息。[S15]
- 必须确保本地客户端启动正常、有长连接在线，才能在开发者后台保存“使用长连接接收事件”的订阅方式。[S15]

【官方】长连接方式内封装了鉴权逻辑，只在建连时进行鉴权；后续事件推送无需业务侧再处理解密和验签逻辑；传输层仍是加密的。[S15][S16]

【官方】飞书事件订阅 FAQ 补充：长连接不支持配置代理；短时网络中断时，SDK 会自动触发重连；当前不支持查看应用已建立了多少个长连接。[S22]

> 原文：“长连接模式仅支持企业自建应用。” [S15]

> 原文：“长连接模式的消息推送为 集群模式，不支持广播，即如果同一应用部署了多个客户端（client），那么只有 其中随机一个 客户端会收到消息。” [S15]

**对 GEO Agent 架构的含义**【推断】：

- 长连接是“出站 WebSocket”，部署环境只需可访问公网，不需要暴露入站端口；这降低了飞书接入的网络与安全成本。
- 3 秒 ack 与 LLM 推理耗时天然冲突；必须在 handler 内尽快完成解析、落库/入队并返回，把 LLM 调用放到异步 worker，再通过飞书发送消息 API 回复。
- 集群模式随机投递意味着“每个副本都收到同一条事件”不成立；如果多个副本都建立连接，事件可能落到任意一个副本。要么只运行一个 active consumer，要么让所有副本共享同一个持久队列并保证幂等。
- 50 个连接是应用级上限；连接数、重连、发布重启都会占用/消耗连接预算，需要监控。
- 不支持代理；部署环境如果有强制出网代理，需要提前验证，否则长连接不可用。
- SDK 自动重连只覆盖短时网络中断；进程崩溃、发布重启、证书/网络策略变化仍需进程守护和连接健康检查。

### 3.2 事件订阅机制

【官方】事件订阅有两种方式：使用长连接接收事件、将事件发送至开发者服务器（Webhook）。如果已集成飞书 SDK 且开发的是企业自建应用，官方推荐更安全高效的长连接方式。[S16]

【官方】订阅流程：选择订阅方式 → 添加所需事件 → 发布应用使配置生效 → 接收事件。事件订阅方式已经封装鉴权逻辑的，无需解密与验签；Webhook 方式需要按加密策略做安全校验、解密。[S16]

【官方】订阅身份分为应用身份（`tenant_access_token`）和用户身份（`user_access_token`）；大多数事件需要使用应用身份订阅；云文档可按场景选择，日历和邮箱只支持用户身份。[S16]

【官方】事件有 v1.0 和 v2.0 两个版本；v2.0 结构更全面，建议订阅 v2.0；不要同时订阅同一事件的 v1.0 和 v2.0，否则会收到重复事件。[S16]

【官方】事件推送的失败重发机制：接收方需在 3 秒内（TCP 建联超时 2 秒、整体回调超时 3 秒）以 HTTP 200 响应；长连接场景下需 3 秒内处理完成且 SDK 处理函数不抛异常；否则飞书认为推送失败，并以 15 秒、5 分钟、1 小时、6 小时的间隔重推，最多重试 4 次。[S16]

【官方】由于业务链路采取“至少发送一次”策略，即使成功接收也可能收到重复消息；建议对所有消息增加幂等逻辑：v1.0 用 `uuid`，v2.0 用 `event_id` 判断事件唯一性。[S16]

【官方】部分事件是有序事件：前一事件接收成功后才会推送下一事件；有序事件发生阻塞时，后续事件会进入重试队列排队；某个事件是否有序，要看对应事件页面的说明。[S16]

**对 GEO Agent 架构的含义**【推断】：

- 事件处理必须是“至少一次”语义；幂等键要按事件类型选择，不能一概而论。
- 3 秒 ack + 最多 4 次重试（15 秒/5 分钟/1 小时/6 小时）意味着失败恢复窗口很长；业务侧要有死信/补偿机制，不能只依赖平台重试。
- 有序事件如果被阻塞，会拖住后续事件；LLM 长任务不能阻塞有序事件的处理链。
- 事件订阅变更需要发版；上线节奏要和发版审核绑定。

### 3.3 消息事件 `im.message.receive_v1`

【官方】机器人接收到用户/机器人发送的消息后触发 `im.message.receive_v1` 事件；前提是应用开启机器人能力，并在事件订阅中订阅“消息与群组”分类下的“接收消息 v2.0”事件。[S17]

【官方】权限要求（满足任一即可，按接收范围区分）：

- 接收用户发给机器人的单聊消息：需要 `im:message.p2p_msg`（获取用户发给机器人的单聊消息）或 `im:message.p2p_msg:readonly`（读取用户发给机器人的单聊消息）。[S17]
- 接收群聊中 @ 当前机器人的消息：如果需要接收其他机器人发送的消息，需要 `im:message.group_at_msg.include_bot:readonly`（获取群组中其他机器人和用户@当前机器人的消息）；如果只接收用户 @ 的消息、不包含机器人消息，需要 `im:message.group_at_msg`（获取用户在群组中@机器人的消息）或 `im:message.group_at_msg:readonly`（接收群聊中@机器人消息事件）。[S17]
- 接收群聊中所有消息：如果需要包含其他机器人消息，需要 `im:message.group_msg.include_bot:read`（获取群组中用户和机器人消息）；如果只接收所有用户消息、不包含机器人消息，需要 `im:message.group_msg`（获取群组中所有消息，敏感权限）。[S17]
- 同时接收单聊和群聊消息，需要分别申请对应的单聊和群聊权限。[S17]

【官方】可以通过 `sender_type` 字段区分消息发送者是 user 还是 bot；特殊情况下可能收到重复推送，如有幂等需求请使用 `message_id` 去重，不要依赖 `event_id`。[S17]

【官方】接收消息事件支持自建应用和商店应用；事件类型为 `im.message.receive_v1`；推送方式在文档表格中写为 Webhook，但长连接同样是官方支持的事件订阅方式。[S15][S17]

**重要冲突与处理**【官方事实 + 推断】：

- 事件概述文档说 v2.0 事件用 `event_id` 判断唯一性；接收消息事件文档说重复推送时用 `message_id` 去重，不要依赖 `event_id`。两个都是官方页面，但针对的具体事件不同。[S16][S17]
- 对 `im.message.receive_v1`，应以该事件文档的 `message_id` 为准，同时保留 `event_id` 作为辅助追踪字段。[S17]
- 对其他事件，按对应事件页面的说明选择 `event_id`/`uuid`；不要用一个全局规则套所有事件。[S16]

**对 GEO Agent 架构的含义**【推断】：

- 单聊和群聊是两条权限线；GEO Agent 如果既要支持 p2p 问答，又要支持群内 @，必须同时申请 p2p 和 group_at 权限，不能只申请一个。
- 群聊中默认只应接收 @ 机器人的消息；不要为了“多拿上下文”申请 `im:message.group_msg` 敏感权限，除非有明确业务必要和最小化评估。
- `chat_type` 可用于区分单聊/群聊；`sender_type` 可用于过滤 bot 消息，避免机器人互相触发循环。
- 幂等键用 `message_id`；同一个 message 可能因重推被多次投递，业务处理必须去重。

### 3.4 机器人能力与会话形态

【官方】机器人是飞书开放平台的应用能力之一，可以基于会话与用户交互；应用机器人可以结合企业业务系统使用，支持接收消息、发送消息、群管理、卡片交互等；自定义机器人仅支持单向往群组推送消息，不支持与用户交互，也不支持单聊/跨群。[S18]

【官方】应用机器人需要：创建应用、开启机器人能力、申请权限、订阅事件、发布应用并经企业管理员审核；应用配置变化（开启机器人能力、申请 API 权限、订阅事件或回调）需要发布应用并经过管理员审核才能生效。[S18]

【官方】应用机器人可以与在应用可用范围内的用户直接发起单聊；支持创建群组或被添加到指定群组，在群组内发送消息、管理群组；支持外部群和外部用户单聊（需满足飞书认证等条件）。[S18]

【官方】自动回复机器人教程：在群组设置 > 群机器人中搜索应用机器人并添加；用户与机器人单聊可通过搜索机器人名称或 AppLink `https://applink.feishu.cn/client/bot/open?appId={appId}` 打开；登录飞书客户端的用户必须在应用可用范围内。[S23]

**对 GEO Agent 的含义**【推断】：

- 本项目应使用“应用机器人”，不是群自定义机器人；自定义机器人无法接收消息、无法做 p2p、无法做交互。
- 群内使用时，用户需要先把机器人添加进群；p2p 使用时，用户需要在应用可用范围内。产品引导、试用范围和客服话术都要覆盖这两个前提。
- 机器人回复应优先使用回复消息/发送消息 API，并考虑卡片、流式消息等交互形态；但核心不是 UI，而是消息可靠处理。

### 3.5 权限模型

【官方】API 权限（Scope）以应用为维度授予，每个应用的权限独立；调用服务端 API 或监听事件可能涉及企业/用户隐私数据，需要申请相应权限，并由企业管理员审核通过后，应用才能调用 API 或监听事件。[S19]

【官方】权限分为应用身份权限（`tenant_access_token`）和用户身份权限（`user_access_token`）；调用 API 或订阅事件时，要区分用哪种身份；自建应用的权限分为免审权限和需审核权限；需审核权限申请后，需要创建版本并提交审核，管理员审核通过后才生效。[S19]

【官方】权限申请应遵循最小可用原则；无充分理由不要申请大量接口权限；企业自建应用的权限需要租户管理员审核；测试阶段可以使用测试企业/人员，让权限免审生效，但测试企业开通的权限不会生效于正式版本，正式发布时需要再次开通。[S19]

【官方】应用权限、可用范围、访问凭证是三个不同维度：访问凭证识别应用身份；API 权限定义能调用哪些开放能力；可用范围定义哪些用户可以看到/使用应用；此外还有数据权限（行/列权限）。[S19][S20]

**对 GEO Agent 的最小权限建议**【建议】：

- `im:message.p2p_msg:readonly`：读取用户发给机器人的单聊消息。
- `im:message.group_at_msg:readonly`：接收群聊中 @ 机器人的消息。
- `im:message:send_as_bot`：以应用身份发送消息。
- 除非确有需要，不申请 `im:message.group_msg`（群聊所有消息，敏感权限）、`im:message.group_msg.include_bot:read`（包含其他机器人消息）等更大范围权限。
- 如果只做“群内 @ 问答 + 单聊问答”，上述三项通常足够；实际以飞书事件文档的权限要求为准。[S17][S23]

### 3.6 发布上线与可用范围

【官方】当应用的基本信息、权限范围、应用功能、事件订阅等信息发生变更时，都需要发布新的应用版本，并通过企业管理员审核后才能生效。[S21]

【官方】发布流程：在开发者后台的“应用发布 > 版本管理与发布”页面创建版本，填写版本号、更新说明、应用能力/权限变更、可用范围、申请理由，点击“申请线上发布”，等待企业管理员审核；审核通过后，所有 API 权限才正式生效。[S19]

【官方】应用可用范围定义哪些用户群体（组织、用户组、个人）可以使用该应用；应遵循最小可用原则；开发者可以在创建版本时配置可用范围；每次修改可用范围后都需要提交版本审核，通过管理员审核正式上线后，新可用范围才生效；企业管理员可以在管理后台直接调整可用范围，无需发布新版本。[S20]

【官方】如果用户不在应用可用范围内，可能搜索不到应用/无法使用；应用所有者即使被管理员加入禁用成员范围，仍能使用应用；可用成员和禁用成员同时配置时，禁用优先级更高。[S20]

【官方】测试阶段可以创建测试企业，在新企业中创建应用、添加权限，实现权限免审；自动回复机器人教程也建议使用应用所有者身份测试，并确认用户在可用范围内。[S19][S23]

**对 GEO Agent 的含义**【推断】：

- 飞书接入不是“代码上线就生效”；权限、事件订阅、机器人能力等变更需要按平台流程处理发版与审核。可用范围变更路径存在例外：企业管理员可在管理后台直接调整，无需发布新版本。[S20]
- 试用/灰度应使用测试企业或小范围可用范围；正式上线前要预留管理员审核时间。
- 事件订阅变更（例如新增 `im.message.receive_v1`）也需要发版；不能只在开发者后台保存就认为线上生效。
- 可用范围是“谁能用”的硬边界；如果品牌只给某些部门/agency 用，应配置部分成员，而不是全员。

### 3.7 生产运维注意事项

【官方】使用长连接时，如果部署多个实例，实例采用随机选择策略；长连接无环境、机房归属区分，所有 client 权重相同；例如 A 环境机器人发送的消息，回调节点也可能分配至 B 环境。[S22]

【官方】如果部分实例未实现对应事件的 handler，可能触发 500 错误；如果多个 client，必须检查每个 client 是否实现了全部 handler。[S22]

【官方】长连接回调失败报 “app not online” 时，如果是短时网络中断，SDK 会自动触发重连，网络恢复后即可正常使用。[S22]

【官方】长连接出现重复消费通常由两种原因导致：业务 handler 处理逻辑异常并返回错误，触发重推；业务 handler 处理耗时超过 3 秒，触发重推；可通过异步开线程的方式解决。[S22]

【官方】长连接不支持配置代理；当前不支持查看应用已建立了多少个长连接。[S22]

**对 GEO Agent 运维的含义**【推断】：

- 多实例部署不是“天然高可用”：事件随机落到某个实例，若该实例没有 handler 或处理失败，会 500/重推；所有实例必须部署同一套 handler，或者只让一个 active consumer 建立连接。
- 发布重启会导致连接断开和重连；需要优雅停机、连接健康检查、告警和重连观测。
- 没有官方连接数指标，需要自己在进程内埋点（连接建立/断开、最后心跳、重连次数），并与飞书事件日志对账。
- 不支持代理，意味着部署网络要允许直连飞书 WebSocket 域名；容器平台/企业网络要提前做 egress 验证。
- 3 秒内必须 ack；LLM 任务应异步化，并把“已接收/处理中/已完成/失败”状态回写，避免用户重复提问。

### 3.8 飞书侧的关键结论

- 【官方】长连接仅企业自建应用；出网 WebSocket；无需公网 IP/域名；不支持代理；SDK 短时断网自动重连。[S15][S22]
- 【官方】3 秒处理上限；50 连接/应用；集群随机投递、不广播；至少一次投递；重试 15 秒/5 分钟/1 小时/6 小时，最多 4 次。[S15][S16]
- 【官方】`im.message.receive_v1` 需要机器人能力；单聊/群聊 @ 分别申请权限；群聊全部消息是敏感权限；重复推送用 `message_id` 去重。[S17]
- 【官方】权限、事件订阅、应用功能变更需要发版 + 管理员审核；可用范围控制谁能使用。[S19][S20][S21]
- 【官方】多实例随机、handler 缺失会 500、重复消费常见；生产要按“至少一次 + 幂等 + 异步”设计。[S22]

---

## 4. GEO 测量的可审计边界

### 4.1 什么叫“可审计”

本文把“可审计”定义为满足以下任一条件：

1. **平台第一方遥测**：数据由平台官方产品直接提供，且有官方文档定义指标口径（例如 Google Search Console 的 Generative AI performance report）；站点工具统计的 ChatGPT UTM 引荐单列为站点分析。[S6][S14]
2. **站点第一方日志**：站点自己的服务器/CDN 日志记录请求的 user-agent、路径、状态码、时间，并按相应官方方法核对来源 IP/反向 DNS 等，以区分已验证爬虫与可伪造的 User-Agent 声明（例如 Googlebot、`OAI-SearchBot`）。未经核验时只能表述为自报身份的请求。
3. **可核对的配置状态**：robots.txt、`noindex`、Search Console 控制、Merchant Center feed 状态等可以被独立检查、复现和版本化。

4. **受控样本及可复核的第三方材料**：保留采样协议、原始记录、处理版本和可获得的验证材料；只能在其实际覆盖与可复核范围内下结论。

“可审计”不等于“完整”。平台第一方数据往往聚合、延迟、缺少查询词；站点日志经身份验证后只能证明该次请求，不证明收录或引用。受控样本不代表全平台总体；第三方方法不透明时无法独立复核。因此还要区分“事实”“估计”“观点”。

### 4.2 第一方可审计信号矩阵

| 信号 | 第一方来源 | 可审计性 | 能回答的问题 | 不能回答的问题 |
|---|---|---|---|---|
| Google AI Overviews / AI Mode impressions | Search Console Generative AI performance report [S6] | 高：平台官方、按 property/页面/国家/设备/日期聚合 | 哪些页面在 AI 功能中出现、出现趋势、国家/设备分布 | 查询词、引用位置、答案文本、AI 专属 clicks、竞品份额 |
| Google AI 功能点击 | Search Console 总体 Performance 的 Web search type [S1][S6] | 中：包含在总体 Web 数据中，无法单独拆分 | 总体自然搜索点击/转化趋势 | AI 功能单独带来多少点击 |
| ChatGPT 引荐点击 | GA4/站点分析 + `utm_source=chatgpt.com` [S14] | 可核对该工具记录，归因取决于采集与配置 | 带该 UTM 的站点会话、转化、落地页 | 全量 ChatGPT 引荐、平台内展示、引用或答案位置 |
| Googlebot 抓取 | 站点/CDN 日志 + Google crawler 文档 [S3] | 经官方身份验证后，可核对该站请求记录 | 抓取频次、状态码、路径、robots.txt 命中 | 是否被 AI Overviews/AI Mode 引用；未验证前不确认爬虫身份 |
| `OAI-SearchBot` 抓取 | 站点/CDN 日志 + OpenAI crawler 文档/IP JSON [S11] | 核对官方 IP 段等后，可核对该站请求记录 | 搜索爬虫是否访问、访问路径、频率 | 是否被 ChatGPT 引用、引用位置；未验证前不确认爬虫身份 |
| Search Console 收录/展示资格 | Search Console 索引与控制 [S7] | 高：可核对状态 | 是否可索引、是否被 Search GenAI control 排除 | AI 答案中的排名/份额 |
| 内容改动效果 | 站点实验 + 平台数据 | 中：观察性/时间序列 | 改动前后相关性、趋势 | 严格因果（受算法、季节、竞品影响） |
| 第三方 GEO 工具评分 | 工具供应商及其方法资料；Google 关于第三方内部数据声明的边界 [S2] | 取决于方法与原始数据是否可核对；不透明时无法独立复核 | 参考其披露范围内的排序/建议 | Google 平台内部排名或未经核实的真实总体 AI 展示 |

**关键边界**：

- 【官方】Google 明确说第三方工具不能访问其内部排名或 AI 系统；第三方“AI 可见性评分”不是 Google 第一方指标。[S2]
- 【官方】Google 的 AI 报告只给 impressions，维度不含查询词；不要把它包装成“AI 排名”。[S6]
- 【官方】ChatGPT 搜索的展示位置不保证，引用可能不完整/过时/错误；OpenAI 没有发布者侧 impressions/citation 分析。[S13][S14]
- 【推断】Google Analytics 里的 Google organic 点击默认无法区分“来自 AI Overviews”与“来自普通蓝色链接”；除非用实验设计、落地页差异或额外参数，否则不能把全部 Google organic 增长归因于 GEO。

### 4.3 可审计样本与平台总体之间的边界

以下信号在本文查阅的 Google / OpenAI 相关发布者资料中未见完整的第一方明细接口，可通过保留原始证据的黑盒抽样、第三方工具或用户调研估计；可复核样本不等于已知全平台总体。Bing AI Performance 在其支持的产品面提供官方聚合引用指标，Citation Share 的分母限于同一 grounding query 的引用；其引用查询为样本、2026-06 的 Citation Share 为 preview，不能笼统说所有平台没有引用数据。[S27][S28]

- AI 答案中的引用位置（第几个引用、引用段落、引用句子长度）。[S6][S24]
- AI 答案中的“引用份额/ share of voice”（品牌被引用次数 / 总引用次数）。[S6][S24]
- AI 答案中的“推荐率/首选率/正面提及率”。[S6][S24]
- 竞品在 AI 答案中的相对位置与份额。[S6][S24]
- 个性化、登录态、地理位置、语言、设备对答案的影响。[S1][S13]
- 不同模型/版本之间的差异；Google 说 AI Mode/AI Overviews 可能用不同模型，回答会变；OpenAI 说搜索使用第三方提供方，引用可能变化。[S1][S13]

**为什么不能把抽样当成平台真相**：

- 【官方】Google 说 AI Overviews 和 AI Mode 可能使用不同模型和技术，回答和链接集合会不同；AI Overviews 不总是触发。[S1]
- 【官方】OpenAI 说 ChatGPT 搜索使用多个排序因素、第三方搜索提供方，引用可能不完整/过时/错误，展示位置不保证。[S12][S13]
- 【官方】GEO 论文说生成引擎是黑盒、快速变化，内容创作者对内容何时/如何展示“几乎没有控制”；论文的指标是研究构造，不是平台指标。[S24]

### 4.4 学术 GEO 框架的测量方法与边界

【官方/学术】Aggarwal 等人在 KDD 2024 发表 “GEO: Generative Engine Optimization”，把生成式引擎（GE）形式化为“检索 + 生成 + 引用”的系统，提出一套面向生成引擎的 visibility/impression 指标和 GEO-bench 基准。[S24]

【官方/学术】论文的指标：

- **Word Count**：引用相关句子的归一化词数；如果一句话被多个来源引用，词数平均分配给各引用。[S24]
- **Position-Adjusted Word Count**：在 Word Count 基础上，按引用位置做指数衰减加权；越靠前的引用权重越高。[S24]
- **Subjective Impression**：用 G-Eval/GPT-3.5 评估 7 个子维度：引用与查询的相关性、引用对回答的影响、引用材料的独特性、主观位置、主观数量、用户点击引用的可能性、材料的多样性。[S24]

【官方/学术】GEO-bench：10,000 条查询，来自 9 个来源、25 个领域；8K/1K/1K 的 train/validation/test 划分；查询被标注为 7 类；每条查询附 Google 搜索 top 5 结果清理后的文本。[S24]

【官方/学术】实验设置：用 gpt-3.5-turbo 生成回答，temperature=0.7，采样 5 次；在 Perplexity.ai 上用 200 条子集做真实引擎验证；所有指标归一化，使一个回答中所有引用的 impression 之和为 1。[S24]

【官方/学术】结果：最佳方法（Cite Sources、Quotation Addition、Statistics Addition）在 Position-Adjusted Word Count 上相对提升 30–40%，在 Subjective Impression 上提升 15–30%；论文摘要说最高提升 40%；在 Perplexity.ai 上，最佳方法在 Position-Adjusted Word Count 上提升 22%，在 Subjective Impression 上提升 37%；Keyword Stuffing 等传统 SEO 方法表现较差甚至变差。[S24]

【官方/学术】论文明确限制：方法可能需要随生成引擎演化而调整；GEO-bench 查询会过时；由于搜索引擎算法的黑盒性质，论文没有评估 GEO 方法对搜索排名的影响；查询标签可能有主观误差；实验是单轮生成引擎、top 5 来源、特定模型，不等于 Google AI Overviews/AI Mode/ChatGPT 的生产环境。[S24]

**对 GEO Agent 的含义**【推断】：

- GEO 论文的指标可以作为“抽样评估协议”的理论基础，但不能直接当成客户报告里的“AI 排名”。
- 论文的 30–40% 提升是在特定合成引擎/Perplexity 设置下测得的，不能外推为“在 Google AI Overviews 提升 40%”。
- 如果 GEO Agent 采用类似指标，必须公开：查询集、采样次数、模型/产品面、时间、地域、语言、抽取规则和归一化方式；只有抽样设计和统计前提支持时才报告相应置信区间。

### 4.5 建议的测量分层与对外表述规则

**按来源类型分账，而非用 A/B/C/D 排序**【建议】：

| 测量来源类型 `measurement_source_type` | 示例 | 对外表述 | 边界 |
|---|---|---|---|
| `platform_first_party` | Google AI impressions；Bing AI Performance 的引用聚合 | “相应平台报告在指定范围和时段显示……” | 披露平台定义与数据覆盖，不能跨平台合并 |
| `site_log` | 验证身份后的 Googlebot/`OAI-SearchBot` 请求；CDN/WAF 记录 | “经核验的爬虫在 X 时间访问了 Y 路径，状态码 Z” | 仅证明该请求；未经验证的 UA 不得冒称真实爬虫 |
| `site_analytics` | GA4 中 ChatGPT UTM 引荐会话 | “站点分析工具在指定归因规则下记录……” | 非平台总体数据；漏采与归因受配置影响 |
| `consumer_observation` | 固定查询集、时间/地域/语言，重复采样并留存原始答案 | “在指定协议的 N 次样本中，被引用 M 次” | 只代表样本，披露未执行、失败和抽取方法 |
| `api_proxy` | 指定模型/搜索 API 的获授权请求及响应 | “指定 API 配置的样本显示……” | 不代表消费端对应产品答案 |
| `third_party` | 工具商评分、样本及行业报告 | “某第三方按其方法报告……” | 披露来源和方法；不透明时无法独立复核 |

站点分析中的 ChatGPT UTM 会话是站点侧分析数据，并非平台第一方聚合引用报告。按实际数据源保存和展示；不因为 UTM 来自平台就将该数字当作平台总体遥测。以上是测量来源类型，不是知识证据等级；统一知识 `evidence_grade` 为总体计划第 4.5 节的 A 系统记录/批准事实、B 官方政策/批准规则、C 研究参考/假设、D 未核实材料。两者维度不同，测量轨道不自动决定等级。

**对外表述规则**【建议】：

- 可以说：“Search Console 的 Generative AI performance report 显示，某页面在 AI Overviews/AI Mode 中获得 X 次 impressions（国家/设备/日期）。”[S6]
- 可以说：“GA4 中 `utm_source=chatgpt.com` 带来 X 次会话/Y 转化。”[S14]
- 可以说：“在固定查询集、固定时间/地域/语言的 N 次抽样中，品牌被引用 M 次；这是抽样估计。”[S24]
- 不可以说：“ChatGPT 排名第一”“AI 引用份额 X%”而不说明抽样方法。
- 不可以说：“Google 内部 AI 排名”“保证进入 AI Overviews”。
- 不可以说：“`llms.txt` 提升 Google AI 可见性”——Google 明确忽略。[S2]
- 不可以说：“特殊 schema 能提升 AI 排名”——Google 明确说没有特殊 schema 要求。[S2]

### 4.6 待实测清单

【待实测】以下问题官方文档没有给出确定答案，必须在项目环境验证：

1. Search Console API 是否暴露 Generative AI performance report 的数据；官方 Search Console API 文档目前描述的是 searchanalytics 的 web/image/video 等类型，未提及该报告。[S26]
2. Search Console AI 报告是否提供导出之外的 API/批量接口；报告本身支持导出，但 API 是否覆盖待验证。[S6][S26]
3. AI 功能专属 clicks 是否可从 Search Console 或 GA4 中拆分；官方文档只描述 impressions，整体 Web 数据包含 AI 流量。[S1][S6]
4. 品牌站点是否已被 Search Console 收录、是否有 snippet 资格、是否被 Search GenAI control 排除；需要项目侧账号权限。[S1][S7]
5. `OAI-SearchBot` 是否被 CDN/WAF/防火墙放行；需要用日志和 IP JSON 核对。[S11]
6. ChatGPT 引荐 UTM 是否在实际流量中出现；需要在 GA4/站点日志验证。[S14]
7. 飞书长连接在实际部署网络（容器/企业网络/无代理）下能否稳定建立；需要 egress 测试。[S15][S22]
8. 飞书 3 秒 ack 与 LLM 推理的延迟分布；需要压测和异步队列验证。[S15][S22]
9. 飞书多副本部署下的事件随机投递、重复消费、handler 缺失 500 的实际表现；需要故障演练。[S22]
10. 飞书应用发版审核时长、可用范围配置、权限审核的实际租户流程；需要与管理员确认。[S19][S20][S21]

---

## 5. 参考框架的第一手资料

### 5.1 GEO 学术框架（Generative Engine Optimization）

**是什么**【官方/学术】：GEO 是 Aggarwal 等人（IIT Delhi、Princeton 等）在 KDD 2024 提出的、面向生成式引擎的内容可见性优化框架；它把生成引擎视为“检索 + 生成 + 引用”的系统，提出 visibility/impression 指标、GEO-bench 基准和 9 种 GEO 方法。[S24]

**它主张什么**【官方/学术】：

- 生成引擎的引用是内嵌在回答中的，长度、位置、样式不同；传统 SEO 的“平均排名”不适用，需要多维度的 impression 指标。[S24]
- 9 种方法包括 Authoritative、Statistics Addition、Keyword Stuffing、Cite Sources、Quotation Addition、Easy-to-Understand、Fluency Optimization、Unique Words、Technical Terms。[S24]
- 在合成生成引擎和 Perplexity.ai 上，Cite Sources、Quotation Addition、Statistics Addition 表现最好；Keyword Stuffing 表现差。[S24]
- 最佳方法在 Position-Adjusted Word Count 上提升 30–40%，在 Subjective Impression 上提升 15–30%；Perplexity 上分别提升 22% 和 37%。[S24]

**它不主张什么**【官方/学术 + 推断】：

- 没有主张这些方法能提升 Google 搜索排名；论文明确说没有评估对搜索排名的影响。[S24]
- 没有主张 40% 的提升适用于 Google AI Overviews/AI Mode/ChatGPT；实验是特定合成引擎 + Perplexity，模型是 gpt-3.5-turbo，top 5 来源，单轮。[S24]
- 没有提供生产级“AI 引用份额”测量标准；指标是研究构造，需要归一化和人工/LLM 评分。[S24]

**对 GEO Agent 的用法**【建议】：把 GEO 论文当作“抽样评估协议”和“内容实验设计”的参考，而不是“平台排名算法”的说明书；在报告里明确标注“基于 GEO 方法的抽样估计”。

### 5.2 `llms.txt` v2

**是什么**【官方/提案页】：`llms.txt` 是一个提案（proposal），建议在网站根路径或任意子路径放置一个 Markdown 文件，向 LLM/agent 提供简洁的站点背景、指引和指向详细 Markdown 文件的链接；文件包含 H1（必需）、blockquote 摘要、可选详情、H2 文件列表；与 robots.txt、sitemap.xml 共存。[S25]

**版本与作者**【官方/提案页】：llms.txt 提案页（作者署名见原文）；最初 2024-09-03，v2 更新于 2026-08-10；提案页说已有数千站点发布，文档平台自动生成，Chrome Lighthouse 将其纳入 agentic browsing 检查；OpenAI、Anthropic、Gemini 也为自己的开发者文档发布 `llms.txt`。[S25]

**Google 的立场**【官方】：Google Search 不使用 `llms.txt`；Google Search 忽略它；为其他服务维护 `llms.txt` 不会伤害也不会帮助 Google Search 的可见性或排名。[S2]

**OpenAI 的立场**【未发现】：OpenAI 的 crawler 文档和 ChatGPT search 帮助页没有提到使用 `llms.txt` 作为搜索收录或引用依据；不能因为 OpenAI 为自己的开发者文档发布 `llms.txt`，就推断 ChatGPT 搜索会读取第三方站点的 `llms.txt`。[S11][S13][S25]

**对 GEO Agent 的含义**【建议】：

- 如果客户面向的是“agent/文档消费”场景（例如 API 文档、知识库），`llms.txt` 可以作为辅助产物。
- 如果目标是 Google Search / ChatGPT 搜索的可见性，`llms.txt` 不是官方杠杆；不应作为核心交付或 KPI。
- 任何声称“`llms.txt` 提升 Google AI 排名”的说法与 Google 官方指引冲突。[S2]

### 5.3 schema.org/Product 与 Merchant Center

**是什么**【官方】：Product 结构化数据用于在 Google Search（包括 Google Images、Google Lens）中展示商品信息，例如价格、库存、评分、配送；支持 product snippets（非购买页，强调评价）和 merchant listings（购买页，强调商品详情、尺寸、配送、退货等）。[S10]

**Google 的建议**【官方】：可以给网页添加 Product 结构化数据、向 Google Merchant Center 上传 feed 并选择免费 listing，或两者都做；两者同时提供可以最大化商品可见性；某些体验会合并结构化数据和 Merchant Center feed 的数据。[S10]

**与 AI 功能的关系**【官方】：Google 的生成式 AI 回答可能包含商品列表、商品信息；使用 Merchant Center feeds 和 Google Business Profiles 可以帮助商品/服务在 AI 回答和其他 Search 结果中可见；但 Google 同时说，生成式 AI 搜索不需要特殊 schema.org markup，结构化数据不是必需项，继续使用是为了 rich results。[S2][S10]

**对 Momcozy 的含义**【推断】：商品页的 GEO 基础是“可抓取 + 可索引 + 准确的 Product 数据 + Merchant Center feed + 库存/价格/配送一致性”，而不是“AI 专用 schema”；AI 生成商品内容还要遵守 IPTC 元数据和 AI 标注要求。[S9][S10]

### 5.4 Google 对 AEO/GEO 的官方立场

【官方】Google 在生成式 AI 优化指南中直接讨论了 “AEO” 和 “GEO”：这两个术语用于描述专门提升 AI 搜索可见性的工作；从 Google Search 的视角，为生成式 AI 搜索优化就是为搜索体验优化，因此仍然是 SEO。Google 建议，如果考虑第三方 AEO/GEO 建议或服务，要对照其“评估第三方 SEO 建议”的指引。[S2]

【官方】Google 还明确说，很多网上流传的 AEO/GEO “hacks” 并不有效或不受 Google Search 实际工作方式支持；并逐条否定了 `llms.txt`、chunking、为 AI 重写、伪造 mentions、过度结构化数据。[S2]

**对 GEO Agent 的含义**【建议】：产品叙事应与 Google 官方立场对齐：GEO 是 SEO 在生成式搜索体验中的延伸，核心是内容质量、技术可抓取性、结构化商品数据和第一方测量；不是一套“绕过 SEO 的 AI 秘籍”。

### 5.5 各框架之间的关系与冲突

| 框架 | 第一方来源 | 它解决什么问题 | 它不解决什么 | 与其他框架的冲突/边界 |
|---|---|---|---|---|
| Google AI 功能指引 | Google Search Central [S1][S2] | 如何让内容有资格出现在 AI Overviews/AI Mode；如何测量 | 不提供 AI 排名/引用份额；不保证展示 | 明确否定 `llms.txt`、特殊 schema、chunking 等“GEO hacks” |
| OpenAI 搜索爬虫 | OpenAI [S11][S13] | 如何允许/禁止 ChatGPT 搜索抓取；如何跟踪引荐点击 | 不提供 impressions/引用分析；不保证展示 | `OAI-SearchBot` ≠ `GPTBot`；`ChatGPT-User` 不是搜索控制 |
| Search Console GenAI report | Google [S5][S6][S7] | 第一方 AI impressions 与纳入控制 | 不提供查询词/位置/答案文本 | 不能当“AI 排名”；第三方工具无法访问内部指标 |
| GEO 学术框架 | KDD 2024 [S24] | 定义生成引擎 visibility 指标与实验方法 | 不是平台算法；不是生产测量标准 | 合成引擎/Perplexity 结果不能外推到 Google/ChatGPT |
| `llms.txt` v2 | llms.txt 提案 [S25] | 为 agent 提供站点 Markdown 导航 | 不是 Google/ChatGPT 搜索的收录杠杆 | Google 明确忽略；OpenAI 文档未提 |
| schema.org/Product + Merchant Center | Google [S9][S10] | 商品信息在 Search/购物表面的结构化与 feed | 不是“AI 排名”因子 | Google 说 AI 功能不需要特殊 schema；商品 feed 是电商第一方通道 |

**结论**【推断】：这些框架不在同一个层面：Google/OpenAI 是“平台规则与遥测”，GEO 论文是“研究测量方法”，`llms.txt` 是“agent 文档提案”，schema.org/Product 是“商品数据标准”。GEO Agent 应把它们组合成“规则审计 + 内容实验 + 证据分级”，而不是混成一个“AI 排名分”。

---

## 6. 对 GEO Agent / GEO 工作台的关键设计含义

### 6.1 能力边界

**应建能力**【建议】：

1. **抓取与收录审计**：检查 robots.txt、`noindex`、snippet 控制、Search GenAI control、CDN/WAF 对 Googlebot/`OAI-SearchBot` 的放行、Product/Merchant Center 状态。
2. **内容诊断**：基于 Google 官方指引检查内容是否独特、非商品化、有第一手经验、结构清晰、图片/视频、AI 生成内容标注与事实核查。
3. **第一方测量接入**：Search Console Generative AI performance report（impressions）、整体 Web Performance、GA4 `utm_source=chatgpt.com` 引荐、服务器日志中的 crawler 请求。
4. **抽样可见性评估**：固定查询集、固定时间/地域/语言、重复采样、原始回答留存和引用抽取；明确标注样本范围。置信区间仅在抽样设计和统计前提支持时计算。
5. **实验与报告**：内容改动前后对比、控制组、版本记录、证据分级、可复现报告；对外话术自动检查“不得承诺排名”。
6. **飞书交付**：群内 @ / p2p 问答、报告推送、告警、审批；按长连接约束设计异步队列和幂等。

**不应承诺的能力**【建议】：

- 不承诺“Google AI Overviews 排名第一”“ChatGPT 引用份额 X%”“保证被 AI 引用”。
- 不声称能访问 Google/OpenAI 内部 AI 排名或内部指标。
- 不把第三方工具评分当成平台事实。
- 不把 `llms.txt`、特殊 schema、内容切块包装成 Google AI 排名杠杆。
- 不绕过 robots.txt、不绕过 CDN/WAF、不违反平台 ToS 抓取 AI 答案。

### 6.2 数据与证据链

**建议的数据模型**【建议】：

- `measurement_source_type`：`platform_first_party` / `consumer_observation` / `api_proxy` / `site_log` / `site_analytics` / `third_party`；站点 UTM/GA4 等归入站点分析，不伪装为平台聚合报告。
- `evidence_grade`：与总体计划第 4.5 节统一，A 系统记录/批准事实、B 官方政策/批准规则、C 研究参考/假设、D 未核实材料；与 `measurement_source_type` 独立标注，不按轨道自动映射。
- `platform`：Google AI Overviews / Google AI Mode / ChatGPT Search / Perplexity / 其他。
- `query_set_id`、`locale`、`country`、`device`、`sampled_at`、`model_or_product_version`。
- `raw_response_ref`：原始回答/截图的存储引用（注意版权与 ToS）。
- `extraction_rule_version`：引用抽取规则版本。
- `claim_text`：对外结论；必须与 `evidence_grade` 绑定。
- `review_status`：人工复核/自动抽取/待复核。

**报告原则**【建议】：

- 平台第一方数据优先；没有第一方数据时，明确写“抽样估计”。
- 每个数字都要有来源、时间窗、口径、样本量。
- 区分“抓取”“展示”“引用”“点击”“转化”五个环节；不要用一个“可见性”词覆盖全部。
- 对竞品对比，必须说明样本、时间、平台和抽取规则；不能把抽样差异说成平台排名。

### 6.3 飞书接入架构建议

**推荐架构**【建议】：

1. **长连接 connector**：使用飞书官方 SDK（Python/Go/Java/Node）建立 WebSocket；作为独立进程/服务运行；出网直连，不支持代理；进程守护 + 健康检查 + 自动重连。
2. **快速 ack**：handler 内只做校验、解析、幂等检查、落库/入队，3 秒内返回；LLM 调用、工具调用、外部 API 放到异步 worker。
3. **持久队列**：Redis Streams / Kafka / SQS 等；按 `message_id` 幂等；失败重试 + 死信队列。
4. **单消费者/共享队列**：如果多副本都建长连接，事件会随机落到一个副本；要么只让一个 active consumer 建连，要么所有副本共享队列并保证任意副本都能处理。避免“部分副本没有 handler”导致 500。[S22]
5. **回复通道**：worker 通过飞书发送消息/回复消息 API 回复；记录 `message_id`、`chat_id`、`chat_type`、`sender_type`，避免 bot 循环。
6. **权限最小化**：p2p readonly + group_at readonly + send_as_bot；不申请群聊全部消息，除非业务确有必要。[S17]
7. **发版流程**：权限/事件订阅/机器人能力变更走版本发布 + 管理员审核；测试企业灰度；可用范围按部门/用户组配置。[S19][S20][S21]
8. **密钥管理**：App Secret 只放 secret manager/env，不写进代码、文档、日志；本文不记录任何真实凭证。

**关键失败模式**【推断】：

- LLM 超过 3 秒未 ack → 飞书重推 → 用户收到重复回答；必须异步 + 幂等。
- 多副本随机投递 + 某些副本没有 handler → 500/丢事件；必须统一 handler 或单消费者。
- 发版后事件订阅未生效 → 机器人无响应；必须把“发版 + 管理员审核”纳入上线检查清单。
- 代理/防火墙阻断 WebSocket → 长连接不可用；必须提前做 egress 验证。
- 连接数达到 50 上限 → 新 client 无法建立；必须监控连接生命周期。

### 6.4 合规与安全

【建议】：

- 抓取公开网页要遵守 robots.txt、平台 ToS、版权和数据库权利；不要绕过登录、付费墙、验证码、WAF。
- 对 AI 答案做抽样时，注意平台 ToS 是否允许自动化访问；必要时使用人工/授权方式。
- 不存储用户飞书消息中的敏感个人信息超过必要期限；按最小化原则设计留存和脱敏。
- 不把客户站点数据、飞书消息、App Secret 混入公开报告或训练数据。
- 对外报告中的“AI 引用份额”等抽样指标，必须附方法说明和免责声明。

---

## 7. 缺漏与待验证

### 7.1 官方文档缺口

| 缺口 | 现状 | 影响 | 下一步 |
|---|---|---|---|
| Search Console GenAI report 的 API 支持 | 官方 Search Console API 文档描述 searchanalytics 的 web/image/video 等类型，未提及 GenAI report [S26] | 工作台可能只能靠 UI 导出，难以自动化 | 申请 Search Console API 实测；关注官方更新 |
| AI 功能专属 clicks | 官方报告只描述 impressions；整体 Web 数据包含 AI 流量 [S1][S6] | 无法直接算 AI 功能的 CTR/转化 | 分别观察 GA4/落地页；实验只有满足归因前提时才能支持有限推断，不能自动拆出 AI 点击 |
| AI 报告查询词维度 | 报告维度只有 Pages/Countries/Dates/Devices [S6] | 无法知道哪些 query 触发 AI 展示 | 用抽样查询集补足；不能声称平台 query 数据 |
| OpenAI 发布者 impressions/citation 分析 | 未发现官方接口 [S11][S13][S14] | 未发现覆盖总体的官方展示/引用遥测；可保留受控样本证据 | 用抽样；不要把抽样当平台总体数据 |
| OpenAI 英文 Publishers FAQ 一致性 | 英文页访问时返回 Atlas retirement；多语言页仍为 Publishers FAQ [S14] | 引用英文页可能失效/歧义 | 引用多语言页并注明；上线前复核 |
| 飞书连接数可见性 | 官方说不支持查看已建立多少个长连接 [S22] | 运维无法直接对账 | 自建连接埋点 + 飞书事件日志 |
| 飞书代理支持 | 官方说不支持配置代理 [S22] | 企业网络可能无法直连 | 部署前做 egress 验证 |
| 飞书发版审核时长 | 官方文档描述流程，未给统一时长 [S19][S21] | 上线排期不确定 | 与租户管理员确认 SLA |
| 飞书事件有序性 | 官方说部分事件有序，需看具体事件页 [S16] | 不同事件的处理并发策略不同 | 逐事件核对；设计队列分区 |
| GEO 论文的时效性 | 论文基于 gpt-3.5-turbo/Perplexity，2024 年发表 [S24] | 不能直接外推到 2026 的 Google/ChatGPT | 用项目侧抽样校准；记录版本 |
| `llms.txt` 在 OpenAI 搜索中的作用 | OpenAI 文档未提 [S11][S13] | 不能把 `llms.txt` 当 ChatGPT 收录杠杆 | 实测/观察；标注未发现 |
| 第三方 GEO 工具口径 | Google 说第三方无法访问内部指标 [S2] | 若方法不透明，评分无法独立复核；不能冒充平台内部指标 | 只作参考；要求厂商披露方法、数据与样本 |

### 7.2 需要项目侧实测

【待实测】：

- Search Console 账号权限、站点验证、AI 报告是否有数据、能否导出。
- GA4 是否出现 `utm_source=chatgpt.com`；引荐落地页和转化路径。
- 站点服务器/CDN 日志是否记录 Googlebot、`OAI-SearchBot`；WAF 是否放行 OpenAI IP。
- 飞书应用类型、租户、可用范围、权限审核、发版流程；长连接在当前网络是否可建立。
- 飞书 3 秒 ack 的实际延迟；多副本随机投递、重复消费、重连行为。
- 抽样查询集的品牌可见性基线；不同平台/地区/语言的差异。
- 内容改动实验的设计与样本量；避免把季节性/算法更新误判为 GEO 效果。

### 7.3 需要产品决策

【建议讨论】：

- GEO Agent 的“可见性”主指标是 Search Console AI impressions、ChatGPT 引荐点击，还是抽样引用率？三者不能混为一个分数。
- 对客户报告是否允许出现“抽样引用份额”；如果允许，需要怎样的方法说明和免责声明。
- 是否把 `llms.txt`、schema、内容切块作为可选交付；如果做，如何避免客户误解为 Google 排名杠杆。
- 飞书机器人是“问答入口”还是“报告推送/告警入口”；两者对权限、会话、异步处理的要求不同。
- 多租户/多品牌场景下，飞书应用是一个还是多个；可用范围、权限、发版如何隔离。

---

## 8. 来源清单

> 所有来源均为公开页面，访问日期均为 2026-11-08。日期为页面显示的发布/更新日期；未标注日期的，写“未标注”。

| ID | 来源 | 发布/更新 | URL |
|---|---|---|---|
| S1 | Google Search Central, “AI Features and Your Website” | 最后更新 2025-12-10 UTC | https://developers.google.com/search/docs/appearance/ai-features |
| S2 | Google Search Central, “Optimizing your website for generative AI features on Google Search” | 最后更新 2026-07-10 UTC | https://developers.google.com/search/docs/fundamentals/ai-optimization-guide |
| S3 | Google Search Central, “Google's common crawlers” | 最后更新 2026-10-30 UTC | https://developers.google.com/search/docs/crawling-indexing/google-common-crawlers |
| S4 | Google Search Central Blog, “Top ways to ensure your content performs well in Google's AI experiences on Search” | 2025-05-21 | https://developers.google.com/search/blog/2025/05/succeeding-in-ai-search |
| S5 | Google Search Central Blog, “Introducing Search Generative AI performance reports in Search Console” | 2026-06-03；2026-08-31 全球全量 | https://developers.google.com/search/blog/2026/06/gen-ai-performance-reports |
| S6 | Google Search Console Help, “Generative AI performance report (Search)” | 2026-08-31 全球全量 | https://support.google.com/webmasters/answer/16984139 |
| S7 | Google Search Console Help, “Search generative AI control” | 2026-08-31 全球全量 | https://support.google.com/webmasters/answer/16908024 |
| S8 | Google Blog, “New opportunities, control and insights for website owners” | 2026-06-03 | https://blog.google/products-and-platforms/products/search/new-controls-website-owners/ |
| S9 | Google Search Central, “Google Search's guidance on using generative AI content on your website” | 最后更新 2026-10-01 UTC | https://developers.google.com/search/docs/fundamentals/using-gen-ai-content |
| S10 | Google Search Central, “Product structured data” | 最后更新 2025-12-10 UTC | https://developers.google.com/search/docs/appearance/structured-data/product |
| S11 | OpenAI, “Overview of OpenAI Crawlers” | 未标注 | https://developers.openai.com/api/docs/bots |
| S12 | OpenAI, “Introducing ChatGPT search” | 2024-10-31；2025-02-05 更新 | https://openai.com/index/introducing-chatgpt-search/ |
| S13 | OpenAI Help Center, “Searching the web with ChatGPT” | 页面显示 Updated: 19 days ago（访问日 2026-11-08） | https://help.openai.com/en/articles/9237897-searching-the-web-with-chatgpt |
| S14 | OpenAI Help Center, “Publishers and Developers FAQ”（多语言页；英文页访问异常） | 页面显示 Updated: 13 days ago（访问日 2026-11-08） | https://help.openai.com/sk-sk/articles/12627856-publishers-and-developers-faq |
| S15 | 飞书开放平台, “使用长连接接收事件” | 最后更新 2026-06-11 | https://open.feishu.cn/document/server-docs/event-subscription-guide/event-subscription-configure-/request-url-configuration-case?lang=zh-CN |
| S16 | 飞书开放平台, “事件概述” | 未标注 | https://open.feishu.cn/document/server-docs/event-subscription-guide/overview?lang=zh-CN |
| S17 | 飞书开放平台, “接收消息” | 最后更新 2026-08-05 | https://open.feishu.cn/document/server-docs/im-v1/message/events/receive?lang=zh-CN |
| S18 | 飞书开放平台, “机器人概述” | 未标注 | https://open.feishu.cn/document/client-docs/bot-v3/bot-overview?lang=zh-CN |
| S19 | 飞书开放平台, “申请 API 权限” | 未标注 | https://open.feishu.cn/document/server-docs/application-scope/introduction?lang=zh-CN |
| S20 | 飞书开放平台, “配置应用可用范围” | 未标注 | https://open.feishu.cn/document/develop-process/test-and-release-app/availability?lang=zh-CN |
| S21 | 飞书开放平台, “企业自建应用开发流程” | 未标注 | https://open.feishu.cn/document/develop-process/self-built-application-development-process?lang=zh-CN |
| S22 | 飞书开放平台, “常见问题”（事件订阅） | 未标注 | https://open.feishu.cn/document/uAjLw4CM/ukTMukTMukTM/event-subscription-guide/event-card-faq?lang=zh-CN |
| S23 | 飞书开放平台, “三分钟快速开发” | 未标注 | https://open.feishu.cn/document/develop-an-echo-bot/introduction?lang=zh-CN |
| S24 | Aggarwal et al., “GEO: Generative Engine Optimization”, KDD 2024, arXiv:2311.09735 | 2024-06-28（arXiv 页面）；KDD 2024-08-25/29 | https://arxiv.org/abs/2311.09735；HTML: https://arxiv.org/html/2311.09735v3 |
| S25 | llms.txt 提案页, “The /llms.txt file, v2” | 最初 2024-09-03；v2 更新 2026-08-10 | https://llmstxt.org/ |
| S26 | Google Search Console API, “Getting your performance data” | 未标注 | https://developers.google.com/webmaster-tools/v1/how-tos/all-your-data |
| S27 | Bing Webmaster Blog, “Introducing AI Performance in Bing Webmaster Tools Public Preview” | 2026-02-10 | https://blogs.bing.com/webmaster/2026/2/Introducing-AI-Performance-in-Bing-Webmaster-Tools-Public-Preview/ |
| S28 | Bing Search Blog, “New AI Visibility Insights in Bing Webmaster Tools: Intents, Topics, Citation Share, Compare” | 2026-06-16 | https://blogs.bing.com/search/2026/6/New-AI-Visibility-Insights-in-Bing-Webmaster-Tools-Intents-Topics-Citation-Share-Compare/ |
| S29 | Bing Webmaster Blog, “Introducing Topics Portfolio (Beta) in AI Performance” | 2026-10-13 | https://blogs.bing.com/webmaster/2026/10/introducing-topics-portfolio-beta-in-ai-performance/ |

**来源使用说明**：

- S14 为官方帮助中心多语言页；英文页在 2026-11-08 访问时返回另一篇文章（ChatGPT Atlas retirement），故正文对 UTM 等事实引用多语言页并保留异常说明。
- S5/S6/S7 的“2026-08-31 全球全量”来自页面注记，不是文章发布日期；文章发布日期为 2026-06-03。
- S27 中的 AI Performance 为 2026-02 public preview；S28 的 Citation Share 等为 2026-06 preview；S29 的 Topics Portfolio 为 2026-10 beta，主题数量为近似值。平台功能及实际可用性在接入前重新核查。[S27][S28][S29]
- S24 的 arXiv 页面显示 2024-06-28；KDD 2024 会议日期为 2024-08-25 至 2024-08-29。
- 飞书部分页面为 JS 渲染，curl 直接抓取只能得到壳；本文使用官方页面内容与搜索引擎抓取的官方正文交叉核对，日期以页面显示“最后更新于”为准，未显示的标注“未标注”。

---

## 附录 A：状态标记与引用规则（速查）

- **【官方】**：官方原文明确；可直接引用，但需带 URL 与访问日期。
- **【官方/多语言页】**：官方帮助中心多语言页；英文页可能不一致，引用时注明。
- **【推断】**：由官方事实推导，不是官方原话；对外报告应标注“推断”。
- **【待实测】**：官方文档未给确定答案；必须在项目环境验证。
- **【未发现】**：截至访问日在官方文档中未找到；不等于官方否认，不能反推为“平台不支持”。
- **【建议】**：本研究的产品/架构建议，不是平台规则。

## 附录 B：本次研究的研究卫生

- 仅只读访问公开官方文档；未登录飞书、未调用飞书 API、未修改任何飞书配置。
- 未采集、未写入任何 App ID、App Secret、token、cookie、密钥或私有资料。
- 未向外部系统写入数据；本文是唯一交付物。
- 未使用 `pmo.luteos.com`、`main.luteos.com` 或 Momcozy 私有文档；这些属于后续研究/项目侧输入。
- 所有 URL 均为公开可访问页面；如遇地域/登录限制，以官方文档的公开镜像或多语言页交叉核对。
