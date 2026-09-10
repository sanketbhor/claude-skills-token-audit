# claude-skills-token-audit
You have no idea where your tokens are actually going . So i built a "token audit" skill for Claude that breaks down exactly what's eating your context window .

# Token Audit

You have no idea where your tokens are actually going.

A Claude skill that breaks down exactly what's eating your context window or API bill — system prompt, tool definitions, repeated file reads, conversation history — and tells you the specific fix, not generic "be more concise" advice.

Works in **Claude Code**, **Claude Cowork**, and **Claude.ai**.

---

## What's a skill?

A skill is a small set of instructions you give to your AI — like a job description for one specific task. Install it once, trigger it with a phrase, and Claude knows exactly how to handle that job from then on.

This skill teaches Claude to run a "token audit" on your current conversation or API setup: it categorizes where every token is going, ranks the biggest offenders, and gives you concrete fixes you can apply in the next 5 minutes.

## What it does

One vague number ("used 40k tokens") tells you nothing. This skill turns that number into a breakdown:

- System prompt overhead
- Tool/skill definitions (often bigger than people expect)
- Files read into context — including files read *more than once*
- Accumulated conversation history
- The actual task content that's irreducible

Then it names the top 2-3 specific offenders and gives you an exact fix for each — not "reduce usage," but "you re-read this file 3 times, cache it" or "this tool description alone costs 800 tokens on every single call."

## When to use it

Good moments to run it:
- "Why did this conversation get so expensive?"
- "I keep running out of context window."
- "Where is my API bill actually going?"
- Any time token usage surprised you

## How to install (no terminal needed)

Open a new chat in Claude and paste this in:

> Please install this Claude skill for me. The SKILL.md file lives in this GitHub repo: [repo URL]
>
> Set it up so I can start using it. Walk me through anything you need from me.

Claude will fetch the file and set it up. If it can't do it automatically, it'll tell you exactly what to click.

## How to use it

Once installed, just say:

- "audit my tokens"
- "where is my context going"
- "why is my API bill so high"

Claude will run the breakdown and hand you a ranked list of fixes.

---

Built as an experiment in applying diagnostic/instrumentation thinking (the QA engineer's habit of "don't guess, measure it") to LLM token usage.
