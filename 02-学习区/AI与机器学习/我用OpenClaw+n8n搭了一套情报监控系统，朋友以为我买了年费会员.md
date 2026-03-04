---
id: c45871ed-8808-4d2a-874d-a562b5c5c4b4
---

# 我用OpenClaw+n8n搭了一套情报监控系统，朋友以为我买了年费会员
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s/aFe3LN0gxIu5TcDp8_BcqQ)
## 正文
公众号名称：脉路

作者名称：脉路

发布时间：2026-03-03 15:19

# 引入

你是否有过这样的经历：每天早上花半小时刷行业资讯，结果刷着刷着就去看热搜了？关注的公众号太多，根本看不完？想跟踪某个竞争对手的动态，但手动搜索太费时间？

如果你也有「信息焦虑」，今天给你分享一个我正在用的方案：\*\*用OpenClaw+n8n搭建行业情报监控系统\*\*。关键是——完全免费，开源可定制。

## 为什么是OpenClaw？

OpenClaw最近火遍大江南北，鄙人也是从Clawdbot 阶段就开始使用了，之前一直想写篇文章介绍一下，但是苦于一直忙于工作，没有什么实际运用的进展，最近也算是实际用起来了，给大家做做分享，首先我们介绍下它：

一个开源的个人 AI 助手，跑在你自己的设备上，通过你已有的聊天渠道（飞书/微信/Telegram/Discord 等 20+ 平台）与你交互。

核心理念：能真正干活的 AI，不只是聊天。

它包含：

• Gateway 守护进程：控制中枢，常驻后台运行

• 多模型支持：可接入 Claude/GPT/Gemini/MiniMax 等各种模型

• Cron/自动化引擎：定时任务、Webhook、心跳检查

• 工具系统：文件操作、Shell 命令、浏览器控制、数据库等

• 插件架构：飞书/Telegram/Discord 等渠道插件

相信各位能看到这篇文章，之前也是对openclaw有所了解了，现在的安装比以前简单太多，现在官方也有了飞书的通道（以前还需要手动装一个插件），在这就不赘述了。

大模型推荐使用minmax或者glm，都有包月套餐，便宜管饱，鄙人基本上市面上的大模型都尝试过，claude用着确实很爽，奈何资金有限。

## 整体架构

一、 核心理念：

OpenClaw 是集自动化与智能化于一体的资讯处理系统，由以下三大支柱组成：

• 定时器 (Trigger)： 驱动任务循环。

• AI 大脑 (Processor)： 负责内容的深度筛选与智能格式化。

• 飞书通道 (Output)： 最终的触达与协作界面。

二、 数据输入层

系统采用 RSS 为主，多元辅助 的输入策略：

• RSSHub (自建代理)： 将 Twitter、YouTube、GitHub 等非标平台转化为标准 RSS 格式。

• n8n (特殊处理器)： 专门用于抓取和预处理高难度、非结构化数据（如 YouTube 详细元数据等）。

• Redis 缓存层： 为 RSSHub 提供加速，通过缓存减少请求频率，规避源站限流风险。

三、 核心组件与流程

rss\_fetcher.py 采集引擎 并行抓取所有 RSS/API；支持 3 次指数退避重试；内置 7 天滚动去重机制。

process\_and\_write\_news.py 处理器 调用 AI 进行内容筛选、摘要提取及格式化，并将结果写入飞书多维表格。

cleanup\_bitable\_news.py 清理器 定期维护飞书多维表格，清理冗余或过期数据，保持存储精简。

四、 配置与持久化

• RSS\_FEEDS.md： 订阅源管理中心，所有 RSS 链路的唯一配置入口。• rss\_cache.json： 本地去重数据库，确保消息不重复推送。

• 飞书多维表格： 结构化归档存储，作为最终的新闻数据库与查阅终端

不是所有RSS源都有价值，这里给你推荐几个高价值源：

\*\*AI/科技赛道\*\*

-   36氪：https://www.36kr.com/feed
    
-   极客公园：https://www.geekpark.net/feed
    
-   AIpresso（AI资讯）：https://aipresso.com/feed
    
-   Hacker News：https://hnrss.org/frontpage
    

\*\*行业垂直源\*\*

-   根据你关注的行业，搜索对应的RSS源
    
-   常用技巧：网站URL+/feed、/rss、/atom
    

我的建议是：先从5-10个源开始，跑通流程后再逐步添加。源太多反而会信息过载。

## 实现

当你的OpenClaw搭建好后，你完全不需要手动去操作任何东西了，你只需要发号施令！

从零搭建 OpenClaw AI 情报收集系统 — 完整 Prompt 列表：

