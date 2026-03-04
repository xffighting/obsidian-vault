---
id: 8150e606-cdc9-4d98-9f7d-46c044adba72
---

# OpenClaw 省钱指南：从月烧到月花10 的完整攻略
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s?__biz=MzI5MDM4NTYwOA==&mid=2247504625&idx=1&sn=b682b96a4e10157e57520c0696c9df17&chksm=ed452555a1a6236c800e05ec90212d3c3dcb0db2214b6fa8e7844e86a3dfa17c2e793a6962b3&mpshare=1&scene=1&srcid=03049FkDDfkDe8uAxh1J3kqJ&sharer_shareinfo=4233f65c4dd320f6c4208577fc0a0ebf&sharer_shareinfo_first=4233f65c4dd320f6c4208577fc0a0ebf#rd)
## 正文
公众号名称：i 小声读书

作者名称：僧僧 GO

发布时间：2026-02-26 09:30

原文链接：[https://platform.minimaxi.com/subscribe/coding-plan?code=5L9P964O7B](https://platform.minimaxi.com/subscribe/coding-plan?code=5L9P964O7B)

**写完之前《[飞牛 OS 跑 OpenClaw 后，它学会了帮我打工](https://mp.weixin.qq.com/s?__biz=MzI5MDM4NTYwOA==&mid=2247504598&idx=1&sn=1826f0acf640e7698348b1895e494d6a&scene=21#wechat_redirect)》的教程，评论区不少朋友反馈每个月 Tokens 费用承受不起，今天就聊聊怎么给 OpenClaw 省钱。🏄‍♂️**

**OpenClaw 本身免费，但 API 费用是个无底洞。不优化的话月烧几百美元很正常，优化到位的话 $5-15/月就能舒服用。这篇文章把我踩过的坑和验证有效的省钱方法全部梳理一遍，照着做，至少省 80%。**

​

2026 年最火的开源项目，OpenClaw 当之无愧。GitHub 星标突破 21 万，从硅谷程序员到国内效率党，人手一个"AI 贾维斯"。但很多人兴冲冲部署完，用了三天一看账单，傻了。

有人一天烧 200，有人一个月3600，甚至有人因为自动化任务死循环，一觉醒来账单上多了四位数。

OpenClaw 的"免费"二字，只是指软件本身。真正花钱的地方在 AI 模型的 API 调用。每一句对话、每一次定时任务、每一个工具调用，都在吃 Token。而 Token，就是钱。

这篇文章不讲怎么装 OpenClaw，只讲一件事：**怎么花最少的钱，把 OpenClaw 用好。**

​

## 先搞清楚钱烧在哪里

要省钱，先得知道钱是怎么没的。OpenClaw 的 Token 消耗主要来自六个地方：

**第一，系统提示词的"底噪"。** 每次你跟 OpenClaw 说话，它不是只把你这句话发给 AI 模型。它会在前面塞一大堆系统提示词，包括你的身份配置（SOUL.md）、行为规范（AGENTS.md）、工具列表（TOOLS.md）、记忆文件（MEMORY.md）等等。这些加在一起，随随便便就是 8000-15000 个 Token。也就是说，哪怕你只发一个"你好"，后台已经消耗了上万 Token。

**第二，会话历史的累积。** OpenClaw 会把整个对话历史发给模型，让它保持上下文。问题是，聊得越久上下文越长，每次调用的成本就越高。我见过最夸张的情况：一个跑了一周没清理的会话，上下文累积到了 20 万 Token，单次请求成本高达 $6-8，而且基本都是超时失败，钱白花了。

**第三，Heartbeat 心跳机制。** OpenClaw 有个后台心跳功能，让 AI 定时自动执行任务。每次心跳触发都是一次完整的 API 调用，而且携带全量上下文。如果你设成每 5 分钟检查一次邮件，一天下来光心跳就能烧掉 $50。大多数人根本不需要这么高的频率。

**第四，工具调用的链式消耗。** 你让 OpenClaw"帮我整理今天的邮件"，它不是一步完成的。它要：调邮件 Skill 拉列表 → 逐封分析内容 → 判断优先级 → 调 Todoist 创建任务 → 生成摘要报告。每一步都是一次 API 调用，每次都携带完整上下文。一个看起来简单的指令，背后可能是 5-10 次 API 请求。

**第五，工具输出的上下文膨胀。** OpenClaw 会把工具调用的返回结果存进会话记录。如果你让它读一个 500 行的文件，3000-5000 个 Token 就塞进了上下文。下一次对话时，这些无用信息又会被原样发给模型，白白消耗。

**第六，模型选择不当。** 这是最普遍的浪费。Claude Opus 4.6 的定价是 25$5/$25（输入/输出，每百万 Token），而 Haiku 4.5 是 $1/$5。两者价差是 5 倍。听起来差距不大？别忘了 OpenClaw 每次调用都携带大量上下文，Token 消耗是指数级放大的，选错模型一个月下来差距非常可观。用 Opus 问一句"今天天气怎么样"，属于纯粹的暴殄天物。

搞清楚这六个出血点，省钱策略就清晰了。

​

## 第一招：模型降级，效果最猛

这是省钱第一大招，也是效果最立竿见影的一招。

OpenClaw 官方默认推荐 Claude Opus 4.6，效果确实好。但实话讲，80% 的日常任务根本用不到 Opus 的能力。日常闲聊、简单问答、定时任务检查、文件操作、翻译，这些活儿 Sonnet 4.5 完全够用，而 Sonnet 的输出价格只有 Opus 的 60%。

具体操作很简单，在 OpenClaw 配置里把默认模型改成 Sonnet：

​

```
{
"agents":{
"defaults":{
"model":{
"primary":"anthropic/claude-sonnet-4-5-20251001",
"fallback":"anthropic/claude-haiku-4-5-20251001"
}
}
}
}
```

这里还有个进阶玩法：配置 fallback 模型。把 Sonnet 设为主力，Haiku 设为兜底。正常情况走 Sonnet，万一触发限速或者余额不足就自动切到 Haiku，保证服务不中断。

真正需要 Opus 的场景（长文写作、复杂代码、多步推理）手动指定就行。实测下来，光这一步就能把月成本降 40% 左右。如果把简单任务进一步下放给 Haiku 整体降幅会更大。

如果你预算更紧，可以考虑国产模型。MiniMax M2.5 是目前 OpenClaw 官方原生支持的模型里性价比最高的选择之一，输入只要 $0.30/百万 Token，大约是 Claude Sonnet 的 1/10。在 SWE-Bench 上的表现已经接近 Sonnet 水平，日常任务完全够用。更狠的是，MiniMax 在 OpenClaw 安装向导里有原生入口，OAuth 一键授权，连 API Key 都不用手动配。

扫描下方的二维码即可获得 9 折订阅 MiniMax Coding Plan 的福利。29 元/月的 Starter 套餐足够使用了。

​

![[笔记同步助手/images/d8d2fe50d9202bc6b10b74f92bd27f0f_MD5.png]]

## 第二招：精简系统提示词，砍掉"底噪"

每次 API 调用都要重复发送的系统提示词，是一笔隐形的固定成本。好消息是，这些文件大部分都可以精简。

重点优化三个文件：

**AGENTS.md：** 默认文件里包含了群聊规则、TTS 配置、各种你可能用不到的功能说明。如果你只通过 Telegram 一个渠道使用，群聊规则删了；不用语音，TTS 删了。目标是压缩到 800 Token 以内。

**SOUL.md：** 你的身份描述。很多人写了一大段"你是一个友善的、专业的、乐于助人的……"，其实两三句话就够了。AI 不需要你写小作文来定义它的性格。

**MEMORY.md：** 记忆文件如果不定期清理，会越积越大。建议定期归档过期内容，只保留当前活跃的记忆条目。

精简完之后，每次调用的"底噪"从 13000+ Token 降到 3000-5000 Token，省的钱会随着使用量放大。

​

## 第三招：上 QMD，这是降本核武器

QMD（Quantum Memory Database）是 Shopify 联合创始人 Tobi 开发的本地语义搜索引擎，从 OpenClaw 2026.2.2 版本开始内置支持。

它解决的核心问题是：传统的记忆系统会把整个 MEMORY.md 文件一股脑塞进上下文，但其中 90% 的内容跟你当前的问题毫无关系。QMD 的做法是先在本地用语义搜索找到最相关的 2-3 句话，只把这些精准内容传给 AI。

效果有多夸张？官方数据是 Token 消耗降低 90-99%，响应速度提升 5-50 倍，精准度反而提高到 93%，因为 AI 不再被无关信息干扰了。

安装也不复杂，OpenClaw 2026.2.2 以上版本直接内置，启用后自动生效。完全本地运行，零 API 成本。

如果你的记忆文件已经膨胀到了几千 Token 以上，QMD 基本是必装的。它不是锦上添花，是救命。

​

## 第四招：控制 Heartbeat 频率

大多数人不需要每 5 分钟检查一次邮件。把系统检查频率从 10 分钟改到 30 分钟，版本检查从 3 次/天改到 1 次/天，通知设成"按需推送"而非"定时播报"。

核心原则：**大多数"实时"需求是假需求。** 你真的需要 AI 每 5 分钟帮你看一次邮箱吗？如果不需要，改成每小时看一次，或者干脆改成你主动触发。

进阶操作：把多个独立检查合并成一次调用。比如把"检查邮件""检查日历""检查待办"三个定时任务合并成一个"每日晨报"任务，一次调用搞定，省掉 75% 的上下文注入成本。

​

## 第五招：多 Agent 分流，用对的模型干对的活

OpenClaw 支持创建多个 Agent，每个 Agent 有独立的会话、记忆和工作空间。这不只是功能隔离，更是成本控制的利器。

思路很简单：复杂任务（写作、编程、深度分析）分配给 Opus 或 Sonnet；简单任务（日程提醒、快速查询、翻译）分配给 Haiku 或 Gemini Flash。每个 Agent 只加载自己需要的 Skill 和记忆文件，上下文干净，Token 消耗低。

更重要的是，多 Agent 架构解决了"记忆污染"问题。一个主 Agent 用久了，会话历史里塞满了各种不相关的信息，上下文越来越臃肿，Token 越吃越多，AI 反而越来越"神经错乱"。拆分成多个专注 Agent 之后，每个都是轻量级的，又快又省。

​

## 第六招：定期清理会话

这是最容易被忽视的一招。OpenClaw 的会话历史会无限累积，而每次请求都会把全部历史发给模型。一个跑了几天的会话，上下文轻松破 10 万 Token。

解决方案：养成定期新建会话的习惯。OpenClaw 有 `maxSessionTokens` 配置，可以设置上下文上限，超过自动截断（不会删除磁盘上的历史记录）。建议设在 50000-100000 之间。

另外，用 `/status` 命令随时检查当前会话的 Token 消耗情况，发现膨胀了就主动开新会话。

​

## 第七招：免费额度和订阅方案

如果你的日均使用量不高，可以优先考虑一些免费或低成本的接入方案：

​

Anthropic Claude Pro 订阅（$20/月）： 如果你的 API 账单会超过 $20/月，直接订阅 Pro 套餐反而更划算。通过 Claude Code CLI 的 API Key 接入 OpenClaw，费用直接走订阅额度。怎么订阅 Claude Pro 看这里 👉 [Claude 中国用户订阅指南](https://mp.weixin.qq.com/s?__biz=MzI5MDM4NTYwOA==&mid=2247504370&idx=1&sn=40a5ac1482abd5a43b9707cc85f31053&scene=21#wechat_redirect)。

**Google Gemini：** 免费层的 Flash 模型额度相当充足，5 小时刷新一次。配合 Antigravity 认证登录 OpenClaw，可以解锁 Gemini 3 Pro、Flash 等全系列模型。适合对成本极度敏感的用户。

**本地模型（Ollama）：** 如果你有一台 32GB 以上内存的 Mac 或者带独显的 PC，可以跑本地模型。API 成本直接归零。代价是响应速度和质量会有所下降，复杂任务搞不定，但日常简单交互够用。

**MiniMax Coding Plan：** MiniMax 推出了针对 Agent 场景的订阅方案，进一步降低了长期使用的门槛。配合 OpenClaw 原生支持，性价比极高。

![[笔记同步助手/images/d8d2fe50d9202bc6b10b74f92bd27f0f_MD5.png]]

​

## 实际省了多少？算笔账

假设你每天跟 OpenClaw 交互 30 次，包含一些定时任务和工具调用。

**优化前（默认 Opus + 不精简 + 不限频）：** 每日 Token 消耗约 200 万，月费用约 $300-600。

**优化后（Sonnet 主力 + 精简提示词 + QMD + 控频 + 定期清理）：** 每日 Token 消耗约 15-30 万，月费用约 $10-25。

降幅在 90% 以上。不是我吹，这是社区里很多人实测验证的数字。

如果你再激进一点，日常用 MiniMax M2.5 或 Gemini Flash，只在复杂任务时切 Sonnet，月费用可以压到 $5 以下。

  

## 附：手把手配置流程

上面讲的七招，核心操作都集中在 `~/.openclaw/openclaw.json` 这个配置文件里。下面把每一步的具体命令列出来，照着复制粘贴就行。

### 1\. 切换默认模型为 Sonnet + Haiku 兜底

直接编辑配置文件，或者用命令行：

​

```
openclaw config set'agents.defaults.model' --json '{
  "primary": "anthropic/claude-sonnet-4-5",
  "fallbacks": ["anthropic/claude-haiku-4-5"]
}'
```

如果你想加入 MiniMax M2.5 做更便宜的兜底：

​

```
openclaw config set'agents.defaults.model' --json '{
  "primary": "anthropic/claude-sonnet-4-5",
  "fallbacks": ["minimax/MiniMax-M2.5", "anthropic/claude-haiku-4-5"]
}'
```

同时注册可用的模型别名，方便在聊天中用 `/model sonnet` 或 `/model opus` 随时手动切换：

​

```
openclaw config set'agents.defaults.models' --json '{
  "anthropic/claude-haiku-4-5": { "alias": "haiku" },
  "anthropic/claude-sonnet-4-5": { "alias": "sonnet" },
  "anthropic/claude-opus-4-6": { "alias": "opus" },
  "minimax/MiniMax-M2.5": { "alias": "minimax" }
}'
```

改完重启网关生效：

​

```
openclaw gateway restart
```

### 2\. 接入 MiniMax M2.5

MiniMax 是 OpenClaw 原生支持的供应商，最简单的方式是 OAuth 一键授权：

​

```
openclaw plugins enable minimax-portal-auth
openclaw gateway restart
```

浏览器会自动弹出让你登录 MiniMax 账号，授权完成后自动配置好，全程不需要手动填 API Key。

如果你更习惯手动配置，先去 MiniMax 开放平台（国内：platform.minimaxi.com）创建 API Key，然后：

​

```
openclaw config set'models.providers.minimax' --json '{
  "baseUrl": "https://api.minimaxi.com/anthropic",
  "apiKey": "你的API-Key",
  "api": "anthropic-messages",
  "authHeader": true,
  "models": [{
    "id": "MiniMax-M2.5",
    "name": "MiniMax M2.5",
    "reasoning": false,
    "input": ["text"],
    "cost": { "input": 0.3, "output": 1.2, "cacheRead": 0.04, "cacheWrite": 0.15 },
    "contextWindow": 200000,
    "maxTokens": 8192
  }]
}'
```

注意：

国内用户 baseUrl 用 `api.minimaxi.com`

海外用 `api.minimax.io`

### 3\. 精简系统提示词文件

进入你的工作区目录（默认 `~/.openclaw/workspace/`），逐个编辑：

​

```
cd ~/.openclaw/workspace

# 编辑 AGENTS.md，删除不需要的功能描述
nano AGENTS.md

# 编辑 SOUL.md，精简到两三句话
nano SOUL.md

# 清理 MEMORY.md，归档过期内容
nano memory/*.md
```

精简原则：只保留你实际用到的功能描述，删掉群聊规则（如果你不用群组）、TTS 配置（如果你不用语音）、多余的行为规范。目标是把 AGENTS.md 压缩到 800 Token 以内。

### 4\. 调整 Heartbeat 频率

把心跳间隔从默认改为 30 分钟：

​

```
openclaw config set'agents.defaults.heartbeat.every''30m'
```

或者直接在 openclaw.json 里改：

​

```
{
"agents":{
"defaults":{
"heartbeat":{
"every":"30m",
"target":"last"
}
}
}
}
```

如果你想把多个定时检查合并成一个"每日晨报"，可以在 cron 任务里配置：

​

```
{
"name":"每日晨报",
"schedule":{"kind":"cron","expr":"0 8 * * *"},
"sessionTarget":"isolated",
"payload":{
"kind":"agentTurn",
"message":"检查今天的邮件、日历和待办，给我一份简要汇报。"
}
}
```

cron 任务存放在 `~/.openclaw/cron/jobs.json`，`sessionTarget: "isolated"` 表示每次都开一个新会话，不会累积上下文。

### 5\. 创建多 Agent 分流

在 openclaw.json 的 agents 部分配置多个 Agent：

​

```
{
"agents":{
"defaults":{
"model":{"primary":"anthropic/claude-sonnet-4-5"}
},
"list":[
{"id":"main","default":true},
{"id":"light","workspace":"~/.openclaw/workspace-light"}
]
}
}
```

然后用 bindings 把不同的聊天频道绑定到不同的 Agent：

​

```
{
"bindings":[
{
"agentId":"light",
"match":{"channel":"telegram","peer":{"kind":"group","id":"你的群组ID"}}
}
]
}
```

轻量 Agent 可以单独配置用 Haiku 或 MiniMax，工作区里只放最精简的提示词文件，上下文消耗极低。

### 6\. 查看当前消耗状态

随时检查你的 Token 使用情况：

​

```
# 在聊天中发送
/status

# 终端查看
openclaw status
```

输出会显示当前模型、上下文占用比例、预估费用。看到上下文超过 50% 就考虑开新会话。

  

## 写在最后

OpenClaw 是目前体验最好的个人 AI 助理框架，没有之一。但"免费开源"不等于"零成本运行"。不做优化的 OpenClaw 就是一台 Token 吞噬机，优化到位的 OpenClaw 才是真正的效率利器。

七招总结：

一、模型降级，日常用 Sonnet 或国产模型，复杂任务才上 Opus。

二、精简系统提示词，砍掉不用的功能描述。

三、装 QMD，本地语义搜索替代全文注入。

四、降低 Heartbeat 频率，合并定时任务。

五、多 Agent 分流，轻重任务分开处理。

六、定期清理会话，控制上下文长度。

七、善用免费额度和订阅方案。

按这个顺序来，从最容易操作的开始，每一步都有实打实的成本下降。用不着全部做完，光前三招就能覆盖大部分省钱效果。

OpenClaw 用得好不好，不看你花了多少钱，看你花的钱有没有花在刀刃上。

就写这么多，希望可以帮助到大家。欢迎点赞、分享、推荐三连。

感谢阅读。❤️

相关阅读

[AI Agent 来了：你手机里的 App 正在集体死亡，只是你还没发现](https://mp.weixin.qq.com/s?__biz=MzI5MDM4NTYwOA==&mid=2247504568&idx=1&sn=5c32196104a8c8500ca3fa54d06311a2&scene=21#wechat_redirect)

---

![[笔记同步助手/images/349ec678570af5dec32f9e69a9856d16_MD5.jpg|cover_image]]

Original 僧僧 GO i 小声读书

Read more

继续滑动看下一个

![[笔记同步助手/images/9e6100b8bf065b5976cbc2fd729d094b_MD5.png]]

i 小声读书

向上滑动看下一个