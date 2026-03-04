---
id: 2b590dc7-215b-4e4b-b9c1-6cb6e6b53a7e
---

# OpenClaw 深度拆解：从本地 AI 助理，看透企业级 Agent 的 17 层终极架构
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s?__biz=MzIzODIzNzE0NQ==&mid=2654457124&idx=1&sn=d63a9cd399e490685e5f1239d45f3563&chksm=f31c74de8d74210dbc12ec620da22b90b921dd325dee34fabc4373385b61b64e5feb6376bf96&mpshare=1&scene=1&srcid=0303nmSew45cuLeycTiZmGkK&sharer_shareinfo=28f9d8c48d8c39ef3dcb423054bb55fe&sharer_shareinfo_first=ef24b8059aac6e369062891c64462428#rd)
## 正文
公众号名称：玄姐聊AGI

作者名称：玄姐

发布时间：2026-03-03 08:03

原文链接：[https://channels-aladin.wxqcloud.qq.com/aladin/html/15aac50a-16d5-41d8-9584-c751962f50f6.html](https://channels-aladin.wxqcloud.qq.com/aladin/html/15aac50a-16d5-41d8-9584-c751962f50f6.html)

大家好，我是玄姐。

PS：

为了让大家真正搞懂 ​OpenClaw 技术架构和落地实践​，​我会开场直播，欢迎点击预约，直播见​。

最近的 AI 开发者圈子里，有一个项目以惊人的速度火爆出圈，它的 GitHub Star 数在短短一个月内飙升到了夸张的 246K，作为对比，沉淀了三年的头部低代码 AI 平台 Dify 目前是 130K 左右。

![[笔记同步助手/images/ce79dab9d8776ed25f82b394e86ff6ba_MD5.png]]

它就是经历了从 Clawdbot 到 Moltbot （小龙虾），最终定名的 OpenClaw。

![[笔记同步助手/images/9abbf62aae28e4f9d32a20a15e76219e_MD5.png]]

与市面上常见的云端通用大模型不同，OpenClaw 的定位异常清晰：一个完全本地化的、带“手脚”的个人数字助理。它可以寄生在微信、Telegram、钉钉等 IM 工具中，接受你的指令，跨越空间控制你的本地电脑、远端手机，甚至执行复杂的自动化运维和代码修复。

![[笔记同步助手/images/1b89310a06c0e3904548a3e41efc6dac_MD5.png]]

今天，我们就花点时间，硬核拆解 OpenClaw 背后的技术架构。在此基础上，我们将进一步推演：一个企业级的通用 Agent 架构，是如何从最简的 3 个模块，一步步膨胀演进到 17 个模块的终极形态的？

# 一、为什么我们需要完全本地化的 OpenClaw？

如果你是产研人员或极客，你一定遇到过云端 Agent 的痛点：隐私焦虑与高昂的 Tokens 费用。

OpenClaw 给出了一套极具性价比的本地化部署方案：一台 Mac mini（推荐 M4 Pro 芯片，36GB-48GB 统一内存）。

![[笔记同步助手/images/de87d7c3a859c3296488e5b6d2da0bc2_MD5.png]]

在这台设备上，你可以通过 8-bit（约占用 30GB 显存）或 4-bit（约占用 15GB 显存）量化，跑起一个 30B 级别的本地大模型。这不仅能 hold 住整个 OpenClaw 架构的运行，彻底告别后续的 Token 消耗，还能在绝对安全的环境下，让 AI 帮你处理 30 多个高频场景，无论是自动拉取分支进行 CR/CD 调优、修复 Bug，还是跨设备监控日志、清洗脏数据，它都能信手拈来。

![[笔记同步助手/images/433cad87424560493d512ff521e1316f_MD5.png]]

# 二、庖丁解牛：OpenClaw 的 6+1 核心架构与工程细节

OpenClaw 能够精准指挥多端设备协同工作，得益于其精巧的“中心化控制面板 + 模块化组件”架构。从源码和工程实现来看，它由 6 个功能侧模块和 1 个治理侧模块构成：

![[笔记同步助手/images/ee74e6d94e6f766dc7541e68760bd166_MD5.png]]

## 1\. Channel（渠道接入层）

这是 OpenClaw 的“耳目”。它负责寄生在 IM 工具中（如注册一个微信机器人账号 OCB）。由于微信等平台往往没有开放的官方收发 API，Channel 层实际上是一个定制的监听程序（类似 RPA），它获取消息后，统一转换为 OpenClaw 内部的标准 JSON 格式，再推给核心网关。

![[笔记同步助手/images/4a9e1006916b32d655ae632367f14095_MD5.png]]

## 2\. Gateway（网关枢纽层）

整个架构的心脏。它不仅负责安全鉴权，还承担着极其复杂的上下文与路由工作：

![[笔记同步助手/images/d67d943ce67dfe255c0d8c76f9c06a3d_MD5.png]]

上下文记忆（Context）：OpenClaw 的记忆不是简单的文本叠加，而是结构化的。临时对话存在 SQLite 中，RAG 知识存在 Vector DB 中，用户上传的文档则放在专门的文件柜（File System）中。

长连接与路由：它维护着与所有远端节点（Remote Node）的 WebSocket 长连接，并决定一个指令是该分发出去，还是留在本地执行。

## 3\. Pi Agent（大脑推理层）

这是一个基于 ReAct 模式的智能体业务逻辑层。它负责将记忆、用户请求和 Prompt 组装后发给 LLM。

![[笔记同步助手/images/93302b9b70a99a05cf1ad43eb0a3d932_MD5.png]]

> **💡 硬核细节：Factory（铸造厂）自我进化机制** PiAgent 内部有一个非常惊艳的“铸造厂”机制。如果它发现某个 Tool（工具）在一段时间内被连续调用超过 5 次，且准确率高达 80% 以上，机制就会自动将该工具“提权”升级为高优先级的“常用工具”，从而在后续推理中大幅提升响应速度和准确率。

## 4\. LLM（大模型层）

负责逻辑推演，比如本地部署的 Qwen 2.5 或云端的 Claude。

## 5\. Node（设备节点层）与 Skill（技能层）

这是 AI 的“手脚”。OpenClaw 将执行节点分为两类，对应不同权限的 Skill：

远端节点（Remote Node）：运行在你随身的 iPhone、办公室的 Windows 或卧室的 MacBook 上。它们连接网关，专门执行个性化 Skill（如：截屏、读取剪贴板、调用特定摄像头）。

![[笔记同步助手/images/5bd5d0c5af23f8e5aa507b264ee9ed1e_MD5.png]]

本地节点（Local Node）：部署在 Gateway 宿主机上，专门执行通用 Skill（如：联网搜索、查天气、读写本地文件）。

![[笔记同步助手/images/707e8bf56c800f0424bb6360602b1697_MD5.png]]

## 6\. Studio（治理可观测层）

独立于上述 6 个功能模块之外，这是一个 UI 面板，供开发者实时监控各个 Agent 的运行状态、日志流、文件变动以及函数调用耗时。

![[笔记同步助手/images/88e178bd872dd681fc6743c5888e912e_MD5.png]]

> **⚠️ 架构演进预警：Sidecar 沙箱隔离改造**目前版本的 OpenClaw 中，Gateway 和 Local Node 跑在同一个 Node.js 单进程里，这意味着执行诸如 Python 脚本、Shell 命令的通用 Skill 相当于“进程内直接调用”，存在极高的**沙箱逃逸风险**。**开源社区的演进方向是引入 Sidecar 模式：** 未来 Gateway 只负责编排路由，真正的 Skill 将被丢进一个独立的 Docker 容器沙箱中执行，用完即毁，实现彻底的进程分离与安全隔离。

# 三、数据流转推演：它到底是如何干活的？

为了让你看懂路由逻辑，我们对比两个经典链路：

场景 1：远端调度（“帮我截一张卧室 MacBook 的屏幕”）

![[笔记同步助手/images/0e8e9ab1d2d319ec1e74103721669ace_MD5.png]]

-   手机 Telegram 发送指令 -> Telegram Channel 转为 JSON -> Gateway。
    
-   Gateway 投喂给 Pi Agent -> LLM 推理得出结论：调用 screenshot 工具，目标端点：远端 MacBook。
    
-   Gateway 查询 Skill 路由表 -> 通过 WebSocket 将指令下发至 MacBook 的 Remote Node。
    
-   MacBook 执行本地截屏 -> 图片回传 Gateway -> Telegram Channel -> 你的手机端收到图片。
    

场景 2：本地通用计算（“搜索一下最新的 SpaceX 发射情况”）

![[笔记同步助手/images/aa0edd898c842267a9c58c8e6d119979_MD5.png]]

-   指令到达 Pi Agent，LLM 判断需调用通用的 web\_search 工具。
    
-   Gateway 识别为通用请求，不走远端转发，直接在宿主机的 Local Node 内触发搜索脚本。
    
-   搜索结果返回给 Pi Agent，由大模型进行“拟人化”总结润色。
    
-   最终文本交由 Gateway 推回给用户。
    

---

# 四、举一反三：企业级 Agent 的 17 层终极架构

理解了 OpenClaw，我们再放眼整个企业级 AI 架构。我们在企业内部构建应用，绝不仅仅是为了写一个脚本，而是要支撑成千上万的并发和极其复杂的业务流。

一个通用的 Agent 架构，经历了怎样的演进？

阶段一：最小可用单元（3 个模块）

用 Dify 搓一个简单的智能客服，只需要 3 块基石：

![[笔记同步助手/images/860733ead8f3569984ac8d0dcf198808_MD5.png]]

-   Agent 业务逻辑层（搭建 Workflow）
    
-   大模型层
    
-   知识库
    

阶段二：功能链路的全面膨胀（扩充至 11 个模块）

当系统接入企业内网，复杂度呈指数级上升：

![[笔记同步助手/images/07242977927ceb714d132b0e3f8b6f7d_MD5.png]]

-   多模型灾难：接入了千问、DeepSeek、嵌入模型、OCR 等，你需要一个 AI 模型网关来统一接口和路由。
    
-   多数据源灾难：面对散落的内部 API、数据库和 MCP 协议，你需要 MCP 资源网关进行屏蔽。
    
-   流量与并发：外部请求涌入，需要 流量网关区分普通微服务流量与 AI 流量；随后交给 Agent API 网关路由到特定的智能体集群；为防止大模型推理缓慢导致系统雪崩，必须在网关后加入 MQ 消息队列实现异步削峰。
    
-   能力进化：单点 Agent 升级为 主从/多智能体（Master-Slave）协同架构；并在执行端挂载长期 Memory 引擎和 Skills 本地执行域。
    

阶段三：无治理，不企业（补齐 6 个治理模块）

为了让这套庞然大物稳定运转，治理侧必须火力全开，这构成了最终的 17 层终极架构：

![[笔记同步助手/images/6b5660ecbd176655aa2b4afba96deadd_MD5.png]]

-   AI 配置中心与 AI 注册中心（实现成百上千个 Agent 之间的服务发现）。
    
-   AI 评估中心（量化回答质量）与 AI 安全中心（拦截 Prompt 注入、执行数据脱敏）。
    
-   AI 治理与可观测中心（监控 Token 消耗水位、链路追踪）。
    
-   AI 弹性伸缩中心（根据请求量动态启停本地模型节点）。
    

# 五、写在最后

从 GitHub 上爆火的极客玩具 OpenClaw，到企业内部严密的 17 层通用架构，底层的技术逻辑正在发生根本性的演变。

在这个阶段，单纯的 Prompt 提示词工程已经难以构建真正的技术壁垒。未来的 AI 核心竞争力，属于那些能够拆解、重组架构，并具备将 AI 模块与现有企业级基础设施（网关、MQ、沙箱、注册中心）深度融合的工程落地派。

PS：

为了让大家更深度的搞懂 ​OpenClaw 技术架构和落地实践​，​我会开场直播，欢迎点击预约，直播见​。

好了，这就是我今天想分享的内容。如果你对构建企业级 AI 原生应用新架构设计和落地实践感兴趣，别忘了点赞、关注噢~

—1—

加我微信

扫码加我👇​有很多不方便公开发公众号的我会直接分享在​朋友圈，​欢迎你扫码加我​个人微信​来看👇

![[笔记同步助手/images/c7bbc5d09a61057b385ccac2a99d4df4_MD5.jpg]]

加星标★，不错过每一次更新！

⬇戳”阅读​原文“，立即预约！

---

![[笔记同步助手/images/8ba7d4ef8e7caa430eae153fd1adb97f_MD5.jpg|cover_image]]

Original 玄姐 玄姐聊AGI

Read more

修改于

继续滑动看下一个

![[笔记同步助手/images/da6f67a27f129f62d29d4e6e17dd510b_MD5.png]]

玄姐聊AGI

向上滑动看下一个