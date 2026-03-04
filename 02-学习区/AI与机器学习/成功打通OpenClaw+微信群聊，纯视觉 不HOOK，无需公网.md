---
id: 8152faae-e1c4-43a9-a8db-fc7898501912
---

# 成功打通OpenClaw+微信群聊，纯视觉 不HOOK，无需公网
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247490037&idx=1&sn=599c293a169043c87efd301cf4bd2aa2&chksm=c3772e2cb99155a30f09b1098cd22cc5ea7169ac417f4f3d3b3e98b16efd6f9f1256a183687c&mpshare=1&scene=1&srcid=0303wAPatbm8R9uUTHXVIpAU&sharer_shareinfo=99d3cace6a42ea6003e5d5764ab83f8a&sharer_shareinfo_first=99d3cace6a42ea6003e5d5764ab83f8a#rd)
## 正文
公众号名称：猴哥的AI知识库

作者名称：AI小爱AI

发布时间：2026-03-03 11:20

![[笔记同步助手/images/6f3318ba0cb6d5a4227a68e39baa62bc_MD5.png]]

猴哥的第 214 期分享，欢迎追看

前文，分享了 `openclaw` 免费部署教程：

[永久免费 OpenClaw 部署和实战，7x24在线，手把手教程](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247489979&idx=1&sn=8a687785b81f53b1ce8ca20f1b4783cc&scene=21#wechat_redirect)

并成功接入`小智`，实现语音控制：

[小智Pro：让小智控制 OpenClaw，一个MCP连接海量Skills](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247490010&idx=1&sn=8f2265bc54632b8e1151612982e26e9a&scene=21#wechat_redirect)

相比飞书，QQ，微信才是国民级社交APP。

可以说，`OpenClaw+微信`，想象空间巨大。

**如何把 `openclaw` 接入微信？**

今日分享，给各位汇报下实现方案，先说结论：

-   不用第三方协议，不用HOOK
    
-   纯视觉方案，全程模拟人为动作
    
-   通过WebScoket和`openclaw`通信
    
-   无需公网，隐私不外露
    

大致流程：

![[笔记同步助手/images/1efc94cee59586d9849d1974ec1a336f_MD5.png]]

全文三部分：

-   微信自动化的技术方案盘点
    
-   Windows上安装OpenClaw
    
-   OpenClaw接入微信
    

## 1\. 微信自动化盘点

微信RPA的技术实现主要分为三类：

-   **UI 层模拟（模拟操作）**：基于Win系统UI接口，模拟人的点击和阅读。
    
-   **Hook/内存注入（内存修改）**：逆向微信PC客户端，通过DLL注入调用内部函数。
    
-   **协议逆向（网络模拟）**：逆向微信网络协议（Android/iPad），脱离客户端收发数据包。
    

怎么选？

| 技术路径 | 优点 | 缺点 | 稳定性 |
| --- | --- | --- | --- |
| UI 层模拟（模拟操作） | 封号风险极低、合规性好、开发简单、不侵入微信 | 依赖界面布局、速度慢、无法实时消息 | ⭐⭐⭐⭐⭐ |
| Hook/内存注入（内存修改） | 功能强、实时消息、可后台、响应快 | 微信版本更新易失效、有封号风险 | ⭐⭐⭐⭐ |
| 协议逆向（网络模拟） | 无界面、可云端多账号、效率高 | 封号风险极高、协议经常变、维护成本极大 | ⭐⭐ |

**平衡风险和收益，我们选用`UI 层模拟`。**

由于全程依赖 Windows PC 端微信客户端，

先在 `Windows` 上安装 `OpenClaw`。

怎么搞？

​

## 2\. Windows上安装OpenClaw

`OpenClaw`官方文档不建议 `Windows` 上使用`OpenClaw`，但就笔者目前体验而言，暂未发现巨坑。

还没体验过`OpenClaw`的朋友，强烈建议跑一遍：

[永久免费 OpenClaw 部署和实战，7x24在线，手把手教程](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247489979&idx=1&sn=8a687785b81f53b1ce8ca20f1b4783cc&scene=21#wechat_redirect)

这样，建立对`OpenClaw`的直观认知。

下面，

给出 `Windows` 安装 `OpenClaw` 的极简路径。

​

### 2.1 安装 Node

`OpenClaw` 已发布为 node.js包。

因此，需先安装node。

前往官网：

https://nodejs.org/en/download

下载.msi手动安装：

![[笔记同步助手/images/5094d18947c5f73a3761b54dae9cb9ea_MD5.png]]

打开powershell，看看是否安装成功：

```
node --version
v24.13.1
```

### 2.2 安装 OpenClaw

用**管理员身份**运行powershell：

```
# 安装
npm i -g openclaw@latest
# 如果下载失败，换个国内源
npm config set registry https://registry.npmmirror.com
```

### 2.3 配置 OpenClaw

安装成功后，需填写引导配置：

```
openclaw onboard --install-daemon
```

后续只要编辑配置文件，路径参考：

```
C:\Users\admin\.openclaw\openclaw.json
```

推荐打开 VsCode 等编辑器修改 `openclaw.json`，方便排查错误。

> PS：笔者的配置文件，需要参考的朋友，拉到文末自取。

### 2.4 熟悉 OpenClaw 终端命令

配置成功后，执行配置检查，看看有没有错误：

```
openclaw doctor --fix
```

然后，试试在命令行和它对话：

```
openclaw tui
```

成功后，就可以启动 gateway 了：

```
# 查看gateway都有哪些指令
openclaw gateway --help
# 前台运行gateway，用于查看失败日志
openclaw gateway run
# 后台运行gateway
openclaw gateway restart
# 查看状态
openclaw gateway status
# 停止gateway
openclaw gateway stop
```

gateway 成功启动，默认 18789 端口，打开 WebChat 控制台：

![[笔记同步助手/images/d2b53350e9c0834f9ae55b02b91e0f64_MD5.png]]

恭喜你，你的专属 `OpenClaw` 已就位。

​

### 2.5 给微信创建 Agent

为了和主 Agent 区分开，我们为`微信AI助理`创建一个独立的 Agent。

命令行中创建 agent （比如我的叫 `echo`）：

```
# 查看已有Agent
openclaw agents list
# 创建Agent并指定workspace
openclaw agents add echo --workspace C:\Users\admin\.openclaw\workspace-echo
# 测试一下是否正常通信
openclaw agent --agent echo --message "hello"
```

**踩坑提醒**：Windows上运行`openclaw`，请采用上述方式创建智能体，不要依赖`openclaw`自动创建，容易失败。因为Windows下的路径是问题重灾区，比如对话时收到报错：

```
run error: Error: Session file path must be within sessions directory
```

### 2.6 配置新 Agent 人设

每个 Agent 的人设配置在各自`workspace`下：

![[笔记同步助手/images/d58654186ddabeee069c671b6b2f7452_MD5.png]]

不同文件的用途说明：

```
workspace/
├── AGENTS.md      # 运行手册：每次启动该做什么
├── SOUL.md        # 行为宪法：当一个什么样的 bot
├── IDENTITY.md    # 身份档案：我是谁
├── USER.md        # 用户画像：我在服务谁
├── TOOLS.md       # 工具配置：专属Skills配置
├── HEARTBEAT.md   # 心跳任务：定期该检查什么
├── MEMORY.md      # 长期记忆：值得永久保存的
└── memory/    
    └── YYYY-MM-DD.md  # 日记：每天发生了什么
```

更新人设后，如果让它只负责回复消息，还得调教一番：

![[笔记同步助手/images/945ca379a60de0e590d64418b346bc7f_MD5.png]]

![[笔记同步助手/images/34a8171ec2cae2eb0b03b8f68b27fea4_MD5.png]]

## 3\. OpenClaw接入微信

原打算让 `OpenClaw` 动手用`UI 层模拟`方案实现一套`微信监管`的方案。

发现对于这种复杂任务，`OpenClaw`还是差点意思，效果不可控。

果断切换到`ClaudeCode`协作开发。

​

### 3.1 业务逻辑

极简四步：

![[笔记同步助手/images/a878ab64981bc8aff8a7cc233582b716_MD5.png]]

交互逻辑：

![[笔记同步助手/images/c9c82e7f8ad848d4e99ab3633606cd5a_MD5.png]]

### 3.2 架构设计

![[笔记同步助手/images/85b655aae33f7cb3f79f1ab3414b1ab0_MD5.png]]

| 层级 | 组件 | 职责 |
| --- | --- | --- |
| 应用层 | wxbot | 业务场景：好友监听、消息监听、群聊@回复等 |
| 通信层 | OpenClawClient | WebSocket 连接管理、请求响应匹配、流式事件处理 |
| 自动化层 | MainWnd | 微信主窗口控制（会话切换、消息收发、好友管理） |
| 基础设施 | uiautomation | Windows UI 自动化框架，模拟鼠标键盘操作 |
| 外部交互 | OpenClaw Gateway | 代理网关，管理 session 和流式输出 |

**关键步骤**：

-   **轮询机制**：通过截图检测红点，遍历会话列表实现消息监听
    
-   **消息去重**：用字典记录每个会话已处理的消息 ID，避免重复处理
    
-   **流式响应**：实时累加 OpenClaw 输出，发送给对应会话
    

### 3.3 测试效果

私聊窗口：

![[笔记同步助手/images/7b1ef8b9a3c29a097fcd513f66e9a8f7_MD5.png]]

群聊窗口：

![[笔记同步助手/images/3082e706fd5dec98a4638ea2307fac2a_MD5.png]]

监听好友申请，自动通过并欢迎：

![[笔记同步助手/images/4a5d33db98b113ae2fbc59433ed8e172_MD5.png]]

## 写在最后

本文分享了`OpenClaw接入微信`的一种实现方案。

如果对你有帮助，不妨**点赞收藏**备用。

目前，机器人已接管体验群，欢迎进群体验。

> 另：为方便大家在Windows上快速跑通`openclaw`，配置文件已放云盘，供需要的朋友参考，公众号后台回复`openclaw`自取，免费。

文中的**微信自动化+OpenClaw**，是一个完整的 RPA 解决方案，持续迭代中。

感谢支持，欢迎链接。

**👇** **关注猴哥，**快速入门AI工具****

![[笔记同步助手/images/81022500d9f2fbdafe2e166231c876ba_MD5.gif]]

**\# AI 工具：**

[本地部署大模型？看这篇就够了，Ollama 部署和实战](http://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247485523&idx=1&sn=3db148b7653d579e2e5f94da026ef30d&chksm=c24855dbf53fdccd807c7a365a7c53a84d1f01d096671398a795da0ea3fdd01b43751d3aa552&scene=21#wechat_redirect)

[免费GPU算力本地跑DeepSeek R1，无惧官方服务繁忙！](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247488515&idx=1&sn=cb678a613e12ea651c67655602da6eea&scene=21#wechat_redirect)

[永久免费 OpenClaw 部署和实战，7x24在线，手把手教程](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247489979&idx=1&sn=8a687785b81f53b1ce8ca20f1b4783cc&scene=21#wechat_redirect)

**\# AI **应用**：**

[弃坑 Coze，我把 Dify 接入了个人微信，AI小助理太强了](http://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247486477&idx=1&sn=328276abc086dc8ede353ddb07a9b480&chksm=c2485185f53fd89302307eda9f26a4eb14e514222731136e89ec876885840e9c3c87b9ea69c7&scene=21#wechat_redirect)

[阿里开源TTS CosyVoice 再升级！语音克隆玩出新花样，支持流式输出](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247488280&idx=1&sn=d0c9349fd02f856902525ee10a6a52ba&scene=21#wechat_redirect)

[借 WeChatFerry 东风，我把微信机器人复活了！](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247488453&idx=1&sn=fe93116db2f7224af9bae47f68e561ae&scene=21#wechat_redirect)

[腾讯开源多模态 RAG：复杂文档秒变自建知识库，支持 API 调用](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247489658&idx=1&sn=fc9e1c24e4877131e6ff5eaa0b60acca&scene=21#wechat_redirect)

**\# 小智 AI：**

[成本不到50的AI对话机器人，如何自建服务端？自定义角色+语音克隆](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247488629&idx=1&sn=c554c2f22836d905cc71097531611c64&scene=21#wechat_redirect)

[成本低至1.5元/天，小智AI服务端，完整解决方案，高可用+可扩展](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247489471&idx=1&sn=d178bdde096e17a5a374325380718825&scene=21#wechat_redirect)

[零门槛为小智接入MCP，小智Pro焕新上线：MCP广场+自定义服务](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247489799&idx=1&sn=6b8ecaf88ccadf9c4b4f6f3a0d1e0cae&scene=21#wechat_redirect)

[远程控制+文字唤醒，小智Pro开放API调用，释放小智无限潜力](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247489857&idx=1&sn=aa5d72055766ed7c198b226e03d7499c&scene=21#wechat_redirect)

[小智Pro：接入长期记忆，一个更懂你、有灵魂的小智](https://mp.weixin.qq.com/s?__biz=MzkzMzY2MTkyNw==&mid=2247489930&idx=1&sn=97c1be641aec6ef54fcea247651804b0&scene=21#wechat_redirect)

---

![[笔记同步助手/images/1de4be4330e0e6a02e438f789cc8c970_MD5.jpg|cover_image]]

Original AI小爱AI 猴哥的AI知识库

继续滑动看下一个

![[笔记同步助手/images/483c059bb115552756dc754d605654ea_MD5.png]]

猴哥的AI知识库

向上滑动看下一个