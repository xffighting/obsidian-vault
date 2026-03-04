---
id: 11ae4f2c-9f78-45c3-a514-4857b83b2d1c
---

# 小龙虾 （OpenClaw） 实操指南：我用 4 个血泪大坑，喂出了最强 AI 助手
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s?__biz=MzkwMzc1MzI0NA==&mid=2247497815&idx=1&sn=b488763c4d3fda301bd70da36ae8c271&chksm=c14cd066531d7a0a4e8ed6fe390181c23827ff73ff4a33023e8fb76c4e8d9b66f1690d89315b&mpshare=1&scene=1&srcid=0304Xzr9NzCaFG8D2xmUieTu&sharer_shareinfo=195ad7c8448b1e7c2eca99cb4a2fcaa1&sharer_shareinfo_first=195ad7c8448b1e7c2eca99cb4a2fcaa1#rd)
## 正文
公众号名称：萝卜AI笔记

作者名称：周萝卜

发布时间：2026-03-03 10:41

大家好，我是你们的萝卜哥～

今天的文章，是我个人小龙虾实践的真实记录和各种踩坑血泪史，发出来主要还是为了给大家打个样，看看小龙虾到底该怎么用。

这几天总有朋友跑来问我：“小龙虾（OpenClaw）到底能用来干嘛？”

其实，我们看待这个问题的出发点需要转变。我们更应该主动思考自己想让小龙虾做什么，打破它只能做什么的思维局限。

每个人的业务诉求千差万别，亲手调教出一个完美契合自己工作流的专属 AI，才理应成为我们的最终目标。

现在满大街都是让小龙虾整理桌面、写文章的教程。这仅仅因为它们是极佳的入门演示案例，绝对代表不了小龙虾的全部实力。

师傅领进门，修行靠个人，没有任何人能穷尽所有的使用场景。

所以今天萝卜哥就完完整整的带着大家实操一遍小龙虾的具体案例，没有什么高大上的概念，都是我的一手经验和血泪教训。

这可能不是最好最优质的教程，但一定是最走心的一个！

说一下这次小龙虾搭建大环境，我选择的是阿里云，因为目前阿里云正在做活动，一个 2C2G 的海外服务器一年竟然只要不到 100 元，这个价格是以前想都不敢想的啊。

​

![[笔记同步助手/images/8c6e6df9788369c11765609d2fb08fce_MD5.png]]

还有它们推出的 Coding Plan 服务，一个月的大模型 API 调用套餐，第一个月只需要 7.9 月，足够玩了。

所以如果你想着先玩一玩小龙虾，那么真的可以看看阿里的这个套餐，超划算。

https://www.aliyun.com/activity/ecs/clawdbot?userCode=l0uorftk

​

![[笔记同步助手/images/bd0f5746b2f8e0e74965fba9a91b0e5d_MD5.png]]

然后下面这张图片是我让小龙虾跑了一夜任务的结果，还是非常耐用的。

​

![[笔记同步助手/images/8bd0293a072260fd62020b12bdac98e2_MD5.png]]

服务器资源情况，货真价实的漂亮国节点，做什么都非常方便，懂的都懂。

​

![[笔记同步助手/images/445e728619de70b94bc6b28d86a16ff6_MD5.png]]

服务器和大模型套餐购买完毕之后，我们就可以开始搭建小龙虾服务了。

这里有个巨坑，就是不要看阿里云上自带的搭建教程，那个东西真的是坑了我整整一下午，启动服务器报错，自动写入的配置文件参数不匹配，当时我人都傻了。

​

![[笔记同步助手/images/13f62149e63b62fc990eaf54a8a4c1d6_MD5.png]]

就是这个教程，千万别信。为了防止大家误解，我就不贴链接了，总之绕着走就行了。

​

![[笔记同步助手/images/324c2771a94965f491d1838e5a3f8c2e_MD5.png]]

后来我询问了在线售后，拿到的那个教程才想点样子，配置起来也非常顺利。

​

![[笔记同步助手/images/8ae8a5b72d4599da6405251dfb4de753_MD5.png]]

