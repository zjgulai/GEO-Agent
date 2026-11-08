# Momcozy GEO 试点：问句、采样与指标讨论稿

> 状态：**L0 讨论稿**，2026-11-08。本文是测量设计，不表示已执行采样、冻结样本、批准预算或具备生产能力。文中英文问句和数字均为候选或玩具示例。

## 1. 已同意的首期讨论基线

- 品牌、市场、语言：Momcozy / 美国 / 英语；品类：吸奶器。
- 首选产品面：Perplexity 消费端；候选 3 个 ASIN，具体商品和最终数量须由业务根据资料与条件确认。
- 人工观察及导入只在获得充分授权的条件下作为兜底考虑；**目前 Perplexity 消费端系统性商业采样、复制、截图与导出为 HOLD，见第 2 节**。
- 先核验 PMO 现有 GEO 资产，再决定承接或扩展；首期只生成内部候选任务，不默认真实跨系统派单。
- **测量讨论方向**：用户于 2026-11-08 同意以无品牌发现为首要视角，同时保留来源引用与产品事实核查，继续讨论下一节点；这不等于逐条冻结具体问句、指标字典、样本量或执行方案。

详见[总体计划](./2026-11-08-geo-agent-workbench-master-plan.md)。范围讨论不等于采样、付费调用、飞书接入、实施、上线或外部写入授权。

## 2. 先处理平台许可门：当前 HOLD

### 2.1 已核对的一手条款与适用性

