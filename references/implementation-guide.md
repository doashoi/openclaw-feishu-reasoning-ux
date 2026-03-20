# OpenClaw Feishu Card Customization Reference

This reference captures a proven debugging and implementation approach for OpenClaw Feishu card customization, especially when the desired UX includes:

- card 2.0 containerized layouts
- streaming answers
- raw reasoning visibility
- collapsible reasoning panels
- unified titles/colors across all message paths

## 1. The most important lesson

When a Feishu card regression appears, do not start by tweaking the visual template.

First determine which of these is broken:
- provider did not emit readable live reasoning
- current session lost reasoning state
- runtime callback wiring is wrong
- card streaming lane logic is wrong
- Feishu send/reply/update API path failed

Visual symptoms often come from lower layers.

## 2. Proven root-cause patterns

### Pattern A: Only shows `Thinking...`

Possible causes:
- no live reasoning events exist
- session lost `reasoningLevel=stream`
- `onReasoningStream` is attached to the wrong layer

What to check:
- current session file
- raw stream log
- runtime logs for `onReasoningStream`

### Pattern B: Raw reasoning repeats itself

Possible causes:
- runtime emits cumulative snapshots
- wrapper text like `Reasoning:` and outer italics are not normalized
- async queue uses mutable shared text and replays later content

Fix strategy:
- normalize snapshot
- compute delta against prior snapshot
- freeze queued text before async update

### Pattern C: Final answer exists but user cannot see it

Possible causes:
- final answer is suppressed by commentary filters
- streaming close/update failure prevents fallback send
- reply API path fails while direct send path would work

Fix strategy:
- confirm final answer exists in session transcript
- inspect reply/send API failures
- add fallback from reply to direct send where appropriate

### Pattern D: Works until a new session starts

Possible causes:
- session init does not preserve reasoning state
- only current session entry was patched manually

Fix strategy:
- patch session initialization defaults for the intended Feishu scope

## 3. Card 2.0 structure that works well

Recommended high-level structure:

- card header
- reasoning `collapsible_panel`
  - element id for reasoning content
- answer content element

Reasoning panel states:
- expanded while raw reasoning streams
- collapsed once answer streaming begins

Header and template strategy:
- use one shared title/template selection path
- do not let fallback sends use a different legacy title

## 4. Runtime conditions that matter

For raw reasoning to actually appear, these conditions must all be true:

- the model/provider path emits readable live reasoning
- the current session is in reasoning stream mode
- the Feishu dispatcher is wired to actual runtime reply options
- the card has a visible reasoning lane

If any one of these is false, the front-end card cannot compensate.

## 5. Model/provider caveat

Different providers can expose reasoning differently:
- some provide readable live reasoning text
- some only provide final transcript thinking
- some provide encrypted reasoning blobs

Do not assume one successful provider path generalizes to another.

### Practical capability test

When checking a model for Feishu raw reasoning support, use this decision tree:

1. Verify the current session truly ran on the intended provider/model.
2. Check `raw-stream.jsonl`.
   - If it shows live thinking text events, the model/runtime can support true raw reasoning.
3. Check the saved session transcript.
   - If transcript has readable `thinking` but no live events, the model supports post-hoc thinking but not true live raw reasoning through the current runtime path.
4. If transcript has only encrypted/opaque reasoning payloads, treat it as non-displayable raw reasoning.

This prevents a common mistake:
- blaming Feishu card code for a model/provider capability limit.

## 5.1 Turn technical diagnosis into user choices

When capability limits are found, do not stop at the technical conclusion.

Translate them into practical options:

- `Current model cannot expose raw reasoning, but we can still improve card 2.0 styling.`
- `Current model cannot expose raw reasoning. If you want raw reasoning, we should switch models.`
- `Current model path is unstable. We should first fix session/provider routing, then continue with card customization.`

This makes the skill usable by non-expert users.

## 6. Persistence caveat

If the behavior works only until restart or only in one current session, the fix is incomplete.

A stable solution must survive:
- gateway restart
- new direct Feishu session
- normal fallback send paths

## 6.1 Restart policy

Do not silently restart `openclaw-gateway`.

Even if restart is technically necessary, the agent should:
- explain why restart is needed
- ask the user to run it or confirm permission first

Unexpected restarts can disrupt:
- active Feishu chats
- session-specific reasoning state
- ongoing tasks

## 7. Practical file map

Most work lands in:
- `extensions/feishu/src/reply-dispatcher.ts`
- `extensions/feishu/src/streaming-card.ts`
- `extensions/feishu/src/send.ts`
- `extensions/feishu/src/outbound.ts`
- `extensions/feishu/src/channel.ts`
- `extensions/feishu/src/config-schema.ts`

If symptoms and source do not match, inspect the actual runtime-loaded code path, which may be compiled `dist/`.

## 8. Release-grade verification checklist

If this skill is being used on another user's machine, do not stop at "the setup looks similar".

Before concluding whether the skill worked, verify this exact checklist:

1. **Current session really runs on the intended provider/model**
- confirm in the latest session transcript
- do not trust only the global default or the agent's reply text

2. **Gateway service env matches the path that was proven to work**
- if local shell tests and live Feishu behavior differ, compare service env first
- proxy-related env drift is a common hidden cause

3. **Live reasoning events exist for the real Feishu request**
- inspect `~/.openclaw/logs/raw-stream.jsonl`
- if there is no live reasoning event there, card-layer changes cannot restore raw reasoning

4. **Runtime is in real reasoning stream mode**
- `reasoningMode = "stream"` is required for live reasoning callbacks
- plain thinking-enabled mode is not enough

5. **Reasoning callbacks are attached to final `replyOptions`**
- if `assistant_thinking_stream` exists in logs but Feishu still shows only `Thinking...`, this is the first place to inspect

6. **New Feishu direct sessions inherit reasoning defaults**
- if only the current session works, the fix is incomplete
- new sessions should preserve:
  - `reasoningLevel = "stream"`
  - `thinkingLevel = "low"`

7. **The target model is really registered in the local model table**
- especially for `minimax-cn/MiniMax-M2.7`
- a provider can support the model while local model selection still falls back to another model

## 9. Known traps

### Trap A: "Same official Feishu channel + same minimax-cn" still failed

This does not prove the skill is wrong.

It often means one of these differed:
- session override
- model table registration
- gateway service env
- runtime callback wiring
- loaded `dist/` vs edited `src/`

### Trap B: `/new` or the next day breaks raw reasoning again

This usually means the current session was patched, but session initialization was not.

### Trap C: Model looks correct on screen, but behavior matches another model

Check whether the local model registry silently falls back.
This is especially relevant for `minimax-cn/MiniMax-M2.7`.
