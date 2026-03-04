---
id: 2e4ace2a-b731-464a-ab52-e3a86a93946e
---

# 🦞 装完 OpenClaw 不知道干嘛？先装这 5 个技能再说
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s?__biz=MzUzODQxMzYxNQ==&mid=2247489644&idx=1&sn=2c516e6c4c51adbc02e942db20676d2b&chksm=fb46ccf2afbd3328727e1c76a2a93470f98e653135d6a824e79f81dc35a884a8080b113b1267&mpshare=1&scene=1&srcid=0304zPlL3LGi7hKv1kB1jMIS&sharer_shareinfo=5c618d91a1761ed4bc662c563ee24099&sharer_shareinfo_first=5c618d91a1761ed4bc662c563ee24099#rd)
## 正文
公众号名称：徐公

作者名称：徐公

发布时间：2026-03-03 08:36

## 新手最常见的困境

可能很多人都遇到这个问题，兴冲冲装好了 OpenClaw，准备让 AI 接管一切繁琐工作。

结果一打开 ClawHub，瞬间傻眼！

**5705 个**社区技能摆在面前，看着生态繁荣挺唬人。但你稍微往里扒拉一下就会发现：

-   • 重复造轮子的有多少？
    
-   • 跑不通的三无废品有多少？
    
-   • 伪装成热门工具、随时准备偷你 API Key 的恶意代码有多少？
    

这不是"技能超市"，这是"技能荒原"——没有导航、没有筛选、没有质量保证。

我见过太多新手，一口气装了 20 个技能，结果最后没一个用起来，还把系统搞得乱七八糟。

​

---

## 这个仓库是什么

好在 GitHub 上有人做了件好事。

**VoltAgent/awesome-openclaw-skills**，一个标星 **24.8k** 的项目。

作者做的事很简单：从官方 ClawHub 的 5705 个技能里，筛掉了 2748 个"废品"，留下了 **3002 个**能用的。

被过滤掉的都有什么？具体看下表：

​

<table style="color: #3f3f3f"><caption><br></caption><tfoot><tr><td><br></td></tr></tfoot></table>

| 过滤类型 | 数量 | 说明 |
| --- | --- | --- |
| 疑似垃圾/机器人账号 | 1180 | 批量上传、无人维护 |
| 加密/区块链/金融投机 | 672 | 炒作工具、旁氏骗局 |
| 名字重复或过于相似 | 492 | 同一个东西换着法发 |
| 被安全审计认定为恶意 | 396 | 窃取数据、执行恶意代码 |
| 非英文描述 | 8 | 语言门槛 |

**留下来的 3002 个技能，被分成了 30+ 个类目。**

不管你是想搞浏览器自动化、多 Agent 协同、GitHub PR 管理，还是想让 AI 帮你收发邮件、控制特斯拉，这里全都有。

​

---

## 核心价值：从"我想做什么"出发

ClawHub 的默认搜索是"按名字搜"，但新手的困境往往是——我根本不知道该搜什么关键词。

awesome 清单的价值在于：**它让你从"我想做什么"出发，而不是"我该搜什么"出发。**

举个例子，你想让 AI 帮你写代码、跑测试、重构项目：

1.  1\. 打开 awesome 清单
    
2.  2\. 找到 "Coding Agents & IDEs" 分类（里面有 133 个技能）
    
3.  3\. 浏览描述，挑 2-3 个顺眼的
    
4.  4\. 点进去看详细说明
    

这比在 ClawHub 里搜"code"、"test"、"refactor"这些关键词快多了。

​

---

## 新手套餐：别贪多，先装这 5 个

3000 个技能摆在面前，我知道你又要选择困难了。

别慌。我帮你挑了一套**新手套餐**，按优先级排好了：

### 🛡️ 保命防身类（不装别往下走）

**1\. skill-vetting** — 装一切 Skill 前的安检门

在你装任何 Skill 之前，先让它帮你扫一遍。能检测潜在的安全威胁，比如奇怪的权限请求、可疑的网络连接。

> 防人之心不可无。这是我实测下来觉得必备的安全技能。

**2\. backup** — AI 犯蠢时的后悔药

AI 跑脚本、改配置，难免有翻车的时候。这个技能能一键备份你的 OpenClaw 配置，翻车了也能快速恢复。

> 别等系统被 AI 瞎折腾崩溃了再后悔。

### 💻 硬核开发类

**3\. coding-agent** — 你的专属代码代驾

直接召唤 Codex、Claude Code 等大模型，帮你写代码、跑测试、做重构。

支持并发执行多个任务，我试过在一个窗口里让它写前端，另一个窗口写后端，效率确实高。

