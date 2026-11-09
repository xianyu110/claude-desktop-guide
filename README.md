# Claude桌面端从0到1保姆级教程

> 基于 **2026-11-09** 在 **Mac mini** 上的真实安装与配置：升级 Claude Desktop、用 [momoAI](https://momoai.asia) + [CC Switch](https://ccswitch.asia/zh/) 接好国内中转，一步步手把手到能正常对话。完整 Key 只写本机配置，文中仅保留掩码。

**Keywords:** Claude Desktop · CC Switch · momoAI · ClaudeMax · 国内中转 · macOS

参考文档：[Claude Desktop 桌面端全流程指南](https://doc.momoai.asia/blog/claude-desktop-guide)

---

## 目录

- [你将得到什么](#你将得到什么)
- [开始前准备](#开始前准备)
- [整体怎么串起来](#整体怎么串起来)
- [第一步：下载安装 Claude Desktop](#第一步下载安装-claude-desktop)
- [第二步：安装 CC Switch](#第二步安装-cc-switch)
- [第三步：在 momoAI 创建 API Key](#第三步在-momoai-创建-api-key)
- [第四步：在 CC Switch 里填中转](#第四步在-cc-switch-里填中转)
- [第五步：彻底重启 Claude 并测试](#第五步彻底重启-claude-并测试)
- [第六步：选对模型再验证](#第六步选对模型再验证)
- [参数对照表](#参数对照表)
- [本机实测结果](#本机实测结果)
- [常见报错怎么排](#常见报错怎么排)
- [安全提醒](#安全提醒)
- [发问前自查清单](#发问前自查清单)
- [真实截图](#真实截图)

---

## 你将得到什么

装好官方 **Claude Desktop**，不走官方登录也能用 momoAI 中转正常聊天；界面里能看到 Gateway / 模型名，发一句短消息有回复。

Claude Desktop 是 Anthropic 官方桌面端：本地文件、Projects、Artifacts、Code 都比网页版更顺手。国内直连常被拦，所以用这三件套：

1. **Claude Desktop** —— 负责界面与交互  
2. **momoAI** —— 负责模型与计费  
3. **CC Switch** —— 负责中转网络配置与 Key 切换  

适合用来：

- 日常多轮对话、长文整理、会议纪要  
- 上传 PDF / 图片 / Excel / 代码一起分析  
- Projects 挂本地目录，带着项目上下文干活  
- Artifacts 预览代码与可视化组件  

---

## 开始前准备

| 准备项 | 说明 |
| --- | --- |
| 电脑 | 本教程以 **Mac mini · macOS** 为例；Windows 差异会标出 |
| 代理 | 能翻墙更好（本机 Clash `127.0.0.1:7897`；官方包下载经常需要） |
| 账号 | [momoai.asia](https://momoai.asia) 已登录控制台 |
| 时间 | 约 10–20 分钟；准备好「完全退出再打开 Claude」的耐心 |

> **先别登录官方账号。** Claude 装好后先不要急着登 Anthropic 官方账号。先把中转配通，再开新对话测试，避免旧会话缓存干扰。

---

## 整体怎么串起来

```text
Claude Desktop ──读当前供应商──▶ CC Switch ──Base URL + Bearer──▶ momoai.asia ──转发──▶ Anthropic
       │                                                              ▲
       └─────────── 3P Gateway（可选，同一把 Key）─────────────────────┘
```

记住一句话：Claude 负责界面，momoAI 负责模型与计费，CC Switch 负责把 Base URL 和 Key 写进桌面端能读到的配置。两边都指向同一个 `https://momoai.asia` 和同一把 Key。

---

## 第一步：下载安装 Claude Desktop

### 下载渠道怎么选

| 渠道 | 适用 | 说明 |
| --- | --- | --- |
| 官方 Download for macOS / Windows | 首选 | 能打开就用官方，保证最新完整包 |
| 夸克等客户端网盘 | 官方页打不开 | 文档站备链：Claude / CC Switch 安装包 |
| Homebrew CDN zip | 本机实测备用 | 官方被地区 / Cloudflare 拦时，经 Clash 下 zip 装到 `/Applications` |

### macOS 手把手

1. **拿安装包**  
   打开官方下载页点 Download for macOS；打不开就换网盘或 CDN zip，下载走 Clash。
2. **打开安装器**  
   双击 `.dmg`（或解压 zip），把 `Claude.app` 拖进「应用程序」。
3. **首次启动**  
   从 Launchpad / Applications 打开。若弹权限、网络提示，本地点掉允许即可。
4. **先别登录**  
   看到欢迎页就停一下，先去装 CC Switch 和建 Key。

### Windows 简述

1. 下载 Download for Windows 安装包（官方打不开用网盘）  
2. 双击安装向导，装完从开始菜单打开 Claude  
3. 同样先不要登录官方账号，先配中转  

本机结果：Claude Desktop 版本 **`4.23118.0`**，界面已切中文。

![Claude 欢迎页](https://upload.maynor1024.live/file/1794212683836_08-claude-welcome.png)

![Claude 启动](https://upload.maynor1024.live/file/1794212688886_02-claude-launched.png)

---

## 第二步：安装 CC Switch

CC Switch 用来管理 Claude Desktop 的中转供应商（**应用类型必须选 Claude Desktop**）。未装可从[文档站网盘](https://doc.momoai.asia/blog/claude-desktop-guide)或 GitHub Release 下 dmg。

1. **下载并安装** —— 打开 CC Switch 的 `.dmg`，拖到应用程序。本机版本 **4.0.28**。  
2. **打开应用** —— 确认能看到供应商列表 / 路由相关界面，暂时先别点启动。  
3. **记下应用类型** —— 后面新建配置时，应用类型务必选 **Claude Desktop**，不要选错成 CLI 或其他。

![CC Switch 窗口](https://upload.maynor1024.live/file/1794212691318_06-cc-switch-window.png)

![CC Switch 供应商](https://upload.maynor1024.live/file/1794212689285_11-cc-switch.png)

---

## 第三步：在 momoAI 创建 API Key

打开 [https://momoai.asia](https://momoai.asia) 登录控制台。

1. **进入 API Keys** —— 控制台左侧或首页进入 API Keys 列表页。  
2. **点创建 / Create** —— 打开新建 Key 对话框。  
3. **填名称与协议** —— 名称建议好认，例如 `ClaudeDesktop-CCSwitch`；协议选 **Anthropic**。  
4. **选对分组** —— 分组选 **ClaudeMax**（或控制台里的 ClaudeMax余额分组）。分组决定能用哪些模型 ID。  
5. **创建并立刻复制** —— 点 Create 后完整 Key 只出现一次。复制保存到本机密码管理器；聊天和截图里只留掩码。

> **Key 安全：** 完整 Key 不要贴进聊天、公开仓库或群截图。泄露立刻在控制台作废并重建。本文只写掩码 `sk-3f4…55b4`。

| 字段 | 本机值 |
| --- | --- |
| Key 名称 | ClaudeDesktop-CCSwitch |
| 掩码 | `sk-3f4…55b4` |
| 分组 | ClaudeMax余额分组 · 1.4x |
| 状态 | Active · 2026-11-09 创建 |

![API Keys 控制台](https://upload.maynor1024.live/file/1794212669537_momoai-01-console.png)

![创建 Key 对话框](https://upload.maynor1024.live/file/1794212667158_momoai-02-create-key.png)

![Use Key 成功](https://upload.maynor1024.live/file/1794212670122_momoai-04-use-key.png)

![Keys 列表最终页](https://upload.maynor1024.live/file/1794212679726_momoai-05-keys-final.png)

---

## 第四步：在 CC Switch 里填中转

打开 CC Switch，新建或编辑一条 Claude Desktop 供应商，按下表逐项核对：

| 配置项 | 填什么 | 注意 |
| --- | --- | --- |
| 应用类型 | `Claude Desktop` | 必须精确选这一项 |
| 中转地址 Base URL | `https://momoai.asia` | 不要加多余路径 |
| API Key | 刚复制的 `sk-…` | 用 raw Bearer，不要多包一层 |
| 模型 | 分组真实支持的 ID | 如 `claude-sonnet-5` / `claude-opus-5`（以控制台为准） |

接着：

1. **保存供应商** —— 确认卡片名称好认（例如 momoAI Claude Desktop）。  
2. **设为当前并启动路由** —— 勾选该卡片，点「启动路由」或等价切换成功提示。  
3. **确认写进本机** —— 成功后 CC Switch 会把 `ANTHROPIC_BASE_URL` / `AUTH_TOKEN` 等写给 Claude Desktop 可读的配置；本机当前供应商 id 为 `momoai-claude-desktop`。

若你同时维护 Claude-3p Gateway：`deploymentMode` 为 `3p`，`inferenceGatewayBaseUrl` 填 `https://momoai.asia`，模型列表含 sonnet-5 / opus-5 / fable-5，与上面是同一把 Key。

---

## 第五步：彻底重启 Claude 并测试

> **切换成功 ≠ 已生效。** CC Switch 显示成功，只说明中转就绪。正在跑的 Claude 进程不会自动刷新，必须完全退出再开。

1. **完全退出 Claude**  
   macOS：菜单栏 Claude → 退出，并确认 Dock / 活动监视器里没有残留进程。Windows：注意托盘图标也要退出。  
2. **重新打开 Claude Desktop**  
   从应用程序重新启动，等到主界面出来。  
3. **新建空白对话**  
   不要在旧的报错对话里重试；点 New 开全新会话。  
4. **发短消息测试**  
   例如：`请回复：连接测试成功。` 能马上收到回复就说明通了。

本机界面已显示 **maynorai · Gateway**，模型侧为 Sonnet 5 High。

![Claude Gateway](https://upload.maynor1024.live/file/1794212685056_10-Claude-2191.png)

---

## 第六步：选对模型再验证

点输入框右下角模型名展开列表。界面显示名可能是别名，真正决定成败的是中转分组支持的模型 ID。

1. 先选通用基础模型（如 `claude-sonnet-5`）  
2. 只发一两句短文本，确认连通  
3. 再试长文、文件上传或 Code 任务  
4. 换 opus 等更贵模型前，先确认 Key 分组已开放  

### 连通后的小用法

**会议纪要**

```text
请把下面内容整理成中文会议纪要，输出结论、待办、负责人、截止时间四列表格；没有的信息标待确认，不要编造。
```

**文件分析**

点 `+` 上传 PDF/表格后：

```text
先概括核心结论，再列 5 条可执行建议；引用原文请标页码。
```

**本地项目**

用 Project or folder 挂目录：先让它扫结构，再限定可改路径，小步改、小步测。

---

## 参数对照表

| 项 | 值 |
| --- | --- |
| 机器 | Mac mini · Asia/Shanghai |
| Claude Desktop | 4.23118.0 · 中文界面 |
| CC Switch | 4.0.28 |
| 端点 | `https://momoai.asia` |
| 代理 | Clash `127.0.0.1:7897` |
| Key 名 | ClaudeDesktop-CCSwitch |
| Key 掩码 | `sk-3f4…55b4` |
| 分组 | ClaudeMax余额分组 · 1.4x |
| 推荐模型 ID | `claude-sonnet-5` / `claude-opus-5` / `claude-fable-5` |
| API 路径 | `POST /v1/messages`（不要用 `/anthropic` 当消息接口） |

---

## 本机实测结果

| 检查项 | 结果 | 备注 |
| --- | --- | --- |
| momo 登录 | 成功 | 已有会话 |
| 创建 Key | 成功 | 2026-11-09 16:16:18 |
| CC Switch 当前供应商 | momoAI Claude Desktop | `is_current=1` |
| Claude UI | maynorai · Gateway | Sonnet 5 High |
| API `claude-sonnet-5` | HTTP 200 · OK | 经 Clash，建议 curl 加 `--http1.1` |

---

## 常见报错怎么排

### 报错：`requested model is not supported by this group`

原因：Key 所属分组没有开放你选的模型。

1. 登录 [momoai.asia](https://momoai.asia) 查该 Key 分组真实支持的模型 ID  
2. 回 CC Switch 改成列表内的模型并保存  
3. 重新启动路由  
4. 彻底退出并重启 Claude，新建对话再测  

### 一直转圈或发送失败

- 先发短文本，排除超大附件  
- 检查代理与 momo 节点是否正常  
- 看额度是否用尽、Key 是否过期  
- 确认 CC Switch 激活的是 Claude Desktop 那张卡  
- 先后重启 CC Switch 与 Claude Desktop  

### 本机踩过的坑

| 现象 | 处理 |
| --- | --- |
| 官方安装包被地区 / Cloudflare 拦 | 改 CDN zip，下载走 Clash |
| `INVALID_API_KEY` | 确认是 raw Key；模型 ID 用 `claude-sonnet-5` / `claude-opus-5` |
| 打到 `/anthropic` 返回 HTML | 消息接口用 `/v1/messages` |
| curl HTTP2 framing 偶发失败 | 加 `--http1.1` 后稳定 200 |
| 辅助功能 / 截屏权限导致自动点弹窗不稳 | 本机手动点掉弹窗；配置可直接写 SQLite + 3P 文件 |

---

## 安全提醒

1. Key 只放本机密码库与 CC Switch，不进公开仓库、群聊、截图  
2. 商业机密、私钥、证件号上传前先脱敏；中转会看到请求上下文  
3. 怀疑泄露：控制台立刻注销旧 Key，生成新 Key 并更新 CC Switch  

---

## 发问前自查清单

- [x] 已安装 Claude Desktop（官方或可靠渠道）  
- [x] 已安装 CC Switch  
- [x] momoai.asia 已建 Key，分组选对（如 ClaudeMax）  
- [x] CC Switch：应用类型 Claude Desktop，Base URL 与 Key 正确  
- [x] 已启动路由 / 供应商为当前  
- [x] 已完全退出并重新打开 Claude  
- [x] 新建对话，模型 ID 在分组支持列表内  
- [x] 短消息测试已收到回复  

以上状态截至 **2026-11-09** 本机配置完成时；你改配置后以实际界面为准。

---

## 真实截图

同一次安装过程抓取（已传到自定义图床）：

| 说明 | 链接 |
| --- | --- |
| API Keys 控制台 | https://upload.maynor1024.live/file/1794212669537_momoai-01-console.png |
| 创建 Key 对话框 | https://upload.maynor1024.live/file/1794212667158_momoai-02-create-key.png |
| Use Key 成功 | https://upload.maynor1024.live/file/1794212670122_momoai-04-use-key.png |
| Keys 列表最终页 | https://upload.maynor1024.live/file/1794212679726_momoai-05-keys-final.png |
| Claude 启动 | https://upload.maynor1024.live/file/1794212688886_02-claude-launched.png |
| Claude 欢迎页 | https://upload.maynor1024.live/file/1794212683836_08-claude-welcome.png |
| Claude Gateway | https://upload.maynor1024.live/file/1794212685056_10-Claude-2191.png |
| CC Switch 窗口 | https://upload.maynor1024.live/file/1794212691318_06-cc-switch-window.png |
| CC Switch 供应商 | https://upload.maynor1024.live/file/1794212689285_11-cc-switch.png |

完整文字指南也可对照：[https://doc.momoai.asia/blog/claude-desktop-guide](https://doc.momoai.asia/blog/claude-desktop-guide)

---

## License

本文以个人安装复盘 / 教程形式发布，示例可自由复用。截图与界面文案归原产品方；中转价格与模型列表以控制台实时为准。momoAI / CC Switch 为第三方工具，非 Anthropic 官方产品。
