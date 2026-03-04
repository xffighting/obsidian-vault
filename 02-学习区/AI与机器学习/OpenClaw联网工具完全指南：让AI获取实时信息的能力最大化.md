---
id: 8fcef021-15dd-4bc1-9f50-c3a8897290ff
---

# OpenClaw联网工具完全指南：让AI获取实时信息的能力最大化
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s?__biz=MzkyODE3ODk4OQ==&mid=2247486314&idx=1&sn=7ba0b60f06809f3785b1c5696ef64b3a&chksm=c30cd1d9b027a5d09dc579a20385f0533565448448d685ef7d92fdfcc4aaf112a2949f03277b&mpshare=1&scene=1&srcid=0304KWZahNqgIYsEKNLRvT0N&sharer_shareinfo=7a697dc906e854b0eb4d31a354c8384d&sharer_shareinfo_first=7a697dc906e854b0eb4d31a354c8384d#rd)
## 正文
公众号名称：猫哥AI量化

作者名称：爱量化的猫哥

发布时间：2026-03-03 20:09

**文/猫哥AI量化**

_ps：2000字_

大家好，我是猫哥。

很多人部署完OpenClaw（最近很火的AI助理项目，绰号"龙虾"）之后，饶有兴致的布置一些任务。

比如有个朋友让小助理查询关于AI的硬件产品情况，结果小助理忙了半天，回复做不到。后来甚至布置了一些查询国内信息的任务，也无法完成。

![[笔记同步助手/images/89d9bcdf9adccb3fbd4bd31232e5405a_MD5.png]]

猫哥一开始也踩过类似的坑，这几天终于把这个问题捋顺了，今天给大家科普一下，OpenClaw的搜索能力，到底来自哪里？

## 一、内置工具（不好用）

OpenClaw内置了一些可以实现联网的工具，主要有三个：web\_search、web\_fetch、browser。

web\_search主要是类似搜索引擎的工具，需要配置 Brave 的API KEY，而这个 API KEY 非常难以获取，这也是初始的小助理没办法搜索的关键原因，有搜索工具，但无法使用。

web\_fetch的作用是读取网页，就是你给他一个网页链接，能帮你读取内容，这个工具是不错的，可以正常使用。

![[笔记同步助手/images/b6f9c0979fe4ca5ba63c086cf61449d6_MD5.png]]

browser这个工具类似浏览器，能像人类一样操作浏览器，进行登录、点击。但这个工具也不是很好用，让读取个网页都做不到。

![[笔记同步助手/images/14ed07e6dbf7a0d75a99f56f51d93433_MD5.png]]

同样是浏览器相关的skill，agent-browser 这个 skill 就强大很多，需要通过 clawhub 安装一下（如果腾讯云部署则是自带的Skill之一，无需额外安装）。agent-browser 是一个浏览器自动化技能，让 AI 能像人一样操作网页——打开页面、点击按钮、填写表单、截图、提取数据等。

![[笔记同步助手/images/2d7151c6516e6e87e4ff5131df8bb3c5_MD5.png]]

## 二、搜索类Skill或MCP

下面介绍一下比较好用的搜索类Skill或者MCP汇总，可以挑选喜欢的安装一下：

### 1.tavily-search

Tavily Search 是一款专为 AI 智能体优化的网页搜索服务。与传统搜索引擎不同，它返回的内容经过精简和结构化处理，去除了冗余信息，直接提供与查询高度相关的核心内容。

该服务支持通用搜索和新闻搜索两种模式，并提供深度搜索选项以应对复杂的研究需求。

使用 Tavily Search 需要在官网申请 API Key：https://tavily.com/

特别适合 AI 助手、自动化工作流等需要高效获取网络信息的场景，能够显著提升信息检索的效率和准确性。

![[笔记同步助手/images/8dbb9c60f54b7a966fe261d3ba912073_MD5.png]]

### 2.Multi Search Engine

Multi Search Engine 是一款集成 17 个搜索引擎的多源检索工具，覆盖国内外主流搜索服务。国内引擎包括百度、必应中国版、360、搜狗、微信搜索、头条搜索和集思录；国际引擎涵盖 Google、DuckDuckGo、Yahoo、Brave、Startpage、Ecosia、Qwant 以及 WolframAlpha 知识计算引擎。

该工具支持高级搜索语法，如站内搜索、文件类型筛选、精确匹配和排除词等，并提供时间过滤功能，可按小时、天、周、月、年限定结果范围。其中 DuckDuckGo、Startpage、Brave 和 Qwant 等隐私搜索引擎不追踪用户行为，适合注重隐私的场景。WolframAlpha 则可用于数学计算、单位换算和知识查询。

最大优势是无需申请 API Key，开箱即用，适合需要多渠道交叉验证信息的场景。

![[笔记同步助手/images/375be163038a33825dafce83eae2b72b_MD5.png]]

### 3.ddgr