**4\. github** — 用 gh CLI 做仓库运营

列出 PR、查看 CI 状态、拉取日志、创建 Issue……让 AI 帮你完成那些机械的 GitHub 操作。

我常用它来每天自动拉取需要处理的 issues，省了不少时间。

### 📧 打工人续命类

**5\. imap-smtp-email** — 通杀各大邮箱

Gmail、Outlook、163，统统支持。让 AI 帮你回那些没有营养的扯皮邮件，把时间花在更有价值的事上。

​

---

## 进阶路线：从 5 个到 N 个

等你把这 5 个技能用顺手了，就可以根据自己的需求扩展了：

### 如果你是重度开发者

加上 `docker-essentials`（容器管理）和 `test-coverage-analyzer`（测试覆盖率分析）

### 如果你需要做知识管理

加上 `obsidian`（写入 Obsidian 知识库）和 `notion`（结构化笔记）

### 如果你想做自动化运营

加上 `slack` 和 `discord`（把 AI 接入团队沟通渠道），让它定时发日报、播报 PR 变更

​

---

## 安装方法：三种方式选一种

### 方式一：用 ClawHub CLI（我推荐这个）

```
npx clawhub@latest install 
```

会自动处理依赖、配置路径，省心。

### 方式二：手动安装

把技能目录拷贝到 `~/.openclaw/skills/`，适合喜欢掌控一切的开发者。

### 方式三：在对话里直接贴链接

最懒的方式——在 OpenClaw 聊天框里直接贴技能的 GitHub 链接，说"用这个技能"，助手会自己完成下载安装。

这对非工程同事也很友好。

​

---

## 安全提醒：别把"精选"当成"绝对安全"

最后提醒一句：

**awesome 清单是目录，不是安全背书。**

它只是帮你做了第一层筛选，把明显的垃圾、废品、恶意代码剔除了。

但真正上生产之前，你还是要：

1.  1\. **看代码**：至少扫一眼核心逻辑，确认没有奇怪的网络请求
    
2.  2\. **看权限**：它要访问你的文件系统？执行命令？联网？问一句"为什么"
    
3.  3\. **用小号测试**：能用测试 API Key 就别用生产环境的
    

被收录不代表绝对安全，这是使用任何第三方代码都该有的意识。

​

---

## 从小白到高手，从 5 个技能开始

真正的极客，是在安全优质的工具库里搭乐高，而不是在没有王法的垃圾场里扫雷。

把 **VoltAgent/awesome-openclaw-skills** 加上收藏夹，别再去 ClawHub 当小白鼠了。

在这份清单里挑 5 个技能装起来，先把闭环跑通。你会发现，让 AI 替你干活，真的会上瘾。

​

---

> 仓库地址：https://github.com/VoltAgent/awesome-openclaw-skills

[我用 Claude Code 半年了，这个功能彻底改变了我的工作方式](https://mp.weixin.qq.com/s?__biz=MzUzODQxMzYxNQ==&mid=2247489404&idx=1&sn=f0033b2b650fe351fbf5bb65e3d48a2b&scene=21#wechat_redirect)

[Claude Code 的 Skills：不只是规则，而是"懒加载"的知识](https://mp.weixin.qq.com/s?__biz=MzUzODQxMzYxNQ==&mid=2247489409&idx=1&sn=49e54065d9c2f68927e2f2b99303377f&scene=21#wechat_redirect)

[我用 Claude Skills 做了个「文章自动配图」技能](https://mp.weixin.qq.com/s?__biz=MzUzODQxMzYxNQ==&mid=2247489422&idx=1&sn=4cde0e5177dfd14e1eabc8ce3972c118&scene=21#wechat_redirect)

[刚开源 Claude Code 神器，狂揽 2.5 万 GitHub Star，黑客松冠军配置全开源！](https://mp.weixin.qq.com/s?__biz=MzUzODQxMzYxNQ==&mid=2247489472&idx=1&sn=72fcd1d8d639eb9c7bb86e1e224e5986&scene=21#wechat_redirect)

[字节跳动超级智能体DeerFlow 2.0开源，登顶GitHub Trending第一！](https://mp.weixin.qq.com/s?__biz=MzUzODQxMzYxNQ==&mid=2247489639&idx=1&sn=9f3638ba82df4a6a15c39c5682c1cc34&scene=21#wechat_redirect)

---

![[笔记同步助手/images/c315b31a88d093b57774f3d694ca40ec_MD5.jpg|cover_image]]

原创 徐公 徐公

继续滑动看下一个

![[笔记同步助手/images/0b7c36ddde4ee5cf58425b3f151af928_MD5.png]]

徐公

向上滑动看下一个