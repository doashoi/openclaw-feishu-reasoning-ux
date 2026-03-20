<div align="right">
简体中文 | <a href="./README_EN.md">English</a>
</div>

# OpenClaw Feishu Reasoning UX

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
![Status](https://img.shields.io/badge/status-beta-orange)
![OpenClaw](https://img.shields.io/badge/OpenClaw-Feishu%20UX-0ea5e9)
![Skill](https://img.shields.io/badge/type-skill-8b5cf6)

让 OpenClaw 在飞书里像 Telegram 一样实时流式显示思考内容，避免长时间黑盒等待，并配上更好看的彩色 2.0 卡片，支持多组件容器和富文本。

这是一个给 Openclaw 使用的 skill，用来改造或排查 OpenClaw 的 Feishu 回复体验，重点包括：

- 可见的 reasoning / reply 双 lane
- 折叠式 reasoning 面板
- 卡片 2.0 布局
- 标题、颜色、模板统一
- 模型能力检测与问题排查

## 前置条件与风险点

在让 Agent 开始改造前，先明确这几件事：

- 先确认当前 **OpenClaw 版本 / 实际运行 build**
- 先确认你用的是 **OpenClaw 内置的 Feishu 通道**
- 不要把它和 **飞书官方插件**、别的 Feishu 集成、浏览器插件方案混为一谈
- 再确认当前模型/provider 路线，尤其是：
  - `minimax-cn`
  - 不是 `minimax-portal`
- 再确认实际加载的是哪套代码路径
  - `src/`
  - 还是 `dist/`
- 如果要动 raw reasoning，不要一上来就改 runtime
  - 先做低风险的标题、颜色、卡片 2.0 容器
  - 再确认普通 answer streaming 稳定
  - 最后才进入 raw reasoning 这条高风险路径

这个 skill 的经验主线，验证条件更重要的是：

- 实际 OpenClaw 版本 / build
- OpenClaw 内置 Feishu 通道
- `minimax-cn/MiniMax-M2.7`

WSL 是我们验证案例的一部分，但不是唯一硬前置条件。真正更容易导致失败的是：

- 版本不同
- 通道实现不同
- provider 路线不同
- 实际加载代码路径不同
- service/session 状态不同

如果你的环境不是这条线，Agent 应该先把差异说清楚，再决定怎么改。

## 效果展示

### 刚进入思考阶段

![刚进入思考阶段](./assets/01-thinking-start.jpg)

### 展开后的 reasoning 内容

![展开后的 reasoning 内容](./assets/02-reasoning-expanded.jpg)

### 折叠 reasoning 后的最终回复

![折叠 reasoning 后的最终回复](./assets/03-reasoning-collapsed-reply.jpg)

## 这是什么

改善 Openclaw 在飞书通道中的回复体验的 skill。

它的目标是：

- 先确认是不是 OpenClaw 内置 Feishu 通道，而不是别的 Feishu 插件/接法
- 先判断当前模型路径是否真的支持可见 raw reasoning
- 如果支持，就把 reasoning 真实接进 Feishu 卡片
- 如果不支持，就明确说明限制，并继续优化回复卡片体验

## 和普通 Feishu 流式的区别

Openclaw 自带的飞书通道以及飞书官方插件仅能实现正式回复内容的流式显示，无法实时流式更新模型的思考内容。

这个 skill 想做的是：

- reasoning 和 answer 分开显示
- reasoning 放进折叠面板
- answer 在单独区域继续流

也就是更接近 Telegram reasoning stream 的体验，而不是只有一段正文在实时更新。

## 默认预设流程

默认预设是这样跑的：

1. 先发一张卡片
2. reasoning 面板先出现占位
3. raw reasoning 实时流进折叠面板
4. 正式回答开始流时，reasoning 面板自动收起
5. 正式回答在 answer 区继续流

## 这不是伪造的“思考过程”

这个 skill 不会默认拿模板文案去假装 reasoning。

它会先检查：

- 当前模型/provider/runtime 是否真的暴露了 live reasoning

然后再决定：

- 能不能显示 raw reasoning
- 还是只能做普通流式回复和卡片优化

## 适合谁

如果你：

- 在用 OpenClaw + Feishu
- 想看到更可见的回复过程
- 想优化卡片 2.0、折叠面板、标题和颜色
- 遇到过 `Thinking...`、raw reasoning 消失、标题样式回退之类问题

那这个 skill 就是给你的。

## 安装方式

推荐方式：

- 直接把这个 GitHub 仓库地址交给 OpenClaw
- 让它从仓库读取 skill 并执行

仓库地址：

- `https://github.com/doashoi/openclaw-feishu-reasoning-ux`

## 最小触发示例

- `帮我把 OpenClaw 在飞书里的回复改得更有层次一点`
- `为什么飞书里只剩 Thinking... 了`
- `我想要可见的推理流和更好看的 Feishu 卡片`
- `帮我统一飞书回复卡片的标题、颜色和折叠面板`

## 常见 FAQ

### 为什么 raw reasoning 有时候是中文，有时候是英文？

这是常见现象，不一定是卡片实现坏了。

原因通常在模型/provider 本身：

- 可能中英混合组织 reasoning
- 可能把上游 reasoning snapshot 原样交给前台
- 不同问题上 reasoning 语言也可能变化

如果显示的是 raw reasoning，就不保证语言绝对统一。

### 为什么有的模型能流 raw reasoning，有的只能看到 Thinking...？

因为不同模型/provider 暴露 reasoning 的方式不同。

常见情况有：

- 有 readable live reasoning event
- 只有最终 transcript 里有 thinking
- reasoning 是加密/opaque payload
- 根本不暴露 reasoning

所以这不只是 Feishu 卡片层的问题，也取决于当前模型路径本身。

补充一点：

- 有些模型路径不是“完全不支持 reasoning”
- 而是只支持 snapshot / transcript-only thinking
- 这种情况下，它可能能在最终记录里留下 thinking，但不能提供真正的 live raw reasoning

所以更准确的说法通常是：
- `不支持 true live raw reasoning`
而不是简单一句：
- `模型不支持 reasoning`

### 能不能强制全都显示成中文？

可以做展示层改写，但那就不再是严格 raw reasoning。

这个 skill 默认优先保留 reasoning 的真实性，不默认替用户翻译或改写。

## 参考资料

### 飞书官方文档

- 飞书卡片 JSON 2.0 总览  
  https://open.feishu.cn/document/feishu-cards/card-json-v2-structure
- 折叠面板 `collapsible_panel`  
  https://open.feishu.cn/document/feishu-cards/card-json-v2-components/containers/collapsible-panel
- 卡片更新说明  
  https://open.feishu.cn/document/feishu-cards/update-feishu-card
- 已发送消息卡片 PATCH 更新  
  https://open.feishu.cn/document/server-docs/im-v1/message-card/patch

### OpenClaw / Telegram 参考

- OpenClaw Telegram 通道文档  
  https://openclawlab.com/en/docs/channels/telegram/
- OpenClaw 上游仓库  
  https://github.com/openclaw/openclaw

### 仓库内参考

- [SKILL.md](./SKILL.md)
- [references/implementation-guide.md](./references/implementation-guide.md)