ddgr 是一款基于命令行的 DuckDuckGo 搜索工具，可在终端中直接进行网页搜索而无需打开浏览器，且无需API KEY！它继承了 DuckDuckGo 的隐私保护特性，不追踪用户搜索行为。

安装方式：

​

```
brew install ddgr
```

这个工具其实不属于 Skill 的范畴，但是非常推荐安装一下。

  

### 4.秘塔（metaso-search)

秘塔是国内强大的AI搜索工具，也支持API的方式进行调用，但市面上没有相关的Skill可以使用。

猫哥让OpenClaw给开发了一个Skill（文末免费领取），可以将这个Skill的压缩包发给OpenClaw，让他帮你配置好。

注意需要在这个网址申请一下 秘塔 的 API KEY :

https://metaso.cn/search-api/api-keys

![[笔记同步助手/images/6d55e7ce19713e2aed241cd488ddccfc_MD5.png]]

然后就可以愉快的使用了（注意秘塔的API是收费的，但是很便宜）：

![[笔记同步助手/images/f8c15ccde8acc258ec42ee9ae56d89c7_MD5.png]]

### 5.智谱web-search和web-reader

智谱提供web-search和web-reader这俩mcp提供给开发者使用，猫哥之前在 Claude Code 一直在用智谱的Coding plan和这个联网工具（MiniMax也有类似的工具）：

![[笔记同步助手/images/006ff002ec7b85402997df8dd527ca5a_MD5.png]]

openclaw的mcp安装是比较费劲的，猫哥昨天研究了很长时间，他是通过 mcporter 这个机制去安装和使用mcp，如果你直接让openclaw安装mcp，他大概率会直接修改自己的 openclaw.json 文件，然后自己把自己弄挂了。。

猫哥开发了 mcp-installer 这个skill帮助大家安装任意MCP（文末免费领取）

首先安装这个 Skill，然后输入这个提示词即可安装 智谱web-search：

​

```
使用 mcp-installer 这个skill 安装一下这个mcp：---
{
  "mcpServers": {
    "zhipu-web-search-sse": {
      "url": "https://open.bigmodel.cn/api/mcp-broker/proxy/web-search/mcp?Authorization=Your Zhipu API Key"
    }
  } 
}
---

我的api key是：XXXXXXXXXXXXXXXX
```

注意智谱同样需要使用 api key，需要在官网获取一下替换过去。

然后就可以愉快的使用了（注意即使有Coding Plan套餐，在OpenClaw使用智谱的搜索功能，也是收费的，但是也很便宜）：

![[笔记同步助手/images/9333c897c047129b1297bbee6bd76444_MD5.png]]

### 6.deep-research-pro

Deep Research Pro 是一款专为 AI Agent 设计的深度研究技能，能自动从多源网络搜索信息、综合分析，并生成带完整引用的专业研究报告。零成本运行，无需任何付费 API Key。

![[笔记同步助手/images/dedfd9e470a62cf5dcb7c0e3f1bf7cc1_MD5.png]]

核心能力：智能规划研究问题、多源搜索（15-30个来源）、深度阅读关键网页、生成结构化报告。

质量原则：每个论点必须有来源、交叉验证信息、优先采用近12个月资料、绝无编造。

适用场景：行业趋势研究、技术选型分析、商业策略调研、市场动态追踪。

一句话：让 AI 拥有"研究员"能力，从简单问答升级为系统性深度研究。

猫哥强烈推荐，这个skill的底层是使用的 DuckDuckGo 搜索工具，所以需要先安装一下ddgr：

​

```
brew install ddgr
```

可以直接让OpenClaw帮你安装一下这个Skill，然后就可以愉快的使用了：

![[笔记同步助手/images/e3d679d74745b07b855a6432b6dc19f3_MD5.png]]

## 三、搜索工具路由

有了这么多搜索工具，如何让OpenClaw选择使用哪个呢？可以在 Memory.md 配置搜索工具的使用顺序，

这个文件相当于AI的长期记忆，位置在：/root/.openclaw/workspace/MEMORY.md。AI每次和你对话之前，都会先读一遍这个文件，知道面对不同的任务，选择哪个搜索工具。

可以直接通过对话的方式去修改，将提示词发给AI即可（文末免费领取）：

![[笔记同步助手/images/77df291475f7f5ba42bd14b4ef475da8_MD5.png]]

Enjoy~

后台私信关键词“龙虾”，领取猫哥精心整理的《OpenClaw自学手册》，

含Skill的安装包和提示词模板

---

![[笔记同步助手/images/bdf738ff88b1b2d6cc074fb13b7db129_MD5.jpg|cover_image]]

Original 爱量化的猫哥 猫哥AI量化

继续滑动看下一个

![[笔记同步助手/images/38398b08d5d3d0c525d2b05691c0019c_MD5.png]]

猫哥AI量化

向上滑动看下一个