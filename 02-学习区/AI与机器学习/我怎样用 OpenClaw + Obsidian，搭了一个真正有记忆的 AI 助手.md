---
id: 43655bd7-bb84-48f6-8909-f653130c13c6
---

# 我怎样用 OpenClaw + Obsidian，搭了一个真正有记忆的 AI 助手
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s?__biz=MzkyNjU5NTUzNw==&mid=2247484403&idx=2&sn=541b613d13b8793e641bd9736f1cb30a&chksm=c3b243ed7eff487cbe2e4892b9fab525ae75d2a7be8647d406b934444a527ad04e6e1134f6e0&mpshare=1&scene=1&srcid=0304PHsblXDQIgDXSSNFahdi&sharer_shareinfo=553cf0e6e4cf6ae1b48128d1d66e776d&sharer_shareinfo_first=553cf0e6e4cf6ae1b48128d1d66e776d#rd)
## 正文
公众号名称：SoSME的Lab

作者名称：SoSME的Lab

发布时间：2026-03-01 17:12

> 💡 导读目录
> 
> \- 🧠 这套组合解决什么问题
> 
> \- 🧩 三个组件各自负责什么
> 
> \- 🏗️ 整体架构（如何把聊天变成写笔记）
> 
> \- ✅ 最小可行路径（照着做就能跑）
> 
> \- ⚠️ 安全边界与常见坑

过去一年，AI 代理（AI Agent）这个词被说烂了。

有的产品只是换皮的聊天框；有的 Demo 看起来很酷，一关页面什么都不记得；真要让它帮你整理电脑里的文件、跟踪项目，很少有东西敢说「真的能落地」。

我也绕了不少弯路，最后落到一个非常「土」但好用的组合：

用 OpenClaw（本地开源 AI 代理） + Obsidian（本地 Markdown 笔记） + Obsidian CLI 命令行，搭了一套「人 + AI + 命令行」三方协作系统。

这篇文章就讲三件事：

​

• 我是怎么把这三样东西串起来的？

• 它比普通「ChatGPT + 记事本」到底强在哪？

• 你如果想照着搭，最小可行路径是什么？

  

## 一、先说清楚：这三样东西各自干什么？

### 1\. Obsidian：本地的第二大脑

Obsidian 的本质，就是一个文件夹，里面全是 .md 文件：

​

• 你可以用文件夹、标签、双向链接来搭自己的知识库；

• 数据完全在本地，想怎么备份、同步都由你自己决定。

![[笔记同步助手/images/daa7659b983550b6697c9d451a006a94_MD5.png]]

### 2\. OpenClaw：本地跑的 AI 代理

OpenClaw 是一个开源的 AI Agent 框架，特点是：

​

• 支持 GPT、Claude、Gemini、DeepSeek 等各种大模型；

• 用 Telegram / WhatsApp / Slack 这些聊天工具做界面；

• 不仅能「聊天」，还可以在本地读写文件、跑命令行、控制浏览器、查网、下载和整理资料。

​

一句话：它是一个「能动手」的本地 AI 助手。

![[笔记同步助手/images/dc3c0c8cbd899c830e24f60686303605_MD5.png]]

### 3\. Obsidian CLI：从文件层到应用层的遥控器

很多人只把 Obsidian 当成「文件夹 + 编辑器」。但通过 CLI / URL scheme / Advanced URI 等工具，其实可以：

​

• 打开指定 Vault、指定笔记；

• 触发「插入模板」「运行查询」等内部命令；

• 控制当前视图打开什么内容。

​

而命令行，正是 OpenClaw 的主场。

![[笔记同步助手/images/19beb98f0578f07639f448f832deb200_MD5.png]]

  

## 二、故事从一个很具体的痛点开始

先讲一个很真实的小场景。

以前我的日常状态是这样的：

​

• 白天在各个 App 里到处记录：聊天、灵感、链接、截图；

• 晚上想回顾一下「今天做了什么」「有哪些结论」，发现东西散落一地；

• 想搞一个统一的「知识库」，但手工整理太费时间，很难坚持。

​

我真正想要的是：

​

• 能在聊天里随手问问题、讨论想法；

• 白天这些碎片，晚上自动「长」成结构化笔记；

• 所有内容进入一个长期维护的知识库，而不是飘在云端当一次性对话。