注意：在 OpenClaw 飞书频道中，按顺序依次发送以下 prompt，每一步完成后再进行下一步。依赖关系：Step 1-2 可并行 → Step 3 依赖 Step 1 → Step 4 依赖 Step 2&3 → Step 5 依赖 Step 4 → Step 6 依赖 Step 4 → Step 7 依赖 Step 5&6 → Step 8 依赖全部 → Step 9-10 依赖 Step 8

Step 1 — 部署自建 RSSHub（基础设施）

⚙️ 为 Twitter/YouTube/GitHub 等非标准平台提供 RSS 转换服务

帮我在服务器上用 Docker 部署 RSSHub + Redis。要求：

​

```
1. 创建 infra/rsshub/docker-compose.yml，RSSHub 映射端口 1200，缓存用 Redis 7 Alpine
2. 配置 ACCESS_KEY 做访问控制（生成一个随机密钥）
3. 启动服务并验证 http://localhost:1200 可访问
4. 测试一个 Twitter 用户路由（如 /twitter/user/sama）确认可用
5. 所有配置参数用中文注释标注「需更改」
```

Step 2 — 创建飞书多维表格存储（存储层）

📊 创建新闻归档的飞书多维表格，定义字段结构

​

```
帮我创建一个飞书多维表格用于存储热点新闻，要求：
1. 表格名称：「热点新闻存档」
2. 字段设计：
- 标题（文本，主键）
- 来源（单选：知乎/B站/GitHub/60s/抖音/头条/小红书/RSS/Twitter/YouTube 等）
- 分类（单选：AI/科技/热点/开源/其他）
- 链接（URL）
- 摘要（文本）
- 抓取时间（日期）
3. 创建完成后，将 app_token、table_id、字段 ID 映射表记录到 feishu/BITABLE_CONFIG.md
4. 确认 OpenClaw 的飞书 App 对该表格有读写权限
```

Step 3 — 配置 RSS 订阅源清单（数据源）

📡 建立分层分类的订阅源清单，覆盖中英文核心信息源

依赖：Step 1（RSSHub 地址需要确认可用）

​

```
帮我创建 news/RSS_FEEDS.md，作为 RSS 订阅源管理清单。要求：
1. 用 Markdown 表格格式，字段：名称 | 地址 | 语言 | 备注
2. 按以下分类组织，每类至少 5-10 个高质量源：
- AI / 人工智能：机器之心、量子位、新智元、TechCrunch AI、Hacker News、OpenAI Blog、Anthropic Blog、Google AI Blog、Hugging Face Blog
- 社交媒体/大佬动态(via RSSHub)：Sam Altman、Elon Musk、OpenAI、Anthropic 的 Twitter（用 http://localhost:1200/twitter/user/xxx 格式）
- YouTube(via RSSHub)：Andrej Karpathy、Anthropic、Y Combinator 等频道
- 科技综合：The Verge、36氪、虎嗅、少数派、IT之家
- 博客/深度：Paul Graham、Simon Willison、阮一峰等技术大佬博客
- 开源/开发者：GitHub Trending、InfoQ
3. 另外添加一个「热搜 API」分区（Markdown 表格），包含：知乎热搜、B站热搜、微博热搜、GitHub Trending、抖音热搜、头条热搜、小红书热搜、60s看世界
4. 总源数目标 100+ 个
5. 每个 RSSHub 源地址使用 localhost:1200 前缀
```

Step 4 — 编写 RSS 采集引擎（采集层核心）

🔧 编写并行抓取脚本，支持 RSS + 热搜 API，带重试和去重

依赖：Step 2（需要确认表格可写入）、Step 3（需要 RSS\_FEEDS.md）

​

```
帮我编写 news/rss_fetcher.py，作为情报采集的核心引擎。要求：
1. 【源解析】从 news/RSS_FEEDS.md 自动解析所有 RSS 源和热搜 API 地址
2. 【并行抓取】用 ThreadPoolExecutor 并行抓取所有源（线程数 20）
3. 【RSS 抓取】用 feedparser 解析 RSS，提取标题、链接、发布时间、来源名、摘要
4. 【热搜 API】直接请求各平台 API（知乎/B站/微博/GitHub/抖音/头条/小红书/60s），解析 JSON 返回结构
5. 【重试机制】每个源失败后自动重试 3 次，指数退避（1s, 2s, 4s）
6. 【RSSHub 认证】自动为 localhost:1200 的请求附加 ACCESS_KEY 参数
7. 【去重】用 SHA256(title+link) 做 hash，维护 memory/rss_cache.json 缓存文件，7 天滚动清理过期条目
8. 【分层抓取】支持 --tier 参数：
- tier1：仅热搜 API（快速扫描，用于每小时热点）
- tier2：核心 RSS 源
- tier3：扩展 RSS 源（博客/播客等）
- 不指定则全量抓取
9. 【时间窗口】支持 --hours 参数，只保留 N 小时内的新条目
10. 【输出】去重后的新条目写入 memory/rss_latest.json
11. 【汇总】打印抓取统计：总源数、成功数、失败数、新条目数、去重数
12. 全局 socket 超时 15 秒，所有「需更改」的配置参数用中文注释标注
```

