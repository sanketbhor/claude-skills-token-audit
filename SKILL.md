---
name: token-audit
description: Breaks down where tokens are actually going in a Claude conversation or API session — system prompt, tool definitions, file reads, conversation history — and flags the biggest offenders with concrete fixes. Use this whenever the user asks about token usage, context window size, why they're running out of context, API costs, "why is this so expensive," how to reduce tokens/cost, or wants to understand what's eating their context. Also trigger on phrases like "token audit," "where are my tokens going," "context is filling up too fast," or "how do I cut my API bill." Be proactive about suggesting this skill any time the user expresses surprise or frustration about token usage or LLM API cost, even if they don't name it directly.
---

# Token Audit

You ask Claude "why am I burning through tokens so fast" and you get a vague answer, or none at all. This skill fixes that: it turns invisible token spend into a categorized breakdown with specific, actionable fixes — not generic advice like "be more concise."

## When to run this

Trigger on:
- Direct requests: "audit my tokens," "where's my context going," "why is my API bill so high"
- Frustration signals: "I keep running out of context," "this conversation got expensive fast," "why did that cost so much"
- Proactive moments: if you notice a conversation, codebase, or session that's clearly heavy on repeated large file reads, verbose tool definitions, or long-accumulated history, offer to run the audit even if unasked.

## How to run the audit

### Step 1: Gather the inputs

Depending on context, pull from what's available:
- **Live conversation (Claude.ai / Claude Code session)**: the system prompt, the tool/skill definitions currently loaded, the file contents that have been read into context, and the accumulated message history.
- **API integration**: ask the user for (or read from logs/code) the system prompt string, the full `tools` array being sent, and a sample request/response payload with `usage` token counts.
- **Codebase/agent setup**: scan for large static context — long system prompts, verbose tool descriptions, files read wholesale instead of chunked, repeated re-reads of the same file across turns.

If any of these aren't available, ask the user directly rather than guessing — a wrong estimate is worse than a partial one.

### Step 2: Categorize and estimate

Break the total into buckets:
1. **System prompt / instructions** — fixed overhead paid on every single call
2. **Tool / skill definitions** — often underestimated; verbose tool descriptions get sent on every call whether or not the tool is used
3. **File / document content read into context** — flag any file read more than once in the same session
4. **Conversation history** — accumulated turns, especially old tool results still sitting in context
5. **Actual task content** — the part that's irreducible, i.e. the real question/code/data the user needed processed

Use word-count-to-token approximation (~0.75 words per token as a rough rule) if exact counts aren't available, and say clearly when a number is an estimate rather than measured.

### Step 3: Identify the biggest offenders

Rank the buckets by size. For each of the top 2-3, name the *specific* culprit, not the category — e.g. "this one file was read 3 separate times" rather than "file reads are high." Specificity is what makes the audit useful instead of generic.

### Step 4: Give concrete fixes, not platitudes

Bad: "Try to be more concise."
Good: "Cache this file's content after the first read instead of re-fetching it each turn" / "This tool description is 800 tokens and repeats on every single call — trim the examples down to one" / "This system prompt section duplicates instructions already covered elsewhere — cut lines X-Y."

Every fix should be something the user could apply in the next 5 minutes.

### Step 5: Present the breakdown

Present as a simple ranked list or table: bucket → approximate tokens → % of total → top fix. If a charting tool is available, a bar chart of the breakdown is the single most shareable output of this whole skill — prioritize showing it visually when possible. If cost data (price per token) is available or the user provides it, convert the top offenders into an actual $ or ₹ figure — concrete cost lands harder than an abstract token count.

### Step 6: Offer the recheck

End by offering to re-run the audit after changes are made, so the user can see the before/after — that comparison is the clearest proof the fixes worked.

## Notes

- Never fabricate precise token counts you don't have a way to measure — clearly mark estimates as estimates.
- Keep the tone diagnostic and matter-of-fact, not alarmist — the goal is clarity, not making the user feel bad about their setup.
