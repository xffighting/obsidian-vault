---
id: 1b830b6c-7be9-4b79-906c-a05f434d68d3
---

# 如何让 OpenClaw 主动找你搭话（发日报）？Heartbeat + Cron 终极攻略
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488581&idx=1&sn=b10d252832618b628b3333002b1ced89&chksm=fa6daf12ff353853ab4dd26ea486f82065554b02f0533d38f3feabda4696eb5e37f045956a41&mpshare=1&scene=1&srcid=0304jJrWMZFvPhGGqi7UScWf&sharer_shareinfo=e34f141cfc5877a8adada36bcd156e22&sharer_shareinfo_first=e34f141cfc5877a8adada36bcd156e22#rd)
## 正文
公众号名称：骁哥AI编程

作者名称：骁哥AI编程

发布时间：2026-02-17 18:17

凌晨3点，​骁哥被手机震醒🫨，打开一看，是🦞仔主动给骁哥发了条消息：

![[笔记同步助手/images/d723ecdb10751be23260343897761017_MD5.png]]

👆这就是OpenClaw让千万人着迷的的理由之一：它能**主动找你搭话**，还能**自己自动干活** 🔥

靠的就是两个机制：**Heartbeat（心跳）+ Cron（定时任务）**

今天骁哥就把这套组合拳掰开了讲 💁

先搞清楚：Heartbeat 和 Cron 各管啥？

简单说：**Heartbeat 是有脑子的助手**（定期醒来自己判断要不要做事），**Cron 是精准闹钟**（到点必须执行）🥸

![[笔记同步助手/images/107c973043ada13fed87de4f0b8cc6e5_MD5.png]]

官方文档有个决策流程图，骁哥翻译成人话 💁

🤔 能跟其他检查一起批量处理？（看邮件、看热点、数据、竞品）→ 用 Heartbeat

![[笔记同步助手/images/20cab0cfdf6f7e3e595c489ed758e759_MD5.png]]

🤔 需要精确时间触发？（每天9:00准时发报告） → 用 Cron

🤔 需要跟主会话隔离？（跑深度分析，不想污染聊天记录）→ 用 Cron 隔离式任务

![[笔记同步助手/images/0a64bb9dde010c5673639d5a6fa5107d_MD5.png]]

画个图，便于大家理解👇

![[笔记同步助手/images/edea72d7d76efdd422b5f2ef68f3dbfe_MD5.png]]

最高效的配置是**两者结合**。骁哥现在的实际节奏：

![[笔记同步助手/images/822e918205a00906a69c02ee7a2487d0_MD5.png]]

下面就由骁哥来手把手教怎么配💁

Heartbeat：让🦞仔学会"没事找事"

​

☝️ 开启心跳

​

在 `openclaw.json` 里加配置：

```
{
  agents: {
    defaults: {
      heartbeat: {
        every: "1h", // 每1小时醒一次
        target: "last", // 投递到最近活跃的渠道
        activeHours: { start: "08:30", end: "23:00" } // 只在这个时段心跳(省token)
      }
    }
  }
}
```

**几个参数解释下** 🧏♀️

`every`：心跳频率

🔥 15-30分钟：实时监控（热点追踪、数据告警）

😎 1-2小时：日常够用，省 Token

😴 4-6小时：佛系模式

`target: "last"`：有事汇报时，消息发到你最近活跃的渠道（Telegram/WhatsApp/飞书 等）

`activeHours`：活跃时段，设了之后半夜不会被打扰 👍

频率一般设置为1-2小时即可🙆

​

✌️ 写 HEARTBEAT.md

​

在 workspace 根目录创建。每次心跳触发，都会读一遍这个文件

给大家举个大概的例子：

```
# HEARTBEAT.md

## 定期检查
- AI 领域热点追踪（有没有值得写的选题）
- 各平台内容数据变化
- 进行中未完成的工作进展
- 竞品/同类博主动态

## 主动汇报原则
- 发现好选题 → 立即汇报
- 热点来了 → 快速响应，提出内容方案
- 有数据洞察 → 主动分享
- 进行中工作 → 主动推进
```

Cron：精确定时 + 深夜重活

☝️ 需要**精确时间**触发的（早报必须9:00，不能"大约9点"）

✌️ 需要**隔离运行**不污染主会话的（深度分析、自主学习）