Step 5 — 编写 AI 处理+飞书写入脚本（处理层 + 推送层）

🤖 AI 筛选、摘要、翻译，格式化后写入飞书多维表格并推送消息

依赖：Step 4（需要 rss\_latest.json 作为输入）

​

```
帮我编写 news/process_and_write_news.py，负责 AI 处理和飞书写入。要求：
1. 【输入】读取 memory/rss_latest.json（rss_fetcher.py 的输出）
2. 【AI 筛选】调用飞书 App 的 API 获取 tenant_access_token，然后：
- 将原始条目发送给 AI（通过 OpenClaw 的 LLM 能力），让 AI 筛选出最有价值的新闻
- AI 对每条新闻生成中文摘要（英文源需翻译）
- AI 按重要性排序，分为：必看（10条）、速览（15条）、GitHub 项目（5条）、大佬博客（10条）
3. 【格式化】生成飞书富文本消息，格式包含：
- 日期头（公历 + 星期 + 农历）
- 分区标题（🔥必看 / ⚡速览 / 🐙GitHub / 📖博客）
- 每条：序号 + 标题（带链接） + 一句话摘要 + 来源标签
4. 【写入多维表格】将筛选后的新闻逐条写入飞书多维表格（使用 feishu/BITABLE_CONFIG.md 中的配置）
- 来源字段要匹配多维表格中的单选选项
- 已存在的标题跳过（防重复写入）
5. 【消息推送】通过飞书 API 将格式化的新闻速览直接推送给用户
6. 【模式支持】支持 --mode 参数：morning（早报30条）、noon（午报增量）、evening（晚报收官）、hourly（热点快报）
7. 所有飞书 API 凭据（APP_ID/APP_SECRET/APP_TOKEN/TABLE_ID）在文件头部定义，用中文注释标注「需更改」
```

Step 6 — 编写数据清理脚本（清理层）

🧹 自动清理 7 天前的旧新闻数据

依赖：Step 4（需要 rss\_cache.json 结构确认）

​

```
帮我编写 news/cleanup_bitable_news.py，用于定期清理旧数据。要求：
1. 清理飞书多维表格中 7 天前的旧记录（按「抓取时间」字段判断）
2. 同时清理 memory/rss_cache.json 中过期的去重缓存
3. 使用飞书 Open API 批量删除记录
4. 打印清理统计：删除了多少条记录、清理了多少缓存条目
5. 飞书 API 配置复用 feishu/BITABLE_CONFIG.md
6. 支持 --dry-run 模式（只统计不删除）
```

Step 7 — 端到端测试验证（集成测试）

✅ 验证完整数据流：抓取 → 去重 → AI 处理 → 写入 → 推送

依赖：Step 5 & Step 6

​

```
帮我做一次完整的端到端测试，验证整个情报收集系统：
1. 运行 rss_fetcher.py（全量模式），确认能成功抓取并输出 rss_latest.json
2. 检查去重是否正常工作（连续跑两次，第二次新条目应该为 0 或很少）
3. 运行 process_and_write_news.py --mode morning，确认：
- AI 筛选和摘要正常生成
- 新闻成功写入飞书多维表格
- 飞书消息成功推送给我
4. 运行 cleanup_bitable_news.py --dry-run，确认清理逻辑正确
5. 检查所有脚本的错误处理（模拟一个 RSS 源不可达的情况）
6. 给出测试报告：通过/失败项、性能数据（抓取耗时、处理耗时）、需要修复的问题
```

Step 8 — 配置定时任务（调度层）

⏰ 用 OpenClaw cron 配置所有定时任务

依赖：Step 7 全部测试通过

​

