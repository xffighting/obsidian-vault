---
id: 9a34ae71-3ab5-47ac-a1d0-e5021b861b3d
---

# 如何用OpenClaw + 飞书，组建一支"AI团队"？
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488818&idx=1&sn=40d4f84a37312085b28c46e86920519f&chksm=fa3b124fb9fea5fa0b6b76d982ff89e1e323cc3ed50a01416989ec933215be6c79be01cb231b&mpshare=1&scene=1&srcid=0304ax5BcS6gbUjgUpbUgB8p&sharer_shareinfo=51c96176f08a54510ac79d139f66c63c&sharer_shareinfo_first=51c96176f08a54510ac79d139f66c63c#rd)
## 正文
公众号名称：骁哥AI编程

作者名称：骁哥AI编程

发布时间：2026-03-01 19:26

上期中，我们讲了：如何使用纯国产化，快速组装出自己的OpenClaw​👉[纯国产化OpenClaw最佳搭建攻略：腾讯云 + 飞书 + 阿里百炼CodingPlan（超详细攻略）](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488763&idx=1&sn=b6df84c2f8c662c50c1012a52d121cd2&scene=21#wechat_redirect)

本期我们来讲讲，如何使用OpenClaw + 飞书，像这样，组建属于自己的“一人公司”💁

![[笔记同步助手/images/0ee97c928ba971384110387dfc8623ee_MD5.png]]

登服务器，用命令行新建角色

如果你还没有自己的openclaw，先跟着文章开头的文章一步步先把openclaw搭起来，再回到这里

为了照顾新人读者，骁哥尽可能讲的详细一点。老观众可以快进🙇

还是用orcaterm登录腾讯云服务器👇

https://orcaterm.com/

![[笔记同步助手/images/d4d3999949cb0bb6768358c4a3fed108_MD5.png]]

左下角登录

![[笔记同步助手/images/f8336bfac7004452c1f50e80d3864daf_MD5.png]]

选择「腾讯云账号登录」，会拉起网页进行登录

![[笔记同步助手/images/540f13b79f68c0d81310f790acd11c7f_MD5.png]]

登录成功后，左边第二个Tab，点击清凉应用服务器

![[笔记同步助手/images/d85e600affb169a3c241eab0b5d7f7ab_MD5.png]]

这就是我们刚买的服务器了👇，点进去

![[笔记同步助手/images/19ab3163f90e5ac6637e1c4486e8963e_MD5.png]]

直接点登录进入服务器

![[笔记同步助手/images/02ff6ca0867aedd0a85eed1104ed36fe_MD5.png]]

这时，我们就要开始招募“新员工”了😃

骁哥这次想找一个帮骁哥理财的📈

输入👇

```
openclaw agents add 新员工ID
```

跟着骁哥走

![[笔记同步助手/images/db61a38d6e4d9ad3ba5794eefedf0610_MD5.png]]

这里直接确认

![[笔记同步助手/images/60d34c331b6709bc2f7d3f42c277b57f_MD5.png]]

不用

![[笔记同步助手/images/790f0b99ef969034af451e9a868d90f8_MD5.png]]

不用

![[笔记同步助手/images/acd13f9809205f5fe27cfbd031c2c4c0_MD5.png]]

这一套指令执行下来，最终会修改到这三处地方

![[笔记同步助手/images/3ccc730e91b7a25cd5883fc6ca513840_MD5.png]]

我们点击一下刷新按钮

![[笔记同步助手/images/81daa3c08d012a6bb192c3d0a2e26721_MD5.png]]

发现新的workspace已经自动建好了，这就是我们“新员工”的“办公场地”🤓

![[笔记同步助手/images/af6228f80aa71f8c15ee2f5ca245278a_MD5.png]]

同样agents目录下，也自动建好了👇

![[笔记同步助手/images/7763a01ec470b7a05bbaf0bcc0ee51e1_MD5.png]]

我们打开openclaw.json配置文件，发现`agents.defaults`多了一个list字段，这些都是通过指令，自动生成的，不用改👇

![[笔记同步助手/images/a2496b1d9299ec3cb0d22383ff2ac7a8_MD5.png]]

去飞书，再新建一个机器人

进入飞书控制台，点击「创建应用」

https://open.feishu.cn/app?lang=zh-CN

![[笔记同步助手/images/b4714b59332b31b2ed698bf118afd049_MD5.png]]

新建一个机器人

![[笔记同步助手/images/ae8f6a0fd74ca17dabc11d9942fc745e_MD5.png]]

然后我们这里，先不用着急配置，只需记下`App ID`和`App Secret`即可🧏

![[笔记同步助手/images/c08cc5a5683ecfbf092637d215147698_MD5.png]]

配置openclaw.json

好，我们再回到openclaw.json配置文件，找到“channel”这个配置。没改造前，应该是长这个样子的👇

![[笔记同步助手/images/9deb6395867358bad8e2c8ea31a683e3_MD5.png]]

现在我们要对channels字段，进行改造👇

```
"channels": {
    "feishu": {
      "enabled": true,
      "domain": "feishu",
      "groupPolicy": "open",
      "accounts": { // 新增accounts字段
        "main": { // 之前的bot
          "appId": "", // 之前bot的appId
          "appSecret": "" // 之前bot的appSecret
        },
        "chaogu": { // 新申请的botId
          "appId": "", // 新bot的appId
          "appSecret": "" // 新bot的appSecret
        }
      }
    }
  },
```

把刚刚在飞书新申请的机器人的`App ID`和`App Secret`，填进去，效果如下👇

![[笔记同步助手/images/c39d3ddb43d785dea940cc180a0fcaf0_MD5.png]]

然后，在最外层，也就是和channels的下面，我们再新增一个配置：bindings，配置如下👇

```
"bindings": [
    {
      "agentId": "main",
      "match": {
        "channel": "feishu",
        "accountId": "main"
      }
    },
    {
      "agentId": "chaogu",
      "match": {
        "channel": "feishu",
        "accountId": "chaogu"
      }
    }
  ],
```

效果如下👇

![[笔记同步助手/images/7f282a332d081df2d22e5b345f90911c_MD5.png]]

然后，再在最外层，也就是和bindings的下面，我们再新增一个配置：tools，配置如下👇

```
"tools": {
    "agentToAgent": { // 开启Agent之间对话
      "enabled": true,
      "allow": ["main", "chaogu"]
    },
    "sessions": { // 这个很重要！
      "visibility": "all"
    }
  },
```

效果如下👇

![[笔记同步助手/images/cecbdeb8f6078e5d9aea4184848a1fcf_MD5.png]]

完整配置给大家，大家对照检查下💁

```
"channels": {
    "feishu": {
      "enabled": true,
      "domain": "feishu",
      "groupPolicy": "open",
      "accounts": {
        "main": {
          "appId": "",
          "appSecret": ""
        },
        "chaogu": {
          "appId": "",
          "appSecret": ""
        }
      }
    }
  },
  "bindings": [
    {
      "agentId": "main",
      "match": {
        "channel": "feishu",
        "accountId": "main"
      }
    },
    {
      "agentId": "chaogu",
      "match": {
        "channel": "feishu",
        "accountId": "chaogu"
      }
    }
  ],
  "tools": {
    "agentToAgent": {
      "enabled": true,
      "allow": ["main", "chaogu"]
    },
    "sessions": {
      "visibility": "all"
    }
  }
```

记得保存哟～

![[笔记同步助手/images/cf78bbdc979b913f2a65a13d4ae2c57b_MD5.png]]

重启一下，让配置生效

![[笔记同步助手/images/6f635acd72f6659fc54968b451061cfa_MD5.png]]

配置飞书机器人

好，现在再回过头来，我们继续配置我们的新员工😀

进入后会来到这个界面，先点击「+添加」机器人

![[笔记同步助手/images/d64dfb4af2789984f7b7a9611f267e42_MD5.png]]

成功后机器人会出现在左侧边栏👌

![[笔记同步助手/images/7b8e385ef374fb29215e75037cd3c220_MD5.png]]

左侧边栏「权限管理」→ 「开通权限」，**把“通讯录”、“消息与群组”、“云文档”和“多维表格”下面所有权限全开了，然后点击「确认」**

![[笔记同步助手/images/287db8037da4f93966cdc9835f90f277_MD5.png]]

**OK，可以发布了，飞书所有的改动一定记得要发布哦！不然不生效！**🧏

![[笔记同步助手/images/eda74cb87e992f7119e882d3a6b206ac_MD5.png]]

左侧菜单点击「事件与回调」，然后点击这个“小铅笔”

![[笔记同步助手/images/1f0dd56a74d7d99d414be7f29aa030e6_MD5.png]]

直接点击「保存」

⚠️注意​：如果这一步报错提示“应用未建立长连接”，请检查前面步骤中的机器人App ID和App Secret是否已正确配置。

![[笔记同步助手/images/e2cd2e1c997d43fc276b01fc26a10fe7_MD5.png]]

然后点击「添加事件」

![[笔记同步助手/images/6ecbb0e7f2b384c831d5c1be2f8d80f9_MD5.png]]

**把“通讯录”、“消息与群组”、“云文档”里所有的权限全勾选上**✅

![[笔记同步助手/images/25e1983ac72e78fc25c0d4eef74356f1_MD5.png]]

**记得发布！**

![[笔记同步助手/images/e03695705e363299bd47b11abecac9ee_MD5.png]]

Channel配对

在飞书聊天里，搜索我们刚刚建好的机器人

![[笔记同步助手/images/424819f089b98e640db260c6f5033dd3_MD5.png]]

跟他随便说点啥，**他会给我们吐一串神秘token（如红框），我们记住这个**

![[笔记同步助手/images/6210755558216bd2221d791f374efe92_MD5.png]]

**再到服务器，命令行里输入**

```
openclaw pairing approve feishu 你的token
```

好，**如果他返回的这个“ou\_xxx”和飞书聊天中他吐的“ou\_xxx”一致，说明成功在即！！**🤓

![[笔记同步助手/images/778b66311683d71b9ad21b0ceba5ae6c_MD5.png]]

重启一下

![[笔记同步助手/images/dff1b8abec4d93c05468c788e8229761_MD5.png]]

**哈哈，我们的新员工来啦！**🎉🤩

![[笔记同步助手/images/5dd33c4e0cdf74db3af2acdb658c0ff9_MD5.png]]

实战演示

创建群组

![[笔记同步助手/images/5160dac70a17ee65625838a973a2f8ef_MD5.png]]

这里，添加群机器人

![[笔记同步助手/images/11c376e9b3695a24d21aecdc5881e0bd_MD5.png]]

添加

![[笔记同步助手/images/ad314a725a3aea33ecf25fb3a46904c3_MD5.png]]

把我们的员工，都拖进来！（为啥叫第二天团？因为骁哥第一天团在telegram😀可以翻下往期文章）

![[笔记同步助手/images/cae21a9672ec9a70a86bdebd7e6d5034_MD5.png]]

但感觉还差点意思...🤔

舒服了😌👇（改了下机器人的名称和头像）

![[笔记同步助手/images/c3c52651ac070324d63c9cb7a8a0ff4d_MD5.png]]

**都给我出来报道！**

![[笔记同步助手/images/9b1382f834ecc7c9af69ce4e6255407e_MD5.png]]

想要Agent之间“私密聊天”～？骁哥教你！🤓

直接单聊

![[笔记同步助手/images/b3e53b6ddb9b20ccdea79de0f6f96be4_MD5.png]]

session\_send是agent的内部通信通道，我们看不见，但他们确实说了悄悄话🤣

![[笔记同步助手/images/13a737aff6338e3069cb6f738bf328c8_MD5.png]]

小结

**快去组建你的 “亿人公司🦞”吧！**

关注骁哥，为你带来更多openclaw实战技巧

那本期就这样，我是🦞openclaw驯兽师骁哥，下期见，88👋🤓

![[笔记同步助手/images/d499152b2fb7b54285d377e9de5830a2_MD5.png]]

**往期实践**👇

[纯国产化OpenClaw最佳搭建攻略：腾讯云 + 飞书 + 阿里百炼CodingPlan（超详细攻略）](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488763&idx=1&sn=b6df84c2f8c662c50c1012a52d121cd2&scene=21#wechat_redirect)

[我没做任何指示，OpenClaw自行迭代，然后擅自发布了我的网站？？](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488700&idx=1&sn=ecc69b8478f436216d89c556b17e4ddd&scene=21#wechat_redirect)

[OpenClaw 突然不说话了？别慌，90%的问题都在这里](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488654&idx=1&sn=a5c7dce2d0766e6046533e64d0d1b71d&scene=21#wechat_redirect)

[Vibe Coding之父：OpenClaw是 AI 的第三阶段？](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488645&idx=1&sn=28fbde92bd5d3e9f44614d0a166ea4e9&scene=21#wechat_redirect)

[如何用OpenClaw，写出精美飞书文档](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488626&idx=1&sn=6a2a24baef67a4fe7b23faede9da8544&scene=21#wechat_redirect)

[我做的网站，SEO排名冲到了第一，就靠这个Skill](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488600&idx=1&sn=2dbe2aa46960c8f065ed6f2d2f975072&scene=21#wechat_redirect)

[如何让 OpenClaw 主动找你搭话（发日报）？Heartbeat + Cron 终极攻略](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488581&idx=1&sn=b10d252832618b628b3333002b1ced89&scene=21#wechat_redirect)

[OpenClaw之父加入OpenAI！从小镇少年到改变世界，这哥们的故事比电影还燃](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488561&idx=1&sn=9c3a41f326372eff43f674cd831fc2d5&scene=21#wechat_redirect)

[如何用OpenClaw，搭建一支"AI团队"？](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488533&idx=1&sn=c9f630c3b45776a95be33ebf03c34813&scene=21#wechat_redirect)

[OpenClaw，给新人的几个超实用小技巧（可能大部分人都会踩的坑）](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488516&idx=1&sn=7b4e8512e54e5798b5cf9669064fa377&scene=21#wechat_redirect)

[从零搭建OpenClaw，腾讯云+Telegram方案](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488490&idx=1&sn=6a10cc94ec999e4d1ab6b0b450e17726&scene=21#wechat_redirect)

[为啥OpenClaw火成这样？用ChatGPT、Claude Code不香吗？](https://mp.weixin.qq.com/s?__biz=MzU0MTkwOTUyMA==&mid=2247488430&idx=1&sn=b0b8ff0af2cd9a2157c53a40acf0f795&scene=21#wechat_redirect)

---

![[笔记同步助手/images/01fa978fdec8f786c075e2d429073073_MD5.jpg|cover_image]]

原创 骁哥AI编程 骁哥AI编程

继续滑动看下一个

![[笔记同步助手/images/b87f170df8e233af1150bb33139e4500_MD5.png]]

骁哥AI编程

向上滑动看下一个