**这些就适合交给 Cron 来做** 💁

就比如最常见的场景，到点发汇报

```
openclaw cron add \
  --name "雷军早报" \
  --cron "0 9 * * *" \
  --tz "Asia/Shanghai" \
  --session main \
  --system-event "早报时间到，整理今日运营计划、昨日数据、热点追踪" \
  --wake now
```

到点触发，雷军在主会话里整理完直接发给骁哥。午报、晚报同理，改个时间就行

![[笔记同步助手/images/8b292fdc8e9c443f0c2a66d5830b8ac1_MD5.png]]

甚至！还可以让OpenClaw，在你睡觉时，自己学习，完成自我进化♻️（骁哥独门绝学🤫）

![[笔记同步助手/images/b19606e772400f816e86080df29cb74f_MD5.png]]

👆AI学到的知识，让他记录到`Memory.md`，重要的甚至可以直接提炼到`AGENTS.md`，让AI越来越聪明

白天干活 → 晚上学习 → 第二天用新知识干活 → 继续学... 🔄

![[笔记同步助手/images/35754996e2e8e4b84bb63395f6360526_MD5.jpg]]

小结

**OpenClaw的出现，让Agent真正进入了L3阶段（早期）**

💡 可以用手机（tg、飞书、QQ等）进行操作，让你可以充分利用碎片时间，进行无时无刻的价值生产（躺床上也能干活儿🤓）

💡 记忆集中管理，让Agent能一直保持记忆

💡 Cron和HeartBeat机制，让Agent能够做到“主动”来找你

💡 可以操作电脑，让他的“手脚”可以伸的更长

💡 多Agent协作也走向成熟

今天给大家介绍的就是，如何使用Cron和HeartBeat机制，让Agent能够做到“主动”来找你💁

官方文档 👉 https://docs.openclaw.ai/zh-CN/automation/cron-vs-heartbeat

当然啦，大家也不要被配置吓到。直接和OpenClaw说，让他自己配，也是OK的💁

哦对了，最好给聊天软件设置下晚间免打扰....

最后，如果你也对OpenClaw感兴趣，欢迎～🙋

![[笔记同步助手/images/0d6b072538fd3caf9a9770f81ba02c5f_MD5.png]]

**往期实践**👇

[OpenClaw之父加入OpenAI！从小镇少年到改变世界，这哥们的故事比电影还燃](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488561&idx=1&sn=9c3a41f326372eff43f674cd831fc2d5&scene=21#wechat_redirect)

[如何用OpenClaw，搭建一支"AI团队"？](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488533&idx=1&sn=c9f630c3b45776a95be33ebf03c34813&scene=21#wechat_redirect)

[OpenClaw，给新人的几个超实用小技巧（可能大部分人都会踩的坑）](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488516&idx=1&sn=7b4e8512e54e5798b5cf9669064fa377&scene=21#wechat_redirect)

[从零搭建OpenClaw，腾讯云+Telegram方案](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488490&idx=1&sn=6a10cc94ec999e4d1ab6b0b450e17726&scene=21#wechat_redirect)

恭喜你，发现了彩蛋！🤩

给大家展示一下骁哥的openclaw最近的调教成果👇

公众号写作大幅提效，完成度稳定可以达到60%以上

![[笔记同步助手/images/7e8bd70384e6e9aae5c4d3c088771cc6_MD5.png]]

复盘

![[笔记同步助手/images/9de89d70d809bf8081f96741b8dcf64d_MD5.png]]

网站迭代（骁哥的https://www.skill-cn.com）

![[笔记同步助手/images/d47bd95a2ec0768f193699bc6016582d_MD5.png]]

半自动化运维

![[笔记同步助手/images/0e9418cf10d86cec118bb0813ba00a1a_MD5.png]]

数据复盘

![[笔记同步助手/images/b90af69eb22f39836fdfd588e9a0df13_MD5.png]]

---

![[笔记同步助手/images/18c9789b4fadaab7adcb311f1a233fae_MD5.jpg|cover_image]]

原创 骁哥AI编程 骁哥AI编程

继续滑动看下一个

![[笔记同步助手/images/b87f170df8e233af1150bb33139e4500_MD5.png]]

骁哥AI编程

向上滑动看下一个