```
帮我配置 OpenClaw 的定时任务（cron），实现以下调度：
1. 【每小时热点】每小时整点运行：
- 执行 rss_fetcher.py --tier tier1 --hours 2（只抓热搜 API，2 小时窗口）
- 执行 process_and_write_news.py --mode hourly（发现重大热点才推送）
2. 【早报 09:00】每天早上 9 点：
- 执行 rss_fetcher.py（全量 tier1-3，12 小时窗口）
- 执行 process_and_write_news.py --mode morning（精选 30 条）
3. 【午报 14:00】每天下午 2 点：
- 执行 rss_fetcher.py（全量，6 小时窗口）
- 执行 process_and_write_news.py --mode noon（增量更新）
4. 【晚报 20:00】每天晚上 8 点：
- 执行 rss_fetcher.py（全量，8 小时窗口）
- 执行 process_and_write_news.py --mode evening（当日收官总结）
5. 【每日清理 03:00】凌晨 3 点运行 cleanup_bitable_news.py
6. 所有任务配置好后，列出完整的 cron 列表让我确认
7. 时区使用 Asia/Shanghai
```

Step 9 — 文档整理与归档（文档化）

📝 整理项目文档，确保可维护可复现

依赖：Step 8

​

```
帮我整理整个情报收集系统的文档：
1. 更新 feishu/BITABLE_CONFIG.md，补充完整的字段 ID 映射
2. 确认 news/RSS_FEEDS.md 中所有源的分类和状态是最新的
3. 创建一个系统架构说明（可以写在 MEMORY.md 或单独文件），包含：
- 系统架构图（文字版）
- 数据流说明
- 关键文件清单和用途
- 定时任务列表
- 运维要点（如何新增 RSS 源、如何排查问题）
4. 确保所有 Python 脚本头部有清晰的中文注释说明
5. 把项目文件推送到 GitHub 仓库，按目录分类整理（news/、infra/、feishu/、memory/）
```

Step 10 — 监控与优化（可选进阶）

📈 添加运行监控和性能优化

依赖：系统已稳定运行

​

```
帮我给情报系统加上监控和优化：
1. 【运行监控】每次定时任务执行后，在日志中记录：抓取源数、成功率、新增条目数、处理耗时
2. 【失败告警】如果某次抓取成功率低于 50%，通过飞书消息告警
3. 【源健康检查】每周统计各 RSS 源的可用率，标记长期不可用的源
4. 【性能优化】
- 如果某些源持续超时，建议是否降级或移除
- 分析去重缓存大小，必要时优化存储
5. 【新源推荐】基于当前覆盖情况，推荐值得添加的新信息源
```

## OpenClaw使用注意事项

1.第一次搭建好openclaw之后，如果是云服务器可以拍个快照；如果是实体部署，可以给它设置个规则：修改任何配置立即生效并且在十分钟之后清除配置，回滚到之前状态，回滚之前向我确定是否执行回滚，除非用户主动取消，负责默认执行。（如果修改有误则会自动回滚，无误则取消就行）

2.建议使用子代理模式运行任务，主进程只与你对话，避免进程卡死，同时也可以运行多个任务。

![[笔记同步助手/images/70579901472e910b6ef3d1020cacbe8d_MD5.png]]

3.可以配合MemOS实现多agent使用（后续各位有需求可以详细写下，目前我也在学习中......）

## 第四步：n8n自动化编排（可选进阶）

如果你想更进一步，可以用n8n做二次处理：

-   每天早上8点自动汇总昨晚的新内容，生成简报
    
-   推送到飞书群机器人
    
-   设置关键词告警（比如竞品名称、产品融资）
    

OpenClaw负责「抓」和「总结」，n8n负责「处理和推送」，两者配合就是完整的情报工作流。

## 避坑指南（亲测经验）

-   \*\*飞书API限制\*\*：免费版飞书有API调用频率限制，如果源太多可能触发限流，建议分批同步
    
-   \*\*源的质量>数量\*\*：我之前加了几十个源，后来删到15个反而效率更高
    

-   \*\*别让它「吃灰」\*\*：工具建好不用等于没建，每天早中晚会推送三次，惊鸿一瞥也是收获。
    

## 写在最后

信息收集的终极问题不是「工具」，而是「系统」。

OpenClaw+n8n给了你一个自动化的起点，但真正让这套系统发挥价值的，是你对信息源的持续优化、对飞书表格结构的不断迭代、以及对情报的主动消费习惯。

工具是免费的，但你的注意力是值钱的。把省下来的时间，用在真正需要深度思考的事情上。

👆 点击关注我，持续获取实用的 N8N 与自动化运维技巧！

---

![[笔记同步助手/images/46f59698af15faf6818ce4698bf50f0f_MD5.jpg|cover_image]]

原创 脉路 脉路

继续滑动看下一个

![[笔记同步助手/images/9c90c9b89f7cbfac905ba4c6fa61bb7a_MD5.png]]

脉路

向上滑动看下一个