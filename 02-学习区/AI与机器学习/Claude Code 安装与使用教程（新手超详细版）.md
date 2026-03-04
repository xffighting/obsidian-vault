---
id: 1b15dd27-cbd0-4dc6-bfae-931126833e34
---

# Claude Code 安装与使用教程（新手超详细版）
#笔记同步助手
## 来源
[原文链接](https://mp.weixin.qq.com/s?__biz=MzY5MTAwMTA2Ng==&mid=2247484140&idx=1&sn=da605482b4fc6015295e417b149576c4&chksm=f5eb39d8601f61cfc42a6722f885cb437c68564575ddba27b0c4bd8a80963ca6b1397eae1ecd&mpshare=1&scene=1&srcid=0304keATYNNSj80Hw4Cckcbn&sharer_shareinfo=f42f1bd68ad8e0f4076683dd6f06baee&sharer_shareinfo_first=f42f1bd68ad8e0f4076683dd6f06baee#rd)
## 正文
公众号名称：01fish

作者名称：

发布时间：2026-02-20 13:17

  

# Let us go~

> 本教程由团队里的 00 后小伙伴整理编写，手把手带你从零开始安装 Claude Code。全程截图 + 文字说明，即使你是完全没接触过命令行的新手，也能跟着一步步搞定。

---

## 0\. 关于 API 的选择：Claude Code + 中转站方案

在正式开始安装之前，先聊一个大家最关心的问题——**API 怎么买最划算？**

国产模型 glm4.7和glm5已经足够好，也很适合新手上手。

但本文主要推荐直接使用 claude 模型，用最好的模型，最节省时间。

各种模型都配置上，根据不同任务来分配不同模型干活，下一阶段推荐。

Claude 目前主流方案有两种：

| 方案 | 优点 | 缺点 |
| --- | --- | --- |
| 官方直连 API 或者账号 | 最稳定、延迟最低 | 需要海外信用卡，价格较高，或者到淘宝闲鱼买账号成本也较高，仍有封号风险 |
| **中转站 API** | **性价比高、支付方便（支持支付宝/微信）** | 稳定性取决于中转站质量 |

**目前的结论：Claude Code + 中转站的官方 API，是当前性价比最高的选择。**

关于如何选购中转站 API，可以参考这个导航站，里面汇总了主流中转站的对比信息：

​

> 中转站导航：https://relaypulse.top/

**实测推荐——FoxCoder**

经过多家对比 **FoxCoder**，整体性价比最高，响应速度和稳定性都不错：

​

> FoxCoder 注册地址：https://foxcode.rjj.cc/auth/register?aff=Y2WMDL2

**实用建议：不要把鸡蛋放在一个篮子里。** 建议同时注册 2～3 家中转站，按需调配使用。当某家出现波动或额度用完时，可以随时切换，保证工作不中断。配置方法非常简单——只需要替换环境变量中的 URL 和 Token 即可（后面会详细教。

​

---

下面分 windows 版本和 mac 版本 两个版本来介绍。

![[笔记同步助手/images/fc9368702e4b6c32ef67f3eb62466b8a_MD5.png]]

## 1\. 安装 Node.js

Claude Code 基于 Node.js 运行，所以第一步需要安装它。

​

### Windows 版

下载地址：https://nodejs.org/en/download/

**1.1 下载安装包**

进入官网，下载 `.msi` 安装包（Windows Installer）。

![[笔记同步助手/images/3eb47714067141589e4bc04c4c887e5c_MD5.png]]

**1.2 运行安装程序**

双击打开下载好的 `.msi` 文件，按如下步骤完成安装：

![[笔记同步助手/images/5b6038e86b83085a5e7811f850bf9a6e_MD5.png]]

勾选「I accept the terms in the License Agreement」接受协议：

![[笔记同步助手/images/271283231c82cc2f34eae47899d89acb_MD5.png]]

选择安装路径（默认是 C 盘，如果 C 盘空间紧张可以更换到其他盘）：

![[笔记同步助手/images/9f4d92d0391c3688fbac52f404fc8129_MD5.png]]

点击「Next」继续：

![[笔记同步助手/images/cfa6e9c1bc49eecee50985cf6c12633a_MD5.png]]

这个页面**不需要打勾**，直接下一步：

![[笔记同步助手/images/a2590040578b3f62045a14c867f56428_MD5.png]]

点击「Finish」，Node.js 安装完成。

**1.3 验证安装**

按 `Win + R`，输入 `cmd`，回车打开命令提示符窗口，依次输入以下命令：

-   • `node -v` — 显示 Node.js 版本号
    
-   • `npm -v` — 显示 npm 版本号
    

如果都能正常显示版本号，说明安装成功。

![[笔记同步助手/images/020baffdd7a61ecb9f98c1d85bb4779b_MD5.png]]

> **Tips:** 网上很多教程会教你用命令行、Docker 等方式安装 Node.js，但对新手来说，直接下载 `.msi` 安装包是最简单省心的方式。

---

### Mac 版

下载地址：https://nodejs.org/en/download/

**1.1 下载安装包**

进入官网，下载 `.pkg` 安装包（macOS Installer）。

![[笔记同步助手/images/f49f3de123b211be52a5347428fda737_MD5.png]]

**1.2 运行安装程序**

双击打开 `.pkg` 文件，一路点击「Continue / 继续」即可完成安装：

![[笔记同步助手/images/536e1c41ea724633f324e8a32e6db56a_MD5.png]]

![[笔记同步助手/images/27d79bc859815b9a2187442d1795605d_MD5.png]]

![[笔记同步助手/images/098d70230d4e55860a3ad2efcd7d336a_MD5.png]]

![[笔记同步助手/images/22fe574a180dec647e3f1ecbe90c43fd_MD5.png]]

**1.3 验证安装**

打开「终端」应用（在启动台搜索"终端"或"Terminal"），依次输入以下命令：

-   • `node -v` — 显示 Node.js 版本号
    
-   • `npm -v` — 显示 npm 版本号
    

如果都能正常显示版本号，说明安装成功。

![[笔记同步助手/images/3b23c13a045b4f26dea653195416ac48_MD5.png]]

---

## 2\. 安装 Claude Code

### Windows 版

> **前置要求：** Windows 用户还需要额外安装 **Git for Windows**，下载地址：https://git-scm.com/install/windows
> 
> 安装过程一路默认即可，装完后才能正常使用 Claude Code。

**2.1 安装 Claude Code**

打开 `cmd` 命令窗口，依次执行以下两条命令（先执行第一条等它跑完，再执行第二条验证）：

```
npm install -g @anthropic-ai/claude-code
claude --version
```

如果第二条命令能正常输出版本号，说明 Claude Code 安装成功。

**2.2 配置环境变量**

鼠标右键点击「此电脑」→「属性」→「高级系统设置」→「环境变量」（或者直接在开始菜单搜索"环境变量"）：

![[笔记同步助手/images/1f82195e8624077c61170bda3f800335_MD5.png]]

**2.3 新建系统变量**

在「系统变量」区域点击「新建」，分别添加以下两个变量：

![[笔记同步助手/images/99a5eec4ed72627c9b2a40769eb882f0_MD5.png]]

| 变量名 | 值 | 说明 |
| --- | --- | --- |
| `ANTHROPIC_AUTH_TOKEN` | 你在中转站获取的 API Key | 用于身份认证 |
| `ANTHROPIC_BASE_URL` | 中转站提供的 API 地址 | 用于指定请求转发地址 |

新建完成后，点击「确定」保存。

​

> **Tips:**
> 
> -   • 这里的核心就是配置 **URL + Token**。不管你用的是官方 API 还是中转站，原理都一样——填上对应的地址和密钥就行。
>     
> -   • 如果你的中转站只支持一个模型，想换模型就替换 URL；如果同一个 URL 支持多个模型，可以在 Claude Code 里用 `/model` 命令切换。
>     
> -   • **配置完环境变量后，如果没有生效，记得重启命令窗口或重启电脑。**
>     

---

### Mac 版

**2.1 安装 Claude Code**

打开终端，执行以下命令：

```
npm install -g @anthropic-ai/claude-code
claude --version
```

**2.2 配置环境变量**

在终端中执行以下命令，将 API 信息写入 Shell 配置文件：

```
# 如果你的终端是 bash（较老的 macOS 默认）
echo'export ANTHROPIC_BASE_URL="替换为中转站提供的URL"' >> ~/.bash_profile
echo'export ANTHROPIC_AUTH_TOKEN="替换为你的API Key"' >> ~/.bash_profile

# 如果你的终端是 zsh（macOS Catalina 及以后的默认终端）
echo'export ANTHROPIC_BASE_URL="替换为中转站提供的URL"' >> ~/.zshrc
echo'export ANTHROPIC_AUTH_TOKEN="替换为你的API Key"' >> ~/.zshrc
```

> **不确定自己是 bash 还是 zsh？** 在终端输入 `echo $SHELL` 看一下，显示 `/bin/zsh` 就是 zsh，显示 `/bin/bash` 就是 bash。新款 Mac 大概率是 zsh。

**2.3 使配置生效**

```
source ~/.bash_profile  # bash 用户执行这条
source ~/.zshrc         # zsh 用户执行这条
```

---

## 3\. 启动 Claude Code

一切配置完成后，来测试一下!

-   • **Windows：** `Win + R` 输入 `cmd` 打开命令窗口，输入 `claude`
    
-   • **Mac：** 打开终端，输入 `claude`
    

看到如下界面就说明一切就绪，可以开始使用了：

![[笔记同步助手/images/af70f38968337800bad1c39aae436a20_MD5.png]]

---

## 4\. Claude Code 使用小贴士

### 4.1 Claude Code 是什么？

Claude Code 是 Anthropic 官方出品的**命令行 AI 编程工具**。和 ChatGPT、Claude 网页版最大的区别在于：它能直接操作你电脑上的文件、执行命令、读写代码、管理项目。

简单理解：**网页版 Claude 是聊天助手，Claude Code 是能动手干活的 AI 员工。**

它能做的事：

-   • 读取和修改你电脑上的代码文件
    
-   • 执行终端命令（安装依赖、运行脚本、Git 操作等）
    
-   • 搜索代码库、理解项目结构
    
-   • 自动创建 Git commit 和 Pull Request
    
-   • 通过 MCP 协议连接外部工具（Notion、Figma、数据库、Jira 等）
    
-   • 在多个会话之间保持记忆
    

### 4.2 常用命令速查

**启动类：**

```
claude           # 直接启动交互模式
claude -c        # 继续上次的对话
claude --resume  # 恢复某个历史会话
```

**会话内指令：**

| 指令 | 作用 |
| --- | --- |
| `/help` | 显示所有可用命令 |
| `/clear` | 清空当前对话历史 |
| `/compact` | 压缩对话上下文（对话太长、响应变慢时使用） |
| `/plan` | 进入计划模式（让 Claude 先出方案再动手） |
| `/cost` | 查看当前会话的 Token 消耗 |
| `/context` | 可视化上下文使用情况，判断是否需要 `/clear` |
| `/memory` | 编辑 CLAUDE.md 记忆文件（存储项目规则和偏好） |
| `/init` | 初始化项目，自动生成 CLAUDE.md（新项目首次使用） |
| `/model` | 切换模型（在不同模型间按需切换） |
| `/resume` | 恢复历史会话，继续之前的工作 |
| `/copy` | 复制上一条回复到剪贴板 |
| `/export` | 导出整个对话（方便存档或分享） |

---

## 5\. 常见问题 FAQ

**Q: 安装 Claude Code 时报错 `npm ERR!` 怎么办？** A: 检查 Node.js 是否安装成功（`node -v` 能否输出版本号）。如果网络不好，可以尝试设置 npm 镜像源：`npm config set registry https://registry.npmmirror.com`

**Q: 输入 `claude` 后提示"不是内部或外部命令"？** A: 大概率是环境变量没有生效。重启命令窗口或重启电脑后再试。

**Q: 环境变量配置好了，但 Claude Code 连不上？** A: 检查 `ANTHROPIC_BASE_URL` 和 `ANTHROPIC_AUTH_TOKEN` 是否填写正确，URL 末尾不要多加 `/`。另外确认中转站账户有余额。

**Q: 中转站 API 和官方 API 有什么区别？** A: 底层调用的是同一个模型，中转站只是做了请求转发。功能上没有区别，区别在于价格和访问方式。中转站支持国内支付，价格通常更优惠。

​

---

> 教程到这里就结束了! 如果在安装过程中遇到任何问题，欢迎在群里随时提问。
> 
> — 来自团队的 00 后小伙伴，祝大家玩得开心 :)

---

![[笔记同步助手/images/85f45bd10d357dfd848bee58fc89827a_MD5.jpg|cover_image]]

01fish

继续滑动看下一个

![[笔记同步助手/images/54dbc993d7d59b450bfcba9d807e07fd_MD5.png]]

01fish

向上滑动看下一个