​

这套 OpenClaw + Obsidian + CLI 的体系，就是围绕这个痛点搭出来的。

  

## 三、整体架构长什么样？（一张图说明白）

先看大结构（Obsidian Vault 的目录）：

​

code

\# Obsidian Vault 根目录（示意）  
\[你的Obsidian目录\]  
├─ daily/ # 每日复盘 & 工作日志  
├─ projects/ # 项目 / 专题研究  
│ ├─ openclaw/  
│ ├─ ai-agents/  
│ └─ ...  
├─ inbox/ # 临时材料，AI 初稿、零散想法  
├─ reference/ # 长期参考资料、文档、规范  
└─ templates/ # 各类模板（日报、会议纪要、研究报告骨架）

在 OpenClaw 的工作目录里，我做了一件非常关键的小事：

建一个指向这个 Vault 的软链接（symlink）。

​

code

cd \[你的OpenClaw工作目录\]  
ln -s \[你的Obsidian目录\] obsidian-vault

于是：

​

• 对 OpenClaw 来说：obsidian-vault/daily/2026-03-01.md 是它可以读写的普通文件；

• 对 Obsidian 来说：这是它 Vault 里的正常笔记。

​

再加上 Obsidian CLI：

​

code

obsidian-cli open \\  
\--vault "OpenClawSecondBrain" \\  
\--file "daily/2026-03-01.md"

Obsidian 前台就会自动打开这篇笔记。

![[笔记同步助手/images/21b1e1b691799cbf7a73f86a61884e7a_MD5.png]]

  

## 四、三个「真在用」的场景例子

### 场景 1：AI 帮我写「每日复盘」，直接落在 Obsidian 里

白天我在 Telegram 里和 OpenClaw 聊天，可能包括：

​

• 今天做了哪些工作；

• 问了它一些问题（比如 KPI 分析、工具调研）；

• 临时想到的公众号选题、内容灵感。

​

晚上，我只发一句话：

基于我们今天的所有对话，帮我整理一篇 daily/2026-03-01.md 的复盘，结构：

\- 今日重点

\- 做了什么

\- 有哪些结论

\- 明天要推进什么

写在 obsidian-vault/daily/2026-03-01.md，然后在 Obsidian 里打开这篇笔记。

OpenClaw 会：

​

• 回顾当天对话内容；

• 生成一篇带小标题和要点的 Markdown；

• 写入 Vault；

• 调用 Obsidian CLI 打开这篇笔记。

​

我切回 Obsidian 时，一篇「已经起好草稿的日记」就在那里了。

区别在于：我不再需要「从零开始回忆今天发生了什么」，而是对着 AI 帮我梳理好的框架，做增删、写反思。

​

### 场景 2：专题研究 → 自动生成项目知识库

这篇文章本身，就是一个很好的例子。

我先让 OpenClaw 帮我调研：

搜索英文网络上关于 OpenClaw Obsidian 的讨论：

\- 有哪些主流教程和实践？

\- 重点讲了哪些连接方式？

\- 有哪些可借鉴的 workflow？

OpenClaw 调用 web\_search 之后，会给出一个汇总：比如哪些人用它做持久记忆、如何批量处理 Obsidian 笔记、怎么配合 Notion 等。

接着我加一句：

把这次调研整理成一篇笔记，放到 obsidian-vault/projects/openclaw/openclaw-obsidian-integration.md，

至少包括：背景 / 主流方案 / 适合我的做法 / 后续可以写的内容选题。

OpenClaw 根据我的要求生成结构、写入文件，然后用 CLI 打开。

我在 Obsidian 里做的是：

​

• 看它生成的大纲；

• 用自己的经验补充/修正；

• 把最后成品变成这篇公众号文章的骨架。

​

### 场景 3：用自己的笔记 + AI 回答问题，而不是凭空聊天

当 Vault 里已经累积了很多内容之后，我会反过来问它：

基于 projects/openclaw/ 下面所有笔记，帮我总结一下：

\- 我目前对 OpenClaw 的理解是什么？

\- 哪几类场景最适合我短期落地？

\- 未来两周可以尝试哪些小实验？

回答时请引用具体笔记名，让我能在 Obsidian 中点开。

OpenClaw 的处理思路大概是：

​

• 遍历 obsidian-vault/projects/openclaw/ 相关文件；

