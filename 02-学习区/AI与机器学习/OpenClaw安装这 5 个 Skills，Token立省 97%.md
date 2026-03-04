---
id: 826bb189-79ef-4b43-93c5-c92ab8c452dd
---

# OpenClaw安装这 5 个 Skills，Token立省 97%
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s?__biz=MzkyNjU5NTUzNw==&mid=2247484181&idx=2&sn=14435729185ebeda33ef910514d417cf&chksm=c31eb275197d349a8cecc42584e6c46c8337d87ba2218269a773380a7a4b13c22b375cc81aa7&mpshare=1&scene=1&srcid=0304XcuM88RsVqTRdQ4PeAAK&sharer_shareinfo=19653480d9a366322110b7e21b4a5bcf&sharer_shareinfo_first=19653480d9a366322110b7e21b4a5bcf#rd)
## 正文
公众号名称：SoSME的Lab

作者名称：SoSME的Lab

发布时间：2026-02-24 18:27

❓ 为什么需要 Skills？

​

"2 小时，100 美元。"

这不是什么高端餐厅的账单，而是一位 OpenClaw 用户真实发生的 Token 消耗故事。更夸张的是，有企业用户晒出月度账单：**180 万 token，$3600**。

![[笔记同步助手/images/de84859eb66525c331fe21908ad5ba82_MD5.png]]

但同样是用 OpenClaw，有人每月$0 成本跑得飞起。差别在哪？

除了模型选择、上下文管理等方法外，**安装专门的 Token 节省 Skills**是最直接的优化方案。

今天这篇，我把 OpenClaw 生态中 5 个专门用于节省 Token 的 Skills 全部整理出来。从安装到配置，手把手教你把 Token 费用打下来。

省下的，都是纯利润。

​

🥇 强烈推荐：QMD（必装）

​

**开发者**: Shopify 创始人 Tobi

**节省比例**: 90%

**实施难度**: ⭐⭐

**回本周期**: 1 周

​

核心功能

​

QMD 是一个本地语义搜索引擎，专为 AI Agent 设计。它解决了 OpenClaw 原生记忆系统的核心痛点：**每次查询都要读取全部记忆文件**。

**技术架构**：

​

• BM25 全文搜索：精确匹配关键词、ID、代码符号

• 向量语义搜索：理解同义词、概念关联

• LLM 重排序：Qwen3 reranker 提升精度

​

真实数据

​

| 指标 | 优化前 | 优化后 | 节省 |
| --- | --- | --- | --- |
| 每次查找 tokens | 15,000 | 1,500 | **90%** |
| 响应时间 | 3-5 秒 | 1-2 秒 | 60% |
| 记忆文件上限 | 2,000 tokens | 无限制 | \- |

**用户评价**："如果 MEMORY.md 超过 2000 tokens，QMD 一周就能回本"

​

安装配置

​

bash

\# 1. 安装 QMD CLI  
bun install -g qmd  
\# 2. 初始化 QMD  
qmd init --backend openclaw  
\# 3. 在 openclaw.json 中启用  
{  
"memory": {  
"backend": "qmd",  
"qmd": {  
"enabled": true,  
"max\_retrievals": 5,  
"truncation\_limit": 10,  
"timeout\_ms": 3000  
}  
}  
}  
\# 4. 配置记忆刷新（防止压缩破坏上下文）  
{  
"memoryFlush": true,  
"enableHybridSearch": true  
}

适用场景

​

| 场景 | 推荐指数 | 说明 |
| --- | --- | --- |
| MEMORY.md > 2000 tokens | ⭐⭐⭐⭐⭐ | 必须装 |
| 多项目并行 | ⭐⭐⭐⭐⭐ | 精准检索 |
| 长周期对话 | ⭐⭐⭐⭐⭐ | 避免遗忘 |
| 代码开发 | ⭐⭐⭐⭐ | 符号精确匹配 |
| 日常闲聊 | ⭐⭐ | 可选 |

注意事项

​

1\. 首次索引需要时间：大量记忆文件可能需要 5-10 分钟

2\. M 系列 Mac 性能最佳：Rust 原生支持 Apple Silicon

3\. 定期清理过期记忆：避免索引膨胀

​

**链接**: https://linux.do/t/topic/1562457

​

三、🥈 强烈推荐：prompt-guard（新发布 3 小时）

​

**节省比例**: 70%

