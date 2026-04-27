---
title: vibe coding工具:claude/geminic li+cursor+copilot
date: 2026-04-18 17:11:31
tags: [programming, tool, claude, cursor]
category:
  - 专业笔记
---

### claude（fee）

官方中文教程参考https://code.claude.com/docs/zh-CN/best-practices

#### 1.安装（linux/macOS）

总的通过npm安装比较方便。

```
//https://zhuanlan.zhihu.com/p/1930212605588412104 linux安装
// 安装最新node --version 24
// 终端claude验证安装成功


//在macos安装多次各种各样的失败 要么没安装二进制文件要么npm没有权限，后来vpn切换到日本一次安装成功
//https://zhuanlan.zhihu.com/p/1937543043256386586
// claude --version
```



#### 2. 无法访问Claude:原因网络限制

- 代理global
- 可能是浏览器能上网，但是终端没有设置代理。export https_proxy和export http_proxy->一般是这个原因，验证成功。



#### 3.（for China）购买获取：个人用手机号注册

https://juejin.cn/post/7613209888123027510

Hero-SMS实卡接码，买的英国区

https://hero-sms.com/purchases/numbers

没有尝试上面的方法。目前手里有一个api key.通过编辑环境里的api key解决了这个问题。

Api key访问：

https://zhuanlan.zhihu.com/p/1937543043256386586

```json
## vim ~/.claude/settings.json

{
  "env": {
    "ANTHROPIC_AUTH_TOKEN": "",
    "ANTHROPIC_BASE_URL": "https://open.bigmodel.cn/api/anthropic",
    "API_TIMEOUT_MS": "3000000",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": 1,
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "GLM-4.5-air",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "GLM-5.1",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "GLM-5.1"
  },
  "permissions": {
    "allow": [],
    "deny": []
  }

```



### cursor(fee)

和claude构建cot的思路可能不一样，作为第二种选择。准备用claude不适应的时候来



### copilot

代码补全的时候切换用



### gemini cli（free）

跟claude 在终端的控制方式有点像，关键是免费！claude用不了的时候果断切换。



### 阅读github repo:deepwiki

阅读github repo



### 