【B：官方政策，非项目许可证明】2026-11-08 只读核对 [Perplexity Terms of Service](https://www.perplexity.ai/hub/legal/terms-of-service)，页面标注更新于 2026-01-23：

- 第 5.1 节将服务使用许可描述为“personal, non-commercial use only”。
- 第 5.2(a) 涉及复制、传播、展示等信息使用限制；第 5.2(e) 涉及商业目的；第 5.2(i) 涉及以自动工具或人工流程进行监测、提取、复制或收集信息。5.2 条款有适用法律或法规及书面许可等例外条件；具体以完整原文为准。
- 条款适用于哪个具体产品、账号、协议和活动，以及企业合同、附加协议、单独书面许可或适用法律是否改变结论，**尚未核实**。本段不是法律意见，不断言所有人工阅读均被禁止。

**项目处理**：在法务/平台 owner 明确适用协议、拟开展的商业测量和允许的数据操作，并获得必要的平台许可及单独项目执行授权之前，系统性消费端采样和复制、截图、导出保持 HOLD；不得靠人工操作、少量记录或其他形式规避限制。已有历史数据的再利用也须核验其来源、权利与适用许可。禁止绕过登录、验证码、地区或其他访问控制。

### 2.2 可并行推进及替代轨道

- **可继续设计**：问句和指标评审、样本量估算、合成 fixture、事实表与标注手册草案；这些都不要求真实平台观察。
- **先核验再决定**：由业务和法务核查现有企业合同或书面许可是否明确覆盖产品面、商业用途、手动/自动采样、保留及共享范围，再由业务 owner 单独批准预算与执行窗口。
- **其他来源独立处理**：经授权的平台第一方聚合、站点数据或 API 均有各自的条款与指标；是否接入须另行审批。`api_proxy` 即使可用也不能伪装为消费端回答，不能把不同平台数据混算。
- 获授权的人工消费端观察可以形成**限定于该协议及样本**的基线；但它不是自动化采样，不代表所有用户或平台总体。

## 3. 候选问句库：先分意图，再关联商品

【C：设计草案】下面 20 条由项目研究材料启发并经重新编写，属于专家拟写候选，不冒充真实用户问题或全市场代表性问题。对照 [Momcozy SEO→GEO 机会材料](../../../reseach/Momcozy_SEO_to_GEO_Opportunities_2026-10-11.md)、[第一手研究](../../research/2026-11-08-geo-first-party-research.md)及总体方案；最终需结合获授权的真实搜索/客服资料、业务与合规审核。PMO 提到的“20 问/ASIN”是当前页面方向，是否为硬门槛尚待 owner 确认。

依据用户同意的**无品牌发现优先**总体方向，本稿建议将 U、S 的无品牌选购题作为发现型指标候选分层，比较和品牌型号事实分账；U/S 分层、具体题目和 KPI 字典仍需逐项审核，不能视为已冻结。场景题有场景偏置，应同时提供 U、S 单独结果，不能将其称为所有消费者的自然提及率。每条题发送前均需冻结版本、适用性、关联对象与执行许可；下表不能直接作为执行清单。

### 3.1 无品牌发现 U：6 条共享候选

| ID | 英语问句候选 | 观察目的 |
|---|---|---|
| U01 | What breast pumps should a first-time parent consider buying in the United States? | 首次购买候选 |
| U02 | Which breast pump brands should I consider when comparing options in the United States? | 品牌选择候选 |
| U03 | What wearable breast pumps are worth considering for everyday use in the United States? | 可穿戴品类发现 |
| U04 | What electric breast pumps are worth considering for regular use in the United States? | 电动品类发现 |
| U05 | Which breast pumps are commonly recommended for people returning to work in the United States? | 职场购买发现 |
| U06 | What are some recommended breast pumps available in the United States? | 泛品类推荐发现 |

U05 与场景题可能语义近似，应由业务评审保留或替换；“commonly recommended”是问题措辞，不证明回答中的流行程度。用户意图为了解规格或学习方法的问句可另入教育分层，不应机械地用是否提及品牌衡量其质量。

### 3.2 比较 C：4 条候选

| ID | 英语问句候选 | 适用性 |
|---|---|---|
| C01 | How do wearable breast pumps compare with traditional double electric breast pumps for daily use? | 共享、无品牌；品类权衡，**不纳入发现型提及主分母** |
| C02 | Which breast pump is the best overall choice for a first-time parent in the United States? | 共享、无品牌；候选 Best Overall 问句，品牌发现与最佳选择分别编码 |
| C03 | How does the Momcozy <model> compare with other <product category> breast pumps? | 具名型号比较；按批准的型号及类别实例化，不纳入无品牌发现 |
| C04 | What are the tradeoffs between the Momcozy <model> and the <competitor model> breast pump? | 具名竞品比较；型号、竞品及可比性均须批准 |

### 3.3 无品牌场景 S：5 条共享候选

| ID | 英语问句候选 | 场景 |
|---|---|---|
| S01 | What breast pumps should I consider if I need to pump during short breaks at work? | 职场短时使用 |
| S02 | What breast pumps should I consider for overnight pumping at home? | 夜间使用 |
| S03 | What breast pumps should I consider for pumping while traveling? | 出行 |
| S04 | What breast pumps should I consider if portability is important to me? | 便携诉求 |
| S05 | What breast pumps should I consider for use in a small shared living space? | 小空间 |

题目中的场景不代表产品已被证实适用；不得从 AI 答案直接推断噪音、安全、清洁效果或其他产品事实。

### 3.4 具名型号事实 F：5 条候选

| ID | 英语问句候选 | 人工核验方向 |
|---|---|---|
| F01 | What are the official specifications for the Momcozy <model> breast pump? | 美国市场生效规格 |
| F02 | Does the Momcozy <model> breast pump support <feature>? | 一次核验一个已批准的功能 |
| F03 | What does the manufacturer say about cleaning the Momcozy <model> breast pump? | 现行官方说明；健康/消毒声明专项复核 |
| F04 | What is included in the box with the Momcozy <model> breast pump? | 美国地区具体版本与包装清单 |
| F05 | What warranty and customer support options does Momcozy provide for the <model> breast pump in the United States? | 当期美国保修及支持条款 |

尖括号是**本稿的占位标记，不是可发送文本**。须由事实 owner 提供经过批准的市场、型号、版本、功能和竞品；无法准确区分 ASIN 变体时保留 `unknown`，不能将同名型号自动映射为某个 ASIN。清洁、健康、安全、疗效、保险及监管声明须按适用证据与合规规则处理；缺事实、过期或冲突时不判对错。

### 3.5 共享问题与计划数：只作算术示例

假设 3 个 ASIN 都适用同样 13 条共享题（U 6 + C01–C02 2 + S 5），其余 7 条（C03–C04、F 5）分别生成各 ASIN 独有且互不相同的文本，且每题重复 2 次：

- ASIN—问句**关联行**：3 × 20 = 60；不是 60 条独立问题。
- **唯一问句**：13 + 3 × 7 = 34；**计划观察项**：34 × 2 = 68。
- 若实际型号共用文本、某题不适用、占位符未获批准或有其他去重结果，34 和 68 都会改变。此处不是获批的样本量、预算或实测数据。

共享问题与 ASIN 是多对多关系；一次回答不会因关联 3 个 ASIN 就计成 3 次品牌观察。具名品牌不自动等于点名某个型号或 ASIN；ASIN 层结果只能在对应关系有充分依据时表达，不能把共享样本冒充各商品的独立样本。

## 4. 观察协议与记录契约（实施条件成立后）

### 4.1 冻结内容

1. **问句集**：精确文本、`prompt_version`、意图/品牌显式性、关联 ASIN、适用性、排序或预设随机种子、审批人与生效版本。文本变化产生新版本。
2. **产品面**：消费端 Web 或 App、入口、实际模式/模型选项、是否登录/订阅、来源面板能力、语言、设备、采样窗口与可合法确认的实际地区。未知的产品/模型/UI 信息记 `unknown`，不推测。
3. **会话**：每次重复建议新开对话；记录 `session_id`、上下文、账号历史/个性化状态及重复间隔。新线程可减少显式上下文污染，但不保证账号、模型或时间层面的统计独立。
4. **证据**：仅在适用协议和许可明确允许的范围内保存原始文本、引用及截图等；指定保留期限、访问权限、敏感信息处理、不可留存的替代处置和审计。记录实际执行时间、执行者、入口、授权依据和状态。许可不足不得降级为绕过式采样。
5. **行为**：按冻结问句和顺序操作；额外追问或模式变更作为不同协议记录，不覆盖先前观察；不通过冒用地区或绕过访问控制构造样本。

### 4.2 实体、去重与重试

- `PlanVersion` 定义一次测量的协议、问句及环境；`PlannedObservation` 是唯一问句版本 × 固定入口/地区/账号策略 × 重复序号；重复序号在同一计划内唯一。`PromptASINLink` 是多对多元数据，不是新观察。
- 一项可有多个 `Attempt`：仅按**预先审批的失败重试策略**重试；每次记录原因、时间、成本和结果。计划项按预先冻结规则选定结果；若重试成功，尝试层依然保留先前失败，不在计划项分母重复计算。有效结果不能因“不理想”被重试替换。
- `Capture` 的来源身份、元数据与内容哈希用于发现同一执行被重复导入；哈希相同不单独证明是同一次执行。独立重复出现相同答案仍保留两次，并标记相同内容，不能按结果选择性删除。
- 协议不合格或证据不足标 `invalid`；若决定补采，事先依据治理规则新建并关联补采计划，原记录不消失；不得静默把 invalid 改写成成功。

### 4.3 截止时状态守恒

`planned` 是冻结计划全集，**不是互斥状态**。报告截止时每项恰属于以下一种：

| 状态 | 说明 | 是否进入回答质量分母 |
|---|---|---|
| `not_run` | 尚未开始，含许可或预算阻断 | 否 |
| `in_progress` | 已开始但截止时尚无终态 | 否 |
| `failed` | 尝试后无可用结果，记录原因 | 否 |
| `invalid` | 有结果但违反已冻结协议或证据不足 | 否 |
| `valid_answer` | 符合协议且有可判断的有效回答 | 按各指标进一步判定 |
| `valid_no_feature` | 对协议明确定义且适用的目标功能，确认其未出现 | 否；可分析未触发本身 |

`valid_answer` 指达到回答/证据层面的有效条件；某一指标仍可为 `unknown` 或 `not_applicable`。Perplexity 具体入口是否存在可定义的 `valid_no_feature` 必须先确认；不适用时标 N/A，不冒报 0%。冻结前即不适用的问句不纳入 `planned`；冻结后的变更留痕，不能按观察结果剔除。

```text
planned = not_run + in_progress + failed + invalid + valid_answer + valid_no_feature
执行终态 = failed + invalid + valid_answer + valid_no_feature
```

## 5. 指标字典 v0：仅为讨论建议

每个指标固定**单位、分子、分母、适用性、未知值及版本**。先按平台/产品面、来源轨道、市场、问句类别和窗口分层，再考虑是否汇总；不可将具名问题与无品牌发现题混成头条。分母为零时显示 N/A，不显示 0%。

| 指标 | 单位与建议口径 | 排除与备注 |
|---|---|---|
| 计划项执行终态率 | `执行终态 / planned` | 流程状态，不代表质量；`not_run`、`in_progress` 单列 |
| 答案或未触发覆盖率 | `(valid_answer + valid_no_feature) / planned` | 不意味着所有项目都有可分析答案 |
| 有效回答覆盖率 | `valid_answer / planned` | 质量指标的起始集合，不等于每项指标都可判定 |
| 功能未触发率 | `valid_no_feature / planned` | 只在入口适用时计算；不表示未提及品牌 |
| 无品牌发现型提及率 | **回答级**：预先批准的 U/S 发现题中，实质提及 Momcozy 的可判断有效回答数 / 同一层可判断有效回答数 | `unknown`、N/A 单列；具名问题、失败、未触发不得当负样本；别名须消歧 |
| 官方来源引用率 | **回答级**：至少一次实际引用已核实品牌官方来源的可判断回答数 / 引用可判断的有效回答数 | 官方域名映射须版本化；品牌自有站、品牌店铺/零售 PDP、第三方分开；仅来源列表和正文实际引用分开；来源不可判为 unknown |
| 引用声明支持度 | **声明—引用对**：来源充分支持的已判定配对数 / 已判定配对数 | 部分支持、不支持、反驳、unknown 分别报；一次回答可有多对，不能当回答级比例 |
| 型号事实准确率 | **声明级**：按观察时点、美国市场、型号及指定知识截止时间可用的适用事实判为正确的声明数 / 可判定声明数 | 事实缺失/冲突/过期为 unknown；另报可核对声明的回答覆盖，不能说成所有回答准确率。区分当时已获批准的判定与后来补证重评 |
| Best Overall 观察率 | **适用回答级**：预先批准的比较题中，明确将目标品牌或型号作为唯一总体最佳/首选的可判断回答数 / 可判断的适用回答数 | 可判断分母含“无明确最佳”；其他品牌、并列、条件最佳、ambiguous 单列；品牌级与型号级不混同；列表首位不自动算 |

对每个指标，适用的有效回答按 `judgeable / unknown / not_applicable` 划分，保留原因和数量。问句、回答、声明、声明—引用对、ASIN 是不同单位；一个指标的分母不能换成另一个单位的计数。比率旁总报 `x/n`、分层、失败和 unknown；不保证平台总体覆盖。

### 5.1 玩具示例：只用于复核计算

假设计划 10 项，截止时：`valid_answer=5`、`valid_no_feature=1`、`invalid=1`、`failed=1`、`not_run=1`、`in_progress=1`。合计 10。

- 执行终态率 = `(5+1+1+1)/10 = 8/10`；答案或未触发覆盖率 = `6/10`；有效回答覆盖率 = `5/10`。本例不意味着 Perplexity 的 `valid_no_feature` 一定适用。
- 假设 5 个有效回答中，有 4 个属于预注册的无品牌发现层；其中 3 个可判断、1 个 unknown；3 个可判断中 2 个提及：报告 **2/3**，并列 `unknown=1`。不能写成 2/4、2/10 或平台整体比例。
- 上述全部数字为人工构造的示例，**不是 Momcozy 或 Perplexity 的真实结果**。

## 6. 标注、事实与复测治理

### 6.1 标注 SOP

- 建立带正反例的品牌别名/歧义、正文引用、声明拆分、事实适用性与 Best Overall 标注手册；先在获准使用的合成 fixture 预演，不以未经批准的数据试采。
- 小样本建议双人独立标注并仲裁分歧；若资源不足，预先确定复核比例与抽样，单独报告覆盖、原始一致率和分歧数。单人审阅不得称作人工金标；盲法可行时隐藏期别和干预假设。
- 观察记录可证明“按记录在该时点看到了什么”，不自动证明模型回答中的产品声明为真。事实判断须链接该市场/型号适用、观察时点生效且在指定知识截止时间内已获适用批准的事实，并区分当时判定与后来补证重评；测量来源 `measurement_source_type` 与知识 `evidence_grade` 分开。
- 缺原始来源、引用关联不清、官方归属不明或批准事实不可用时使用 `unknown` 及原因码；健康、安全、报销、疗效与监管声明转合规 owner。

### 6.2 复测与比较门禁

- 预先定义**共同核心问句**与观察匹配键；同轨道、产品面、市场/语言、模式、账号/设备策略、时间窗口定义、重复规则、标注与官方映射版本应一致，变化须留痕。
- 同时展示基线和复测的有效数量、失败/无效/unknown、共同匹配项及差异流失；新增或移除的问题单列。对匹配子集和各期完整样本分别标口径，不能隐藏缺失造成的偏差。
- 记录页面变更、外部事件及平台 UI/模型变化和实施时间；若协议或平台变化导致不可直接比较，分期展示并标记原因，只有经审批的方法才能做桥接。
- 初期是目的性小样本，不声称统计独立、平台总体比例、显著性或 SEO 改动的因果效果；若要估计总体或区间，需另设抽样框、聚类/依赖处理与统计方案。

## 7. 进入执行前的责任与门禁

| 责任方 | 待完成事项 | 缺失时处理 |
|---|---|---|
| 业务/产品 owner | 确认 KPI 优先级、具体 ASIN、问句意图与试点边界 | 讨论稿不冻结 |
| 平台 owner + 法务/合规 | 核实 Perplexity 适用合同、书面许可、商业用途、手动/自动与数据留存范围 | 消费端采样及受限复制 HOLD |
| 商品事实 owner | 提供美国市场、具体版本生效事实和证据锚点 | 型号判断为 unknown；不生成无依据声明 |
| GEO/SEO 与数据负责人 | 审核问句、抽样预算、标注规则、去重、统计与复测协议 | 不批准计划/报告口径 |
| 项目负责人 | 审核成本上限、执行窗口、人员与明确执行授权 | 不采样、不付费调用 |
| PMO/系统 owner | 核验现有资产、字段、权限和 `ai_answer_snapshot_id` 契约 | 不宣称复用或完成跨系统交接 |

**下一步建议**：按已同意的无品牌发现优先方向，继续开展许可核查、商品事实盘点及问句业务评审。所有外部采样、写入和真实调用均需各自通过许可和授权门。
