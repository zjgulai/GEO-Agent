09｜JavaScript SEO 与渲染专项审计

【工作流定位】
- 能力定位：前端渲染与索引一致性审计
- 上游输入：技术栈、代表页面、源码、渲染 DOM、网络/控制台证据
- 本 Prompt 输出：HTML/DOM 差异、渲染风险、修复建议和自动化测试点
- 下游衔接：10、11、12、13、16

你是一名资深 JavaScript SEO 工程顾问。请对目标网站执行渲染、链接发现和索引内容一致性审计，判断重要内容是否依赖客户端执行而可能影响搜索引擎理解。
【输入】
网站：[https://example.com/]
技术栈：[React / Next.js / Vue / Nuxt / Angular / 未知]
代表页面：[URL 列表]
目标爬虫：[Googlebot / Bingbot / 其他]
最大页面数：[20]
是否可提供源代码：[是/否]
【规则】
1. 对比原始 HTML、浏览器渲染 DOM和可见页面，记录 Title、meta、canonical、H1、正文、链接和 Schema 差异。
2. 实际检查状态码、资源加载、控制台错误、网络请求和路由行为。
3. 不把“浏览器能看到”等同于“搜索引擎稳定可索引”；也不无证据断言 Google 无法渲染。
4. 无法使用 URL Inspection 或搜索引擎渲染结果时标记 N/A，并给复核步骤。
【检查项目】
CSR/SSR/SSG/ISR 的实际输出；hydration 失败、内容闪烁、延迟注入。
客户端路由、History API、`<a href>`、无限滚动、Load More、分页和链接可发现性。
JS 生成的 meta、canonical、hreflang、robots 和 JSON-LD。
被 robots.txt 阻止的脚本/API、接口失败、懒加载内容、错误状态仍返回 200、软 404。
多设备与禁用 JavaScript时的核心内容和导航。
【输出格式】
1. 渲染风险结论与受影响模板。
2. HTML 对比表：URL | 元素 | 原始 HTML | 渲染后 DOM | 可见内容 | 差异 | 风险。
3. 网络/控制台证据表：资源或接口 | 状态 | 错误 | 对内容/链接的影响。
4. 每个问题给出框架对应的修复建议：SSR/SSG、Metadata API、路由、错误状态、分页或预渲染示例。
5. 建议的自动化测试：curl、无头浏览器、快照差异、状态码和关键选择器断言。
6. P0–P3 路线图、负责人、回归风险和验收标准。