**实施难度**: ⭐

**发布时间**: 2026-02-24（3 小时前）

​

核心功能

​

prompt-guard 是一个 Token 优化的提示注入防御引擎。它解决了两个问题：

​

1\. \*\*安全防护\*\*：防止恶意提示注入

2\. \*\*成本优化\*\*：分层模式加载，减少 70% token 开销

​

技术原理

​

**传统方式**：每次请求都加载完整防御规则（约 5000 tokens）

**prompt-guard**：

​

• 初始扫描：仅加载基础规则（约 1500 tokens）

• 深度检测：仅在可疑时加载完整规则

• 缓存优化：重复请求直接复用结果

​

安装配置

​

bash

\# 安装 Skill  
openclaw skills install prompt-guard  
\# 在 openclaw.json 中启用  
{  
"skills": {  
"prompt-guard": {  
"enabled": true,  
"mode": "tiered", # 分层模式  
"cacheTTL": "1h" # 缓存存活时间  
}  
}  
}

节省效果

​

| 场景 | 优化前 tokens | 优化后 tokens | 节省 |
| --- | --- | --- | --- |
| 简单查询 | 5,000 | 1,500 | 70% |
| 复杂任务 | 8,000 | 3,000 | 62% |
| 重复请求 | 5,000 | 500 | 90%（缓存） |

适用场景

​

| 场景 | 推荐指数 | 说明 |
| --- | --- | --- |
| 对外服务 | ⭐⭐⭐⭐⭐ | 安全 + 省钱 |
| 多用户环境 | ⭐⭐⭐⭐⭐ | 防止注入攻击 |
| 高频调用 | ⭐⭐⭐⭐⭐ | 缓存收益高 |
| 个人使用 | ⭐⭐⭐ | 主要收益是安全 |

**链接**: https://lobehub.com/skills/openclaw-skills-prompt-guard

​

四、🥉 推荐：memory-hygiene

​

**节省比例**: 30-40%

**实施难度**: ⭐

​

核心功能

​

memory-hygiene 负责保持向量内存精简，防止垃圾记忆浪费 token。它自动：

​

• 清理过期/无用记忆

• 合并重复内容

• 标记重要记忆防止误删

​

安装配置

​

bash

\# 安装 Skill  
openclaw skills install memory-hygiene  
\# 配置自动清理  
{  
"skills": {  
"memory-hygiene": {  
"enabled": true,  
"autoClean": true,  
"cleanInterval": "24h", # 每 24 小时清理  
"keepImportant": true # 保留重要记忆  
}  
}  
}

节省效果

​

• 向量内存减少 30-40%

• 记忆检索速度提升 50%

• 避免"记忆污染"导致的错误回答

​

适用场景

​

| 场景 | 推荐指数 | 说明 |
| --- | --- | --- |
| 长期运行 Agent | ⭐⭐⭐⭐⭐ | 必须装 |
| 记忆文件 > 100 个 | ⭐⭐⭐⭐⭐ | 清理收益高 |
| 多项目并行 | ⭐⭐⭐⭐ | 避免交叉污染 |
| 新用户 | ⭐⭐ | 可暂缓 |

**链接**: https://lobehub.com/skills/openclaw-skills-memory-hygiene

​

五、其他可选 Skills

1\. token-optimizer

​

**节省比例**: 70%+

**实施难度**: ⭐⭐⭐

**核心功能**:

​

• 动态工具加载：工具 schema 只在需要时注入

• 缓存优化：最大化 Anthropic Prompt Caching

• 重复轮次便宜 90%

​

**安装**:

​

bash

openclaw skills install token-optimizer

**适用**: 高频调用、长 workflow 场景

​

2\. memory-search

​

**节省比例**: 50-60%

**实施难度**: ⭐

**核心功能**:

​

• OpenClaw 内置记忆检索

• 管理 MEMORY.md 召回

• 比 QMD 轻量，但功能有限

​

**启用**:

​

json

{  
"memory": {  
"search": {  
"enabled": true  
}  
}  
}

**适用**: 不想装 QMD 的轻量用户

​

3\. SecureClaw

​

**节省比例**: -

**实施难度**: ⭐⭐

**核心功能**:

​

• 企业级安全插件

• 优化后约 1150 tokens

• 直接影响安全指令是否被遵循

​

**安装**:

​

bash

