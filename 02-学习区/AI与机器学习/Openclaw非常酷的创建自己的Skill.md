---
id: 1b23d7ac-480a-4eb9-beda-e8d5d02fb3c5
---

# Openclaw非常酷的创建自己的Skill
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s?__biz=MzIzMjUyMTM0Nw==&mid=2247483994&idx=1&sn=ec2fe65d15e5806005d3524270227f26&chksm=e923dd191a7478cd5a208a28f9ba73aab06f74d94cc631d1b6f4f78bf8dfed46fefa7256211b&mpshare=1&scene=1&srcid=0302bsAVHXThFdTL87CNCSot&sharer_shareinfo=a2ca7769ea0f6d8dc72d014adb2f972c&sharer_shareinfo_first=a2ca7769ea0f6d8dc72d014adb2f972c#rd)
## 正文
公众号名称：云界方舟

作者名称：小柒的方舟空间

发布时间：2026-03-02 19:45

大家在玩耍openclaw时，会发现openclaw的功能都是通过skill去延伸的，不管是写作/视频生成/图片生成/自动化流程等等都是通过一系列的skill去实现的！那么大家想过去创建自己的skill吗？

比如你打算做电商，整合一下电商/跨境电商的整体流程，做成一个skill，去自动化操作电商呢？

再比如你打算设计一款游戏，但是你不会游戏开发，是不是可以设计一款skill去自动化生产游戏呢？当然前提你要有游戏开发的基本配置，不然让skill去自动化配置消耗的时间太长！

或者你做自媒体运营，咱们这么多自媒体平台可不可以统一交给openclaw去管理呢？设计一个skill，去专门做这件事，这样会不会很酷呢？

那么我就来举一个例子：我想

Step 1

首先你要有skill-creator这个技能，因为这个技能是让openclaw自己创建技能的关键。

![[笔记同步助手/images/7fdd6b67d551e21b6364644ab1fae631_MD5.png]]

如果你的openclaw没有这个界面，可以通过命令行去查看一下

```
openclaw skills list
```

通过上面的命令会有一个列表

![[笔记同步助手/images/2af7d3a66a40ee1c6d10933361e978d7_MD5.png]]

Step 2

打开你的openclaw的chat页面，输入你想要创建skill的要求，我的要求是，每天把快手/抖音上的热点视频翻译成文字，这个步骤是openclaw搜索关键词相关的热度视频，然后通过openai-whisper去翻译成文字，你也可以使用付费服务，各大云厂商都有这个服务！

还有就是记得在你的电脑上安装python环境，因为关于AI方面的项目Python的用处非常大，再把国内的python镜像设置一下，对后面你玩openclaw会有非常大的帮助，因为openclaw在执行某些脚本的时候就是通过命令执行的，安装好为上策

![[笔记同步助手/images/7ca22bff1716b9821de77d6563bb40da_MD5.png]]

这个就是我创建的skill，完美使用，就是比较费token，​如果想要免费使用token，看我之前的文章

openclaw的出现，就是让你发挥想象力，去实现一些不可能实现的事情，AI的时代，你要做会玩AI的人，才能赶上时代的潮流！

好了今天的分享就到这，创作不易，记得一键三连哦！

  

---

![[笔记同步助手/images/ef33e594b45ac6e349962ef58e47ce10_MD5.jpg|cover_image]]

Original 小柒的方舟空间 云界方舟

继续滑动看下一个

![[笔记同步助手/images/1c6b1746c6f16564cedbe74d7d1fc173_MD5.png]]

云界方舟

向上滑动看下一个