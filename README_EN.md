<div align="right">
<a href="./README.md">简体中文</a> | English
</div>

# OpenClaw Feishu Reasoning UX

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
![Status](https://img.shields.io/badge/status-beta-orange)
![OpenClaw](https://img.shields.io/badge/OpenClaw-Feishu%20UX-0ea5e9)
![Skill](https://img.shields.io/badge/type-skill-8b5cf6)

Make OpenClaw in Feishu more observable, streamable, and customizable.

This repository contains a skill for agents that need to customize or repair OpenClaw's Feishu reply UX, especially around:

- visible streaming replies
- raw reasoning diagnosis and display paths
- card 2.0 layouts
- collapsible reasoning panels
- title/color/template consistency
- system-level debugging of Feishu reply regressions

## What this is

This is not just a generic "send Feishu cards" skill.

It is a practical methodology and implementation skill for turning OpenClaw's Feishu reply experience from a black box into something more:

- observable
- layered
- debuggable
- customizable

It helps an agent:
- detect whether the current model/provider path can expose readable raw reasoning
- distinguish runtime/session/provider issues from card-layer issues
- customize Feishu reply cards based on user preference
- preserve behavior across restarts and new sessions as much as possible

This skill is not meant to impose only one fixed visual preset.

It is designed so that:

- container structure can be highly customized
- title wording can be highly customized
- reasoning panel wording can be highly customized
- color strategy can be highly customized

So after loading the skill, the ideal agent behavior is not just "apply one template", but to continue asking the user things like:

- what should the main title say?
- what should the reasoning panel say?
- should colors be random or fixed?
- should the reasoning panel auto-collapse?
- should the final reply card feel minimal or layered?

## How the default preset works

The default preset roughly works like this:

1. send a Feishu card first
2. show a placeholder inside the reasoning panel
3. stream raw reasoning into the collapsible panel in real time
4. when the final answer starts streaming, collapse the reasoning panel
5. continue streaming the final answer in a separate answer area
6. close on a single structured final card

So this is not just a single-lane "Thinking... then answer" experience.

It is a two-lane experience:

- reasoning lane
- answer lane

## What this is not

This skill does **not** guarantee raw reasoning on every model.

Some models/providers:
- do not emit live reasoning events
- only expose post-hoc transcript thinking
- expose encrypted or opaque reasoning payloads

In those cases, the skill should guide the agent to:
- explain the limitation clearly
- offer practical alternatives
- still improve Feishu card 2.0 UX where possible

## How this differs from ordinary Feishu streaming

This skill is not just about turning on OpenClaw's Feishu `streaming=true`.

Ordinary Feishu streaming usually only provides:

- streaming final answer text

What it usually does **not** provide:

- visible raw reasoning
- separate reasoning and answer lanes
- a collapsible reasoning panel
- auto-collapse of reasoning followed by continued answer streaming

So this experience is fundamentally different from merely enabling normal Feishu streaming.

More precisely:

- **ordinary Feishu streaming**: only streams the final answer text
- **this skill's target UX**: like Telegram reasoning stream, it makes the thinking process itself visible in real time, then closes into the final answer

## This is not fake simulated thinking

An important distinction:

this skill is not trying to fake reasoning by printing a handcrafted "thinking..." block.

The real flow is:

1. detect whether the current model/provider/runtime truly exposes live reasoning
2. if yes, wire the real reasoning stream into the Feishu card
3. if not, tell the user clearly that the current model path does not support it instead of faking a reasoning section

That is the main difference from many UIs that merely look like they are showing thought:

- capability detection first
- only real reasoning is shown
- template text is not used to impersonate model reasoning

## Who this is for

Use this if you:
- use OpenClaw with Feishu
- want a better reply UX than the default black-box behavior
- want visible reasoning where the model/runtime supports it
- want card 2.0 styling, collapsible panels, better titles, and better colors
- need a structured debugging workflow when Feishu reply behavior regresses

## What an agent should do with this skill

After loading this skill, the agent should:

1. verify the actual model/provider/session path
2. detect whether raw reasoning is truly supported in the current route
3. explain findings in plain language
4. offer practical next-step options
5. then customize Feishu card behavior and styling

This skill is intentionally user-friendly:
- it should ask for title/panel/color preferences
- it should not assume the user knows OpenClaw internals
- it should not silently restart `openclaw-gateway`

## Repository contents

- [SKILL.md](./SKILL.md): main skill
- [references/implementation-guide.md](./references/implementation-guide.md): deeper implementation and debugging reference

## References

These are the main references used to shape this skill and its methodology.

### Official Feishu docs

- Feishu Card JSON 2.0 structure  
  https://open.feishu.cn/document/feishu-cards/card-json-v2-structure

- `collapsible_panel` in Feishu Card 2.0  
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

Note:
- We referenced Telegram’s reasoning-stream direction as a product and implementation reference
- But upstream source paths may change across versions
- So this repository keeps stable upstream entry links here instead of hardcoding fragile deep source links

### Repository-local references

- Main skill: [`SKILL.md`](./SKILL.md)
- Implementation reference: [`references/implementation-guide.md`](./references/implementation-guide.md)

## Installation

This repository is meant for users who primarily interact through OpenClaw itself.

The recommended installation path is:

- give this GitHub repository URL directly to OpenClaw / your agent
- let it read the repository as a skill source

Repository URL:

- `https://github.com/doashoi/openclaw-feishu-reasoning-ux`

## Minimal trigger examples

Examples:

- `Help me make OpenClaw replies in Feishu feel more layered and visible`
- `Why does Feishu only show Thinking... now?`
- `I want visible reasoning and better-looking Feishu cards`
- `Help me unify the title, colors, and collapsible reasoning panel in Feishu replies`

## Release status

This project is best treated as a:

- **practical public beta / methodology skill**

rather than a universal, guaranteed-stable product for every environment.

Why:
- OpenClaw version differences matter
- provider/model reasoning formats differ
- some environments execute compiled runtime paths rather than the source files you inspect

The strength of this skill is not "identical behavior everywhere".

Its strength is:
- the debugging order
- the capability detection logic
- the Feishu UX customization method
- the accumulated implementation experience

## Screenshots

### 1. Initial thinking state

![Initial thinking state](./assets/01-thinking-start.jpg)

### 2. Expanded reasoning content

![Expanded reasoning content](./assets/02-reasoning-expanded.jpg)

### 3. Collapsed reasoning with final reply

![Collapsed reasoning with final reply](./assets/03-reasoning-collapsed-reply.jpg)

## How this experience is implemented

At a high level:

1. determine whether the current model truly supports readable live reasoning
2. if it does, wire the reasoning live event into Feishu
3. split the Feishu card into two content lanes:
   - reasoning lane
   - answer lane
4. use `collapsible_panel` for reasoning
5. use a separate answer area for the final reply
6. collapse reasoning at the correct time instead of hiding it immediately

So the core challenge is not just "make the card prettier". It is aligning:

- provider
- runtime
- session
- dispatcher
- card layout

Detailed implementation and troubleshooting are documented in:

- [SKILL.md](./SKILL.md)
- [references/implementation-guide.md](./references/implementation-guide.md)

## FAQ

### 1. Why is raw reasoning sometimes in Chinese and sometimes in English?

That is usually normal. It does not automatically mean the card logic is broken.

Typical reasons:

- the model/provider may internally reason in mixed Chinese and English
- some runtimes pass upstream reasoning snapshots through to the UI with minimal transformation
- different models, providers, and prompts can change the reasoning language

If what you are seeing is truly raw reasoning, language consistency is not guaranteed.

### 2. Does that mean the skill stopped working?

Not necessarily.

Language drift is different from an actual failure.

Real failures look more like:
- only `Thinking...` is visible
- no raw reasoning appears at all
- final answer does not stream
- title/panel/send paths regress

If the only issue is that some reasoning turns English, that is more likely a model-output characteristic than a broken Feishu implementation.

### 3. Can we force all reasoning to appear in Chinese?

You can try presentation-layer rewriting, but that stops being strictly raw reasoning.

So there are two different goals:

- if you want truly raw reasoning, mixed language may be part of the output
- if you want consistently Chinese presentation, that becomes a transformed display layer

By default, this skill prioritizes preserving the authenticity of visible raw reasoning rather than silently rewriting it.

### 4. Why do some models stream raw reasoning while others only show Thinking...?

Because models/providers expose reasoning in different forms.

Common cases:

- readable live reasoning events
- thinking only in the final transcript
- encrypted/opaque reasoning payloads
- no exposed reasoning at all

So raw reasoning visibility is not only a Feishu card problem; it also depends on the model/provider path itself.

## Restart policy

If a change requires restarting `openclaw-gateway`, the agent must:

- explain why
- ask the user to run it
- or ask for confirmation first

It should never restart the gateway silently.

Unexpected restarts can disrupt:
- active Feishu conversations
- current session reasoning state
- ongoing tasks
