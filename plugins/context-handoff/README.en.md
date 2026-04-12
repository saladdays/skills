[English](./README.en.md) | [日本語](./README.md)

# context-handoff

**Pick up right where you left off when working with AI agents.**

Tired of re-explaining the same context every time you start a new session? Frustrated when the AI retries an approach you already know failed last time?

context-handoff automatically saves and restores context across sessions. No manual saving or resuming required.

## Who This Is For

- You use AI coding agents (Claude Code, Cursor, etc.) on a daily basis
- You feel the friction of "starting over from scratch" every time a session ends
- You've watched the AI repeat the exact same failed approach from a previous session
- You work across multiple PCs and want to seamlessly continue where you left off

## Usage

### Initial Setup

```
/context-handoff:save
```

That's it. The skill handles everything: hook configuration, adding rules to CLAUDE.md, and generating the initial handoff.md.

### After That, You Don't Need to Do Anything

- **During work**: Handoff notes accumulate automatically whenever the AI makes an important decision
- **When context is full**: Notes are automatically organized
- **When you start a new session**: The previous state is loaded automatically, and the AI displays a 3-line summary

If you start a session before sending your first message, the AI will confirm the handoff for you.

```
AI: Picking up from last session:
    - Working on: auth refactor (src/auth.ts, src/session.ts)
    - Note: passport.js abandoned due to custom header incompatibility
    - Next: add error handling
    Does this look right?

You: yep, let's keep going
```

Even if you send a message first, the handoff information is already loaded into the AI's context. Just say "continue from last time" whenever you want to pick up.

### When You Want to Save Explicitly

```
/context-handoff:save
```

Use this at major milestones or when you want to ensure an especially accurate save. Auto-save is sufficient for everyday use.

### When Continuing on a Different PC

```
git push   (on your current PC)
git pull   (on the other PC)
claude     (picks up right where you left off)
```

## Why It Works This Way

### A Working-State Snapshot, Not a Conversation Summary

Many AI tools summarize the conversation for handoff, but errors accumulate as summaries are layered on top of each other (the recursive summarization hallucination problem). context-handoff does not summarize the conversation. Instead, it saves **only the information that git doesn't capture**, in a structured format. Since git already records what changed in the code, the handoff notes only contain "why that change was made" and "what was tried and didn't work."

### Recording What You Didn't Do Is the Most Valuable Part

According to Chris Parnin's research on resuming interrupted programming tasks, the biggest cost developers face after an interruption is "re-exploring the same dead ends." AI agents lack the human Zeigarnik effect (the tendency to remember unfinished tasks), so without a record of failures, they will repeat the same mistakes over and over. context-handoff **treats the Dead Ends (abandoned attempts) section as the highest priority** and never compresses the reasoning behind why something was rejected.

### Three-Layer Saving Means You're Covered No Matter How the Session Ends

| When | What Happens |
|---|---|
| An important decision is made during work | Automatically appended as a single line (you won't even notice) |
| Context reaches capacity | Notes are automatically organized (you won't even notice) |
| You want to save manually | Run `/context-handoff:save` |

Whether you close the terminal or your PC goes to sleep, the most recent state is preserved.

## Installation

### Claude Code

Run the following commands in order in Claude Code.

```
/plugin marketplace add saladdays/agent-skills
```

Once the marketplace is added, install the plugin.

```
/plugin install context-handoff@saladdays-skills
```

After installation, run `/context-handoff:save` to start the initial setup.

> This uses Claude Code's "plugin" system. Install once, and it's always available.

### Cursor

Search for `context-handoff` in the Cursor plugin marketplace and install.

> [Cursor Marketplace](https://cursor.com/marketplace) — one-click install once published.

### Other AI Tools

Copy SKILL.md to your AI tool's skill directory.

## License

MIT
