---
id: c61512cb-4449-495a-a1e6-15defeb97528
---

# 【建议收藏】OpenClaw太烧钱？解锁这2个隐藏渠道，Token无限免费用！
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s?__biz=MzUyMjEyMDcyOQ==&mid=2247499953&idx=1&sn=13e7ab2d8030c38b55c81b1e69d98ee3&chksm=f81fd2a00bfabdafb01f23efa8fe0ee66d966df2e89a9fcdafefe1a0b8f04aa7a49ba3d04be8&mpshare=1&scene=1&srcid=0302jxqaLVCgMvlAlpwIK8M0&sharer_shareinfo=2a6ce406542242be059b679a49deb093&sharer_shareinfo_first=2a6ce406542242be059b679a49deb093#rd)
## 正文
公众号名称：Hellos AI

作者名称：Hellos AI杰哥

发布时间：2026-02-19 09:17

![[笔记同步助手/images/0534aec3a3c9864019d1ffcb5d14ae86_MD5.png]]

_**您好，我是Hellos AI，擅长AI编程、分享AI工具资讯等，立志让更多普通人了解AI、学会AI，利用AI找到人生的第二曲线。**_

我这几天用这个openclaw，发现token消耗确实快，接着我又发现了几个能够免费用大模型的渠道！

这里我把这2个渠道和整个配置过程介绍给大家！

**0****1**

**opencode**

这个opencode是啥呢？它跟常见说的claude code一样是一个编码工具。

**0****1**

**opencode**

之前就看到有粉丝朋友说opencode里面有不少免费模型，这里我也来看看！例如，我打开opencode后，发现里面有几个免费模型：

![[笔记同步助手/images/7727aafe8e5ffe9f196d0944b85579c1_MD5.png]]

这里有MiniMax M2.5、Big Pickle、Kimi K2.5 Free这几个是免费模型

嗯，不过呢，我发现这几个模型没法直接在openclaw中使用，例如我该怎么办呢？

**0****2**

**解决难题**

因为这个opencode是一个开源的编程工具，所以我直接看代码不就知道它该如何调用了吗？

接着我直接下载它的源代码，如：

![[笔记同步助手/images/e2bd4712cd37ba6ea6d64efe60c2f628_MD5.png]]

![[笔记同步助手/images/d849f24fc528b4ac66b7d1d36f6b8712_MD5.png]]

嗯，既然代码已经到本地了，那接下类使用antigravity直接分析代码，找到正确的调用接口，如：

![[笔记同步助手/images/089363576a5ac34e1507411988bfec81_MD5.png]]

antigravity告诉我它的访问接口是https://opencode.ai/zen/v1/chat/completions，然后，我到postman进行测试，如：

![[笔记同步助手/images/d313ce6eddfd777bb5454f849a96b70a_MD5.png]]

不过需要有一个TOKEN，这个可以在opencode官网注册账号后获得，如：

![[笔记同步助手/images/07a03a9717d8fcc656a7c14b290eb0cf_MD5.png]]

如果一个账号的额度受限，那么我们可以多注册几个账号，那么就有多个TOKEN，可以循环使用！

**0****3**

**openclaw配置**

对于这个opencode的配置也比较简单，我们只需要在执行命令后，设置对应的TOKEN就行，例如：

![[笔记同步助手/images/5a34f1667eb6fd471c5ba56f73534d94_MD5.png]]

选择模型如：因为这2个模型是免费的

![[笔记同步助手/images/577e29def4df4fd25446d2f1cd45f25f_MD5.png]]

![[笔记同步助手/images/f839c0f4f8058ca6f0fd572f9d9c36f8_MD5.png]]

OK，现在呢，我们就可以直接用openclaw提供的免费大模型啦！

PS：毕竟在油管、reddit上，这个kimi2.5和minimax这2个模型是很多玩openclaw玩家的推荐模型，所以照着这几个国产大模型冲准没错！（如果你想要更聪明、智能不在乎token消耗，那就冲claude那几个顶级模型吧）

**0****2**

**英伟达**

其实在我前面的文章中有提到可以使用英伟达的免费模型，但是我倒是没有写文章或发视频该如何使用！这里我把如何获取和配置放这里！

**0****1**

**注册**

网址：https://build.nvidia.com/models

![[笔记同步助手/images/706441e7ad8fc129dc69ed4bc8c72d7c_MD5.png]]

点击右上角的login进行注册：

![[笔记同步助手/images/9586db79562a25fc8ae57da4aaee3bd8_MD5.png]]

![[笔记同步助手/images/b4cc7e0f870307386809d8899c390f2b_MD5.png]]

在这里选择API KEYS，然后进入到页面中添加KEY即可！

![[笔记同步助手/images/7416efdc30f0039771fc8259d26cf475_MD5.png]]

添加好这些KEY之后，我们就可以到openclaw进行配置了，如下：

**0****2**

**配置**

配置过程如下，这个的配置跟前面的不太一样，而是直接在openclaow.json中直接进行修改，如：

PROVIDER中的配置

![[笔记同步助手/images/e0cd06e9ad8bbad434d57b99b893af67_MD5.png]]

![[笔记同步助手/images/02d07c788502e404ba6b204f79d5db9d_MD5.png]]

因为是手动修改该配置，那么我还需要重启一下容器，然后再进行测试！

**0****3**

**测试**

这里验证如下：

![[笔记同步助手/images/365815297b4c1671c408b8401b412bd5_MD5.png]]

![[笔记同步助手/images/37b9b1d0a43486d0bb6e3a26b6c6fb5f_MD5.png]]

**0****3**

**写在最后**

好了，关于openclaw中如何配置其他免费模型的方法和步骤就介绍到这里了。各位朋友知道还有哪些能够免费使用模型的可以在评论区讨论哈！

另欢迎大家来我的个人博客网站https://hellosai.cc/逛逛！

**关注杰哥不迷路**，每天给你分享不一样的实用好工具。

免责声明：本公众号分享的内容以及软件等来自互联网，仅供大家学习交流，同时请遵守你当地的法律法规，否则造成的一切后果自负，与本公众号无关。如有侵权联删！部分知识难免有时效性，若内容过期失效，请见谅,感谢！

  

---

_**喜欢这篇干货？如果觉得不错，请帮我一键三连，转发给您的朋友，都是对我最大的鼓励与认可。**_

_**如果想第一时间收到推送，可以把我的公众号加个星标🌟方便后面我们一起探讨AI或有意思的东西，还能够快速找到我！我们明天见！**_

_**—**_ **END** _**—**_

_**图 | 来源网络侵删**_

_**欢迎点赞，在看，转发给我鼓励~**_

_**👇👇关注我👇👇**_

![[笔记同步助手/images/bdeab444e57624ddadd05271176bc80a_MD5.png]]

_**👇👇扫码加入粉丝群领取福利👇👇**_

![[笔记同步助手/images/629d5109f51b18ee8164ba0c15e246ef_MD5.png]]![[笔记同步助手/images/1658aa51262c49eca9a0f8deffdfb92c_MD5.png]]

  

---

![[笔记同步助手/images/1b4a17ad571ea2b4681043e8521dca24_MD5.jpg|cover_image]]

Original Hellos AI杰哥 Hellos AI

继续滑动看下一个