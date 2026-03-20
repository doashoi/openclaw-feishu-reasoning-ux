<div align="right">
简体中文 | <a href="./README_EN.md">English</a>
</div>

# OpenClaw Feishu Reasoning UX

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
![Status](https://img.shields.io/badge/status-beta-orange)
![OpenClaw](https://img.shields.io/badge/OpenClaw-Feishu%20UX-0ea5e9)
![Skill](https://img.shields.io/badge/type-skill-8b5cf6)

让 OpenClaw 在飞书里的回复，从黑盒，变成更可见、可流、可定制的体验。

这是一个给 Agent 使用的 skill，用来定制或修复 OpenClaw 在 Feishu 中的回复体验，重点包括：

- 可见的流式回复体验
- raw reasoning 诊断与展示路径
- 卡片 2.0 布局
- 折叠式 reasoning 面板
- 标题、颜色、模板统一
- Feishu 回复回归时的系统级排障

## 这是什么

这不是一个普通的“发飞书卡片” skill。

它更像是一套方法论 + 实战经验，用来把 OpenClaw 在 Feishu 里的回复体验从黑盒改造成：

- 可观察
- 有层次
- 可排障
- 可定制

它帮助 Agent：
- 判断当前模型/provider 是否真的支持可见 raw reasoning
- 区分是 runtime/session/provider 问题，还是卡片层问题
- 根据用户偏好定制 Feishu 回复卡片
- 尽量让行为在重启和新会话后也保持稳定

## 这不是什么

这个 skill **不保证** 在所有模型上都能显示 raw reasoning。

有些模型/provider：
- 根本不发 live reasoning event
- 只在结束后保留 transcript thinking
- 返回的是加密或不可直接展示的 reasoning payload

在这些情况下，这个 skill 应该指导 Agent：
- 明确解释限制来自哪里
- 给出下一步可选路线
- 在 raw reasoning 不可得时，仍然优化 Feishu 卡片 2.0 体验

## 适合谁

如果你：
- 在用 OpenClaw + Feishu
- 觉得官方 Feishu 回复体验太黑盒
- 想在支持的模型上看到 reasoning
- 想定制卡片 2.0、折叠面板、标题和颜色
- 遇到过 `Thinking...`、raw reasoning 消失、标题样式回退之类问题

那这个 skill 就是给你的。

## Agent 读了这个 skill 之后应该做什么

Agent 应该按这个顺序做事：

1. 先确认当前实际模型/provider/session 路径
2. 检测当前路线是否真的支持 raw reasoning
3. 用用户能懂的话解释现状
4. 给出实际可选路线
5. 再去做 Feishu 卡片和回复体验的定制

这个 skill 刻意写成了更用户友好的风格：
- 会主动问标题文案、面板文案、颜色偏好
- 不默认用户懂 OpenClaw 内部结构
- 不允许静默重启 `openclaw-gateway`

## 仓库内容

- [SKILL.md](./SKILL.md)：主 skill
- [references/implementation-guide.md](./references/implementation-guide.md)：更偏实现和排障的参考文档

## 安装方式

把这个目录复制到你的 skill 目录，例如：

- `~/.codex/skills/`
- `~/.agents/skills/`

具体以你的 Agent 运行环境为准。

## 推荐触发方式

例如：

- `帮我把 OpenClaw 在飞书里的回复改得更有层次一点`
- `为什么飞书里只剩 Thinking... 了`
- `我想要可见的推理流和更好看的 Feishu 卡片`
- `帮我统一飞书回复卡片的标题、颜色和折叠面板`

## 当前发布状态

建议把这个项目理解成：

- **实用型公开 beta / preview skill**

而不是：

- 对所有环境都 100% 稳定的通用成品

原因很简单：
- OpenClaw 版本差异会影响行为
- 不同 provider/model 的 reasoning 形态不同
- 有些环境实际跑的是编译产物，不是你眼前改的源码

这个 skill 最强的地方不是“任何地方都一样跑通”，而是：

- 排障顺序对
- 模型能力判断准
- Feishu UX 定制方法成熟
- 有真实踩坑后的经验沉淀

## 重启策略

如果某个改动需要重启 `openclaw-gateway`：

- Agent 必须先说明原因
- 要么让用户自己执行
- 要么先征求用户确认

不能静默重启。

因为意外重启会影响：
- 正在进行的 Feishu 对话
- 当前会话的 reasoning 状态
- 正在运行的任务

---

## English Summary

This repository provides a skill for customizing and debugging OpenClaw's Feishu reply UX, especially around:

- streaming replies
- raw reasoning visibility
- card 2.0 layouts
- collapsible reasoning panels
- title/color consistency
- system-level diagnosis when Feishu reply behavior regresses

It is best treated as a practical public beta / methodology skill rather than a universal guarantee across all models and providers.

## 下一步建议

如果你准备把它发布到更多 skill 库，推荐再补：

- 仓库 topics
- 一两张实际效果截图
- 一个最小安装示例
- 一个最小触发示例
