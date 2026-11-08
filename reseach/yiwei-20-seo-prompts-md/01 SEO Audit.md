01 SEO Audit

【工作流定位】
- 能力定位：全站基线诊断
- 上游输入：当前 URL、GSC/GA4/日志/站点导出、重点模板样本
- 本 Prompt 输出：问题清单、证据、优先级和首轮修复建议
- 下游衔接：02、03、08、09、10、11、12、13、16、17、18、19、20

你是一名资深技术 SEO 顾问。请对下面的网站执行一次基于真实网页证据的 SEO 审计，并生成一份专业、可下载的 PDF 报告。
【审计对象】
目标网址：[https://example.com/]
主要市场：[中国 / 美国 / 全球]
目标语言：[简体中文 / 英文]
主要关键词：[填写关键词；不知道则由你根据页面内容推断]
审计范围：[单个页面 / 全站]
最大抓取页面数：[20]
报告语言：[简体中文]
【重要规则】
必须实际访问并检查网站，不要只根据域名或页面摘要判断。
所有结论都必须附上证据，包括实际 URL、抓取到的标签内容、数量、状态码或测试结果。
不得虚构数据。无法检测的项目标记为“N/A”，并说明原因和人工检测方法。
区分“已确认的问题”“可能的问题”和“优化建议”。
不要输出空泛的 SEO 常识；建议必须针对这个网站。
如果 PageSpeed Insights、Lighthouse 或其他接口限流/失败，记录错误，并提供手动测试链接。
审计以当前可公开访问的网页为准，不要求登录后台。
【需要检查的项目】
一、抓取与索引
HTTP 状态码、HTTPS、HTTP→HTTPS 跳转
www 与非 www 版本是否统一
robots.txt 是否存在、是否误拦截
XML Sitemap 是否存在、格式是否正常
robots meta 与 X-Robots-Tag
canonical 标签是否存在、自引用且与最终 URL 一致
尾斜杠、参数、大小写及重复 URL 问题
3xx 跳转、4xx/5xx 错误和重定向链
可索引性判断
二、页面 SEO
Title：实际内容、字符数、关键词、唯一性及修改建议
Meta Description：实际内容、字符数、搜索意图、CTA 及修改建议
H1：数量、内容、语言和关键词匹配
H2/H3：层级结构、主题覆盖和缺失情况
HTML lang 属性、页面实际语言及二者是否一致
hreflang（如果是多语言网站）
URL slug 的可读性和关键词相关性
正文主题、搜索意图、内容深度及关键词使用
Open Graph 和 Twitter Card
图片 alt、图片尺寸和懒加载
内部链接、外部链接和无效链接
结构化数据/Schema 类型、有效性和缺失项
三、性能与体验
分别检查移动端和桌面端 PageSpeed/Lighthouse
Core Web Vitals：LCP、INP、CLS
FCP、TBT、Speed Index
移动端适配、viewport 和可读性
主要性能瓶颈与可执行的修复建议
四、内容与搜索机会
推断页面主要关键词和搜索意图
判断 Title、Description、H1、正文语言及关键词是否一致
找出主题覆盖缺口
提供 3–5 个相关关键词或长尾关键词建议
提供可直接替换的 Title、Meta Description 和 H1
如果信息不足，明确标记为推断，不要假装拥有搜索量数据
【每项检查使用统一状态】
PASS：符合要求
WARNING：可改进或存在潜在风险
FAIL：确认存在明显问题
N/A：无法验证或不适用
【报告结构】
封面
“SEO Basic Audit Report”
网站名称、目标 URL、审计日期
审计范围与报告语言
Executive Summary
总体 SEO 健康度评分：0–100
PASS、WARNING、FAIL、N/A 数量
一段不超过 150 字的管理层摘要
Page Checks 总表
使用以下列：
检查项目 | 状态 | 实际证据 | 问题说明 | 修复建议
至少包含：
PageSpeed Mobile、PageSpeed Desktop、URL、Title、Meta Description、
H1、Heading Structure、Canonical、Robots、Sitemap、Language、Hreflang、
Image Alt、Internal Links、Schema、Mobile Friendly、Indexability。
Priority Actions
按照影响和紧迫程度列出优先事项：
P0：阻碍抓取、索引或排名的严重问题
P1：高影响，建议立即处理
P2：中等影响
P3：优化项
每项必须写明：
问题 → SEO 影响 → 具体修改方式 → 预期效果 → 验证方法
Insight Walkthrough
对每个 FAIL 和重要 WARNING 单独展开，格式为：
严重程度
问题标题
Evidence：实际检测证据
Why it matters：为什么影响 SEO
Fix：可直接执行的修复方法
Example：修改后的标签、文案或代码示例
Validation：修复后如何验证
推荐文案
给出可直接使用的：
推荐 Title（控制在约 50–60 个英文字符；中文按搜索展示合理控制）
推荐 Meta Description
推荐 H1
建议的 H2/H3 结构
推荐 canonical 标签
必要时给出 hreflang 和 Schema JSON-LD 示例
修复路线图
整理为表格：
优先级 | 任务 | 负责人建议 | 工作量（低/中/高） | 预期影响 | 验收标准
审计限制
列出接口失败、反爬限制、登录限制、未获得 Search Console/Analytics
数据等情况，并注明哪些结论需要人工复核。
【视觉与交付要求】
报告风格简洁、专业，参考专业咨询公司的 SEO 审计报告。
使用蓝色章节标题、浅灰表格边框。
PASS 使用绿色，WARNING 使用橙色，FAIL 使用红色，N/A 使用灰色。
长内容自动换页，不允许表格文字被截断或重叠。
URL 必须可以点击。
同时输出：
Markdown/网页预览版
完整 PDF 文件，文件名为：SEO-Audit-[域名]-[日期].pdf
生成后检查 PDF 的分页、表格、字体和中文显示是否正常。
现在开始审计该网址：[https://example.com/]
你是一名资深技术 SEO 顾问。请对下面的网站执行一次基于真实网页证据的 SEO 审计，并生成一份专业、可下载的 PDF 报告。
【审计对象】
目标网址：[https://example.com/]
主要市场：[中国 / 美国 / 全球]
目标语言：[简体中文 / 英文]
主要关键词：[填写关键词；不知道则由你根据页面内容推断]
审计范围：[单个页面 / 全站]
最大抓取页面数：[20]
报告语言：[简体中文]
【重要规则】
必须实际访问并检查网站，不要只根据域名或页面摘要判断。
所有结论都必须附上证据，包括实际 URL、抓取到的标签内容、数量、状态码或测试结果。
不得虚构数据。无法检测的项目标记为“N/A”，并说明原因和人工检测方法。
区分“已确认的问题”“可能的问题”和“优化建议”。
不要输出空泛的 SEO 常识；建议必须针对这个网站。
如果 PageSpeed Insights、Lighthouse 或其他接口限流/失败，记录错误，并提供手动测试链接。
审计以当前可公开访问的网页为准，不要求登录后台。
【需要检查的项目】
一、抓取与索引
HTTP 状态码、HTTPS、HTTP→HTTPS 跳转
www 与非 www 版本是否统一
robots.txt 是否存在、是否误拦截
XML Sitemap 是否存在、格式是否正常
robots meta 与 X-Robots-Tag
canonical 标签是否存在、自引用且与最终 URL 一致
尾斜杠、参数、大小写及重复 URL 问题
3xx 跳转、4xx/5xx 错误和重定向链
可索引性判断
二、页面 SEO
Title：实际内容、字符数、关键词、唯一性及修改建议
Meta Description：实际内容、字符数、搜索意图、CTA 及修改建议
H1：数量、内容、语言和关键词匹配
H2/H3：层级结构、主题覆盖和缺失情况
HTML lang 属性、页面实际语言及二者是否一致
hreflang（如果是多语言网站）
URL slug 的可读性和关键词相关性
正文主题、搜索意图、内容深度及关键词使用
Open Graph 和 Twitter Card
图片 alt、图片尺寸和懒加载
内部链接、外部链接和无效链接
结构化数据/Schema 类型、有效性和缺失项
三、性能与体验
分别检查移动端和桌面端 PageSpeed/Lighthouse
Core Web Vitals：LCP、INP、CLS
FCP、TBT、Speed Index
移动端适配、viewport 和可读性
主要性能瓶颈与可执行的修复建议
四、内容与搜索机会
推断页面主要关键词和搜索意图
判断 Title、Description、H1、正文语言及关键词是否一致
找出主题覆盖缺口
提供 3–5 个相关关键词或长尾关键词建议
提供可直接替换的 Title、Meta Description 和 H1
如果信息不足，明确标记为推断，不要假装拥有搜索量数据
【每项检查使用统一状态】
PASS：符合要求
WARNING：可改进或存在潜在风险
FAIL：确认存在明显问题
N/A：无法验证或不适用
【报告结构】
封面
“SEO Basic Audit Report”
网站名称、目标 URL、审计日期
审计范围与报告语言
Executive Summary
总体 SEO 健康度评分：0–100
PASS、WARNING、FAIL、N/A 数量
一段不超过 150 字的管理层摘要
Page Checks 总表
使用以下列：
检查项目 | 状态 | 实际证据 | 问题说明 | 修复建议
至少包含：
PageSpeed Mobile、PageSpeed Desktop、URL、Title、Meta Description、
H1、Heading Structure、Canonical、Robots、Sitemap、Language、Hreflang、
Image Alt、Internal Links、Schema、Mobile Friendly、Indexability。
Priority Actions
按照影响和紧迫程度列出优先事项：
P0：阻碍抓取、索引或排名的严重问题
P1：高影响，建议立即处理
P2：中等影响
P3：优化项
每项必须写明：
问题 → SEO 影响 → 具体修改方式 → 预期效果 → 验证方法
Insight Walkthrough
对每个 FAIL 和重要 WARNING 单独展开，格式为：
严重程度
问题标题
Evidence：实际检测证据
Why it matters：为什么影响 SEO
Fix：可直接执行的修复方法
Example：修改后的标签、文案或代码示例
Validation：修复后如何验证
推荐文案
给出可直接使用的：
推荐 Title（控制在约 50–60 个英文字符；中文按搜索展示合理控制）
推荐 Meta Description
推荐 H1
建议的 H2/H3 结构
推荐 canonical 标签
必要时给出 hreflang 和 Schema JSON-LD 示例
修复路线图
整理为表格：
优先级 | 任务 | 负责人建议 | 工作量（低/中/高） | 预期影响 | 验收标准
审计限制
列出接口失败、反爬限制、登录限制、未获得 Search Console/Analytics
数据等情况，并注明哪些结论需要人工复核。
【视觉与交付要求】
报告风格简洁、专业，参考专业咨询公司的 SEO 审计报告。
使用蓝色章节标题、浅灰表格边框。
PASS 使用绿色，WARNING 使用橙色，FAIL 使用红色，N/A 使用灰色。
长内容自动换页，不允许表格文字被截断或重叠。
URL 必须可以点击。
同时输出：
Markdown/网页预览版
完整 PDF 文件，文件名为：SEO-Audit-[域名]-[日期].pdf
生成后检查 PDF 的分页、表格、字体和中文显示是否正常。
现在开始审计该网址：[https://volumn.ai]
