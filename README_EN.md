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

## Installation

Copy this directory into your skill directory, for example:

- `~/.codex/skills/`
- `~/.agents/skills/`

depending on your agent environment.

## Suggested trigger prompts

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

## Suggested next steps

If you want to publish this repository to more skill libraries, consider adding:

- repository topics
- one or two real screenshots
- a minimal installation example
- a minimal trigger prompt example