• 抽取关键结论和 TODO；

• 给出一个基于「我自己的历史笔记」的建议；

• 在回答里附上笔记文件名，方便我回到 Obsidian 里查看细节。

​

这一步的本质是：让 AI 建立在「我的知识库」之上，而不是凭空编故事。

  

## 五、为什么还要把 Obsidian CLI 拉进来？

既然 OpenClaw 可以直接写文件，为什么还要再加一层 CLI？对我来说有三个原因：

​

### 1\. 反馈要「看得见」

如果 AI 只是在某个目录里悄悄写了一个新文件，很容易被我忘掉；CLI 可以让它在最后一步「把窗口推到我面前」：打开那篇刚生成/刚修改的笔记。

这一步对使用体验非常关键：你会感觉是「我和 AI 一起在 Obsidian 里工作」，而不是在两个世界里各干各的。

​

### 2\. 复杂逻辑留在 Obsidian 生态里

比如：

​

• 模板系统（会议纪要模板、日报模板）；

• Dataview 查询；

• 任务管理插件。

​

这些工具已经在 Obsidian 里长得很成熟，我不想在 OpenClaw 里重造一遍轮子。

更好的做法是：让 OpenClaw 调用 CLI，触发这些现有命令，比如：

​

• 先用 CLI 打开一篇新建笔记；

• 再用 CLI 执行「插入模板」这个命令；

• 然后它再往里填具体内容。

​

### 3\. 清晰的角色分工，方便扩展

我的角色分工是：

​

• 人类：定方向、搭结构、做判断；

• OpenClaw：理解自然语言、决定下一步要调哪些工具；

• Obsidian CLI：把「命令」翻译成 Obsidian 的实际操作；

• Obsidian：展示结果、承载长期知识库存储。

​

这样的好处是：以后想换编辑器（比如从 Obsidian 换到别的 Markdown 工具），只需要换「CLI 接口」这一层，上层的「人类工作流 + AI 决策逻辑」可以基本保持不变。

  

## 六、如果你也想搭一套，最小可行路径

最后给一个尽量简单、但有实际感知的入门方案。

​

### Step 1：先有一个干净的 Obsidian Vault

建议结构：

​

code

daily/  
projects/  
inbox/

### Step 2：把这个 Vault 暴露给 OpenClaw

在 OpenClaw 的 workspace 下做一个软链接：

​

code

cd \[你的OpenClaw工作目录\]  
ln -s \[你的Obsidian目录\] obsidian-vault

### Step 3：只做两件小事，坚持一两周

• 每天晚上，让 OpenClaw 帮你生成一篇 daily/YYYY-MM-DD.md 的复盘笔记；

• 遇到一个「值得深入」的主题，就让它调研 + 整理，写成 projects/某主题.md，然后用 Obsidian CLI 打开这篇笔记，方便你继续改。

​

这两件事做顺了，你的感受会非常直观：

AI 不再是「一次性聊天窗口」，而是变成了一个能帮你维护 Obsidian 知识库的长期助手。

  

## 七、写在最后

我越来越不相信那种「一键托管整个人生」的 AI 神话。

更现实、更好用的路径往往是：

​

• 选几个自己已经在用、信得过的工具（比如 Obsidian）；

• 再用一个足够开放的 AI 代理（比如 OpenClaw）；

• 用很朴素的方式把它们「接起来」：文件夹、命令行，而不是黑盒魔法。

​

这篇文章算是我当前版本的「实践小结」。如果你对这里面的某一块（比如具体命令、目录结构、或者如何在 OpenClaw 里写 skills 来操作 Obsidian）感兴趣，可以在评论里告诉我。

后面我可以再写一篇更硬核的「配置手册版」，把具体命令、脚本结构和安全注意事项拆开讲清楚。

⬇️ 扫码加作者微信 ⬇️

![[笔记同步助手/images/74a442a2925a109a12a0e10ce5438f5a_MD5.jpg]]

---

![[笔记同步助手/images/17af40d7a53d0d71b4e0543079c58e37_MD5.jpg|cover_image]]

原创 SoSME的Lab SoSME的Lab

继续滑动看下一个

![[笔记同步助手/images/07db7e056a1eecea1bb918591b62663f_MD5.png]]

SoSME的Lab

向上滑动看下一个