其实很简单，看下图，只需要进入轻量级服务器控制台，在应用详情里面配置就好，先是执行三条命令，然后配置 IM 软件鉴权信息就好。

​

![[笔记同步助手/images/d5d2f6cca703e268ed9a6e91818434ca_MD5.png]]

如果大家想要配置钉钉机器人，还是直接在线问售后机器人就好，那个教程我验证了，是正确的。

​

![[笔记同步助手/images/f103259a97fa3d9dfe38987d6952c4ce_MD5.png]]

如果你想要接入飞书机器人，还是看下面这个教程，是萝卜哥一步一步走过来的。

[别买 Mac mini 了！我用 38 块钱的服务器，接通了最强的 AI 助理。附完整 Clawdbot 部署教程](https://mp.weixin.qq.com/s?__biz=MzkwMzc1MzI0NA==&mid=2247497106&idx=1&sn=8dc3e8021ebff3af99989d96d483ba14&scene=21#wechat_redirect)

​

![[笔记同步助手/images/06d0beba70213359561886996602ed08_MD5.png]]

对于权限配置，可以直接用下面的 JSON 导入就行，能方便很多。

​

```
{
  "scopes":{
    "tenant":[
      "aily:file:read",
      "aily:file:write",
      "application:application.app_message_stats.overview:readonly",
      "application:application:self_manage",
      "application:bot.menu:write",
      "contact:contact.base:readonly",
      "contact:user.employee_id:readonly",
      "corehr:file:download",
      "event:ip_list",
      "im:chat.access_event.bot_p2p_chat:read",
      "im:chat.members:bot_access",
      "im:message",
      "im:message.group_at_msg:readonly",
      "im:message.p2p_msg:readonly",
      "im:message:readonly",
      "im:message:send_as_bot",
      "im:resource"
    ],
    "user":[
      "aily:file:read",
      "aily:file:write",
      "contact:contact.base:readonly",
      "im:chat.access_event.bot_p2p_chat:read",
      "im:message"
    ]
}
}
```

然后如果你像我一样有要求机器人读写飞书文档的诉求，那么可以导入下面的这些权限

​

```
{
  "scopes":{
    "tenant":[
      "aily:file:read",
      "aily:file:write",
      "application:application.app_message_stats.overview:readonly",
      "application:application:self_manage",
      "application:bot.menu:write",
      "bitable:app",
      "bitable:app:readonly",
      "board:whiteboard:node:create",
      "board:whiteboard:node:delete",
      "board:whiteboard:node:read",
      "board:whiteboard:node:update",
      "contact:contact.base:readonly",
      "contact:user.employee_id:readonly",
      "corehr:file:download",
      "docs:doc",
      "docs:doc:readonly",
      "docs:document.comment:create",
      "docs:document.comment:read",
      "docs:document.comment:update",
      "docs:document.comment:write_only",
      "docs:document.content:read",
      "docs:document.media:download",
      "docs:document.media:upload",
      "docs:document.subscription",
      "docs:document.subscription:read",
      "docs:document:copy",
      "docs:document:export",
      "docs:document:import",
      "docs:event.document_deleted:read",
      "docs:event.document_edited:read",
      "docs:event.document_opened:read",
      "docs:event:subscribe",
      "docs:permission.member",
      "docs:permission.member:auth",
      "docs:permission.member:create",
      "docs:permission.member:delete",
      "docs:permission.member:readonly",
      "docs:permission.member:retrieve",
      "docs:permission.member:transfer",
      "docs:permission.member:update",
      "docs:permission.setting",
      "docs:permission.setting:read",
      "docs:permission.setting:readonly",
      "docs:permission.setting:write_only",
      "docx:document",
      "docx:document.block:convert",
      "docx:document:create",
      "docx:document:readonly",
      "docx:document:write_only",
      "drive:drive",
      "drive:drive.metadata:readonly",
      "drive:drive.search:readonly",
      "drive:drive:readonly",
      "drive:drive:version",
      "drive:drive:version:readonly",
      "drive:export:readonly",
      "drive:file",
      "drive:file.like:readonly",
      "drive:file.meta.sec_label.read_only",
      "drive:file:download",
      "drive:file:readonly",
      "drive:file:upload",
      "drive:file:view_record:readonly",
      "event:ip_list",
      "im:chat.access_event.bot_p2p_chat:read",
      "im:chat.members:bot_access",
      "im:message",
      "im:message.group_at_msg:readonly",
      "im:message.p2p_msg:readonly",
      "im:message:readonly",
      "im:message:send_as_bot",
      "im:resource",
      "sheets:spreadsheet",
      "sheets:spreadsheet.meta:read",
      "sheets:spreadsheet.meta:write_only",
      "sheets:spreadsheet:create",
      "sheets:spreadsheet:read",
      "sheets:spreadsheet:readonly",
      "sheets:spreadsheet:write_only",
      "slides:presentation:create",
      "slides:presentation:read",
      "slides:presentation:update",
      "slides:presentation:write_only",
      "space:document.event:read",
      "space:document:delete",
      "space:document:move",
      "space:document:retrieve",
      "space:document:shortcut",
      "space:folder:create",
      "wiki:member:create",
      "wiki:member:retrieve",
      "wiki:member:update",
      "wiki:node:copy",
      "wiki:node:create",
      "wiki:node:move",
      "wiki:node:read",
      "wiki:node:retrieve",
      "wiki:node:update",
      "wiki:setting:read",
      "wiki:setting:write_only",
      "wiki:space:read",
      "wiki:space:retrieve",
      "wiki:space:write_only",
      "wiki:wiki",
      "wiki:wiki:readonly"
    ],
    "user":[
      "aily:file:read",
      "aily:file:write",
      "contact:contact.base:readonly",
      "im:chat.access_event.bot_p2p_chat:read",
      "im:message"
    ]
}
}
```

好了现在我就算是你已经完成配置了哈，你在钉钉或者飞书上已经可以和机器人正常对话了哦～

下面来逐条展示我和小龙虾的对话过程，我觉得这个过程是非常宝贵的，也是现在市面上大多数教程都没有展示的。

其实我作为一名小龙虾初学者，这个对话过程应该也是大多数人的心中疑惑，今天完全公开给大家，相互学习吧，我也不怕丢脸了，哈哈哈哈。

很多大佬展示的都是最终高大上的成品小龙虾，背后调测的痛苦，谁人能懂啊。

首先我想要让小龙虾自己写一套 OpenClaw 的教程，所以我先给出的指令很直接。

​

![[笔记同步助手/images/455957bbff6057466f1e1b97528d1254_MD5.png]]

然后小龙虾就开始库库的干活，虽然说也给出了大纲，但是感觉还是欠缺些什么，所以我开始优化指令，给它更多资料来参考。

这里我给了它四个权威的官方网站让它去学习参考，然后我不再急着让它写教程了，而是让它思考如果要完成这个任务，它需要怎么做，缺少什么 skill 自己去整理。

​

![[笔记同步助手/images/e01b42dc99dfe19155c6e2ee1ec6ba30_MD5.png]]

接下来它在安装 skill 的过程中遇到问题想要逃避跳过，我果断拒绝并且在不断催促下成功完成了所需 skill 的安装。

​

![[笔记同步助手/images/37c775c5d58ed2dc68b74e71057bddcc_MD5.png]]

![[笔记同步助手/images/55b01a1e65e2cde02042985c13def26f_MD5.png]]

在接下来我觉得时机成熟了，就开始让小龙虾写教程

​

![[笔记同步助手/images/7519eca3dfcad30215feb26d888d2c40_MD5.png]]

开始还是挺顺利的，写出来的内容也不错，只不过在自动写入飞书文档的过程中，卡了挺久的。

​

![[笔记同步助手/images/5b5ab061725c138a84c9010d3bccb62f_MD5.png]]

其实就是前面权限给的不够，把权限都勾选好就可以了。

​

![[笔记同步助手/images/1a3a5f4bacf2f113d2a7da2135165707_MD5.png]]

再接下来我发现它写入飞书文档的内容顺序是错乱的，它说是调用飞书 API 的时候不好控制，这块我尝试了很久也没有最终解决。

​

![[笔记同步助手/images/826b017a6e8f7a40aa516c662165ef13_MD5.png]]

如果大家有什么好的办法，可以交流一下，非常感谢。

​

![[笔记同步助手/images/28eb3e39c440113e2224bdf168ea56ec_MD5.png]]

最后没有办法我还是妥协了，选择了让它先给出 markdown 文档，然后我手工粘贴的方式。

​

![[笔记同步助手/images/f8050fdde862b64e02f98cdd45c50a09_MD5.png]]

然后我就安心睡觉去了。

第二天起来我看到飞书对话框里好多 markdown 内容，复制起来超级麻烦，就想再更新一下命令让它生成 markdown 文件，让它把文件保存到本地，这样要容易复杂一些。

结果它却失忆了，什么都不记得了。

​

![[笔记同步助手/images/261c6d46866eb4dd66202357c84fcc16_MD5.png]]

天塌了，可能是服务器配置太低的原因，夜里 OpenClaw 竟然重启了，还丢失了所有的记忆。

​

![[笔记同步助手/images/f9b81d0392b75f453a14186b758e77ab_MD5.png]]

现在没有什么好办法了，要么让小龙虾重新写一遍教程，要么就只能自己手工在飞书对话框里复制教程内容了。

​

![[笔记同步助手/images/ca61c2cc3f68389aee16c79ea166b41e_MD5.png]]

这次记忆丢失对我来说有点痛苦，本来是想着一早起来收获果实的，结果。。。

所以我赶快亡羊补牢，让 OpenClaw 给出解决方案，你看小龙虾本身就设计了记忆功能，只不过我没有及时开启而已。

​

![[笔记同步助手/images/419a8035c40a85c5eb2aafa9ce6ed3e8_MD5.png]]

现在我再也不用担心小龙虾重启而丢失记忆了。

最后大家如果需要这份教程，可以在后台回复“龙虾”获取哈。

​

![[笔记同步助手/images/04e02381059ace4d74c2fbeb6af2322e_MD5.png]]

我觉得这份教程的意义是，可以让新人快速了解小龙虾，也可以直接投喂给小龙虾让它参考着输出一版更优质的教程！

虽然在整个实践过程中我踩了不少坑，但是我从中也学到了很多，这个才是最宝贵的，也是我们养虾的本质。

哦对了，上面的教训，也从侧面印证了下面这篇文章，大神们总结的神级 Skills，还是非常有必要及时安装的！！！

2026 OpenClaw 封神开局：新手必装的 10 大“满血”神级 Skills 技能包

踩了这么多坑，有人可能会问：折腾这些值得吗？去用现成的网页端工具不香吗？

工具到底好不好用，唯有亲自下场体验才能见分晓。请坚信：你现在为了驯服工具所浪费的时间，都会在未来以百倍的效率回馈给你。

养虾的过程，本质上就是一场充满成就感的养成游戏。

你看着它从一开始呆头呆脑、只会报错的弱鸡，在你的悉心调教下，一步步变成对你言听计从、极其能干的超级外脑。

给它一点成长的时间，同时也是给自己留出一个认知升级的空间。你看我不就是从血泪中学到了要及时开启 OpenClaw 的记忆功能吗。

不要因为 AI 眼下的笨拙，就轻易否定它的未来；你亲手喂养的代码和记忆，终将成为你在数字时代最坚不可摧的铠甲。

最后的最后想说，因为本人水平有限，教程难免有疏忽错误的地方，如果有任何问题，可以私聊萝卜哥，我也建立了一个 OpenClaw 的学习交流群，免费加入的，后台回复“openclaw”加入哦～

---

以上就是今天的分享，觉得有帮助，帮请帮一键三连：**点赞、转发，再看**和**留言**，你的反馈对我很重要！

  

---

![[笔记同步助手/images/e8a4fcc472f8ea18e9cac9c0eaaaacb5_MD5.jpg|cover_image]]

原创 周萝卜 萝卜AI笔记

继续滑动看下一个

![[笔记同步助手/images/852f7a2ea2b2f7f9c2f2a266615bc4e7_MD5.png]]

萝卜AI笔记

向上滑动看下一个