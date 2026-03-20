# OpenClaw Feishu Reasoning UX

Make OpenClaw in Feishu more observable, streamable, and customizable.

This project is a skill for agents that need to customize or repair the Feishu reply experience in OpenClaw. It focuses on:

- visible streaming reply UX
- raw reasoning diagnosis and display paths
- card 2.0 layouts
- collapsible reasoning panels
- title/color/template consistency
- system-level debugging of Feishu reply regressions

## What this skill is

This is **not** just a "send Feishu cards" skill.

It is a practical methodology and customization skill for turning OpenClaw's Feishu reply experience from a black box into something more:

- observable
- layered
- debuggable
- styleable

It helps an agent:
- detect whether the current model/provider can expose readable raw reasoning
- distinguish runtime/session/provider problems from card-layer problems
- customize Feishu reply cards based on user taste
- preserve behavior across restart and new sessions

## What this skill is not

This skill does **not** guarantee raw reasoning on every model.

Some models/providers:
- do not expose live reasoning events
- only expose transcript thinking after completion
- expose encrypted or opaque reasoning payloads

In those cases, this skill should guide the agent to:
- explain the limitation clearly
- offer alternatives
- still improve Feishu card 2.0 experience where possible

## Who this is for

Use this if you:
- use OpenClaw with Feishu
- want better reply UX than the default black-box behavior
- want visible reasoning where the model/runtime supports it
- want card 2.0 styling, collapsible panels, and better titles/colors
- need a structured debugging workflow when Feishu reply behavior regresses

## What the agent should do with this skill

After loading this skill, the agent should:

1. Check the current model/provider/session path first
2. Detect whether raw reasoning is actually supported in the current route
3. Explain findings in plain language
4. Offer practical next steps to the user
5. Only then customize Feishu card behavior and styling

The skill is intentionally written to be user-friendly:
- it should ask preference questions like title text, panel wording, and color style
- it should not assume the user understands OpenClaw internals
- it should not silently restart `openclaw-gateway`

## Files

- [SKILL.md](./SKILL.md): main skill
- [references/implementation-guide.md](./references/implementation-guide.md): deeper reference for root-cause patterns and implementation structure

## Install

Copy this directory into your skill location, for example:

- `~/.codex/skills/`
- `~/.agents/skills/`

depending on your agent environment.

## Suggested trigger phrases

Examples:

- `帮我把 OpenClaw 在飞书里的回复改得更有层次一点`
- `为什么飞书里只剩 Thinking... 了`
- `我想要可见的推理流和更好看的 Feishu 卡片`
- `帮我统一飞书回复卡片的标题、颜色和折叠面板`

## Stability and release status

This is best treated as a **practical public beta / methodology skill**, not a universal guarantee.

Why:
- OpenClaw version differences matter
- provider/runtime behavior differs by model
- some environments execute compiled runtime paths rather than edited source

Its strength is not "works identically everywhere".

Its strength is:
- the debugging order
- the capability detection logic
- the Feishu UX customization method
- the accumulated implementation experience

## Important policy

If a change requires restarting `openclaw-gateway`, the agent should:
- explain why
- ask the user to run it
- or get confirmation first

It should never restart the gateway silently.