openclaw skills install secureclaw

**适用**: 企业用户、敏感数据处理

**链接**: https://www.helpnetsecurity.com/2026/02/18/secureclaw-open-source-security-plugin-skill-openclaw/

​

六、组合效果对比

​

| 组合 | 总节省 | 推荐指数 | 适合人群 |
| --- | --- | --- | --- |
| QMD + memory-hygiene | 90%+ | ⭐⭐⭐⭐⭐ | 所有用户 |
| QMD + prompt-guard | 95%+ | ⭐⭐⭐⭐⭐ | 对外服务 |
| 全部安装 | 97%+ | ⭐⭐⭐⭐⭐ | 企业用户 |
| QMD 单装 | 90% | ⭐⭐⭐⭐⭐ | 个人用户 |

推荐安装顺序

​

**新手**：

​

1\. memory-hygiene — 简单，零配置

2\. memory-search — 内置，启用即可

​

**进阶**：

​

1\. QMD — 节省 90%，一周回本

2\. prompt-guard — 安全 + 节省双重收益

​

**高阶**：

​

1\. token-optimizer — 动态加载 + 缓存优化

2\. SecureClaw — 企业级安全

​

❓常见问题

Q1: Skills 会影响性能吗？

​

**答**: 短期可能有轻微影响（首次索引/加载），但长期来看：

​

• 检索速度更快（精准匹配）

• 响应时间更短（缓存优化）

• 总体性能提升

​

Q2: 安装后需要重启吗？

​

**答**: 大部分 Skills 热加载，无需重启。但 QMD 需要初始化索引，建议重启一次。

​

Q3: 更新会丢失配置吗？

​

**答**: 配置存储在 openclaw.json，更新不会丢失。但建议更新前备份配置。

​

📚 总结：投资建议

​

**必装**：

​

• QMD — 一周回本，节省 90%

• memory-hygiene — 零配置，节省 30%

​

**推荐**：

​

• prompt-guard — 安全 + 节省，新发布

• memory-search — 内置功能，启用即可

​

**按需**：

​

• token-optimizer — 高频调用场景

• SecureClaw — 企业级安全需求

​

**核心建议**: 先装 QMD，一周回本；再加 prompt-guard，安全 + 节省双重收益。

​

💬 互动话题

​

你现在的 OpenClaw 月费用是多少？

​

• A. ¥0（本地部署党）

• B. ¥1-50（轻度用户）

• C. ¥50-200（中度用户）

• D. ¥200+（重度用户/企业）

​

近期文章

​

[我在 Discord 养了 5 个 AI 员工，它们居然自己开会！](https://mp.weixin.qq.com/s?__biz=MzkyNjU5NTUzNw==&mid=2247484087&idx=1&sn=d3fae2a7ae362b86efb4daf148b0b46f&scene=21#wechat_redirect)

[EvoMap 来了！AI 技能可以像基因一样遗传，成本降低 99%](https://mp.weixin.qq.com/s?__biz=MzkyNjU5NTUzNw==&mid=2247484087&idx=2&sn=12332bd078c3e20587caa9ea675f4e63&scene=21#wechat_redirect)

[我用AI团队1个月，效率翻了3倍 2026年项目管理大变局](https://mp.weixin.qq.com/s?__biz=MzkyNjU5NTUzNw==&mid=2247483927&idx=1&sn=78ba95eabab8a49e0dec54a2857717c0&scene=21#wechat_redirect)

[从7小时到50分钟：我的OpenClaw虚拟团队改造实录 （长文慎入）](https://mp.weixin.qq.com/s?__biz=MzkyNjU5NTUzNw==&mid=2247483907&idx=1&sn=9174c2f8eb290695ed8d963a24990929&scene=21#wechat_redirect)

[我在一台Mac上跑了3个OpenClaw，互不干扰](https://mp.weixin.qq.com/s?__biz=MzkyNjU5NTUzNw==&mid=2247483852&idx=1&sn=0a66b673f0f4a0d337ef88ac9893e1fd&scene=21#wechat_redirect)

​

⬇️ ⬇️ ⬇️ 评论区聊聊你的省钱妙招 ⬇️ ⬇️ ⬇️

---

![[笔记同步助手/images/73112fbfee40f996a01dc653789c3db7_MD5.jpg|cover_image]]

Original SoSME的Lab SoSME的Lab

继续滑动看下一个