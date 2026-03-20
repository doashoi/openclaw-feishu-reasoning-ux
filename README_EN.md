<div align="right">
<a href="./README.md">简体中文</a> | English
</div>

# OpenClaw Feishu Reasoning UX

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
![Status](https://img.shields.io/badge/status-beta-orange)
![OpenClaw](https://img.shields.io/badge/OpenClaw-Feishu%20UX-0ea5e9)
![Skill](https://img.shields.io/badge/type-skill-8b5cf6)

Make OpenClaw replies in Feishu more observable, streamable, and customizable.

This is a skill for agents that need to improve or debug OpenClaw's Feishu reply UX, especially around:

- visible reasoning / answer dual-lane replies
- collapsible reasoning panels
- card 2.0 layouts
- unified titles, colors, and templates
- model capability detection and troubleshooting

## Screenshots

### Initial thinking state

![Initial thinking state](./assets/01-thinking-start.jpg)

### Expanded reasoning content

![Expanded reasoning content](./assets/02-reasoning-expanded.jpg)

### Collapsed reasoning with final reply

![Collapsed reasoning with final reply](./assets/03-reasoning-collapsed-reply.jpg)

## What this is

This is not a generic "send Feishu cards" skill.

Its goal is to:

- detect whether the current model path truly supports visible raw reasoning
- if yes, wire real reasoning into Feishu cards
- if not, explain the limitation clearly and still improve the reply-card experience

## How this differs from ordinary Feishu streaming

Ordinary Feishu streaming usually only streams the final answer text.

This skill aims for:

- separate reasoning and answer lanes
- reasoning inside a collapsible panel
- answer continuing in a separate area

That is much closer to a Telegram-style reasoning-stream experience than a single updating text block.

## Default preset flow

The default preset works like this:

1. send a card
2. show a placeholder inside the reasoning panel
3. stream raw reasoning into the collapsible panel
4. collapse the reasoning panel when the answer starts streaming
5. continue streaming the final answer in the answer lane

## This does not fake reasoning

This skill does not default to template text pretending to be reasoning.

It first checks:

- whether the current model/provider/runtime truly exposes live reasoning

Then it decides:

- whether raw reasoning can be shown
- or whether only ordinary streaming + card UX improvements are possible

## Who this is for

Use this if you:

- use OpenClaw with Feishu
- want a more visible reply experience
- want better card 2.0 layouts, collapsible panels, titles, and colors
- have seen issues like `Thinking...`, missing raw reasoning, or title/style regressions

## Installation

Recommended path:

- give this GitHub repository URL directly to OpenClaw / your agent
- let it read the repository as a skill source

Repository URL:

- `https://github.com/doashoi/openclaw-feishu-reasoning-ux`

## Minimal trigger examples

- `Help me make OpenClaw replies in Feishu feel more layered`
- `Why does Feishu only show Thinking... now?`
- `I want visible reasoning and better-looking Feishu cards`
- `Help me unify the title, colors, and collapsible reasoning panel`

## FAQ

### Why is raw reasoning sometimes Chinese and sometimes English?

That is common and does not automatically mean the card implementation is broken.

Typical causes are model/provider-side:

- mixed-language internal reasoning
- upstream reasoning snapshots passed through with minimal transformation
- different prompts causing different reasoning language

If what is shown is truly raw reasoning, language consistency is not guaranteed.

### Why do some models stream raw reasoning while others only show Thinking...?

Because different models/providers expose reasoning in different forms.

Typical cases:

- readable live reasoning events
- thinking only in the final transcript
- encrypted/opaque reasoning payloads
- no exposed reasoning at all

So this is not only a Feishu card problem. It also depends on the active model path.

### Can we force everything into Chinese?

You can add a presentation-layer rewrite, but then it stops being strictly raw reasoning.

By default, this skill prioritizes preserving the authenticity of visible reasoning rather than silently translating or rewriting it.

## References

### Official Feishu docs

- Feishu Card JSON 2.0 structure  
  https://open.feishu.cn/document/feishu-cards/card-json-v2-structure
- `collapsible_panel`  
  https://open.feishu.cn/document/feishu-cards/card-json-v2-components/containers/collapsible-panel
- Feishu card update guide  
  https://open.feishu.cn/document/feishu-cards/update-feishu-card
- PATCH updates for sent message cards  
  https://open.feishu.cn/document/server-docs/im-v1/message-card/patch

### OpenClaw / Telegram references

- OpenClaw Telegram channel docs  
  https://openclawlab.com/en/docs/channels/telegram/
- OpenClaw upstream repository  
  https://github.com/openclaw/openclaw

### Repository-local references

- [SKILL.md](./SKILL.md)
- [references/implementation-guide.md](./references/implementation-guide.md)

## Restart policy

If a change requires restarting `openclaw-gateway`:

- the agent must explain why
- either ask the user to run it
- or ask for confirmation first

It should never restart the gateway silently.
