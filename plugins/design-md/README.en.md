[English](./README.en.md) | [日本語](./README.md)

# DESIGN.md Generator

**When AI builds your UI, does every screen end up looking different?**

The first screen looks great. Then the second screen uses a different font. The third screen drifts on color. The more you try to fix it, the more you lose what made the first screen good.

DESIGN.md solves this problem.

By consolidating your product's design decisions into a single file, you give AI-generated UI a consistent foundation. You can start from what you already have — your existing brand identity, Figma designs, or the look and feel of services you admire. No need to start from scratch. No design expertise required. Just answer a few questions in a conversation, and you'll have a DESIGN.md tailored to your product in minutes.

### DESIGN.md With / Without Comparison

Here's a side-by-side: the same fashion e-commerce site, built across 5 screens — once **with** DESIGN.md and once **without**.

**With DESIGN.md** — Colors, fonts, and components stay consistent across every screen

| Top Page | Cart |
|---|---|
| ![WITH - Top](./images/with-ec-top.png) | ![WITH - Cart](./images/with-ec-cart.png) |

**Without DESIGN.md** — Fonts, colors, and header structures vary from screen to screen

| Top Page | Cart |
|---|---|
| ![WITHOUT - Top](./images/without-ec-top.png) | ![WITHOUT - Cart](./images/without-ec-cart.png) |

Without DESIGN.md, 5 screens produced 4 different heading fonts and 4 separate color palettes. Fixing that kind of inconsistency takes 6-10 hours. Creating a DESIGN.md takes a few minutes of conversation.

---

## Best Paired with Claude Design

If you use [Claude Design](https://support.claude.com/en/articles/14604397-set-up-your-design-system-in-claude-design) to create screens conversationally, you can turn the resulting tokens and design decisions into a DESIGN.md and carry the same judgment straight into Cursor or Claude Code during implementation.

Going the other direction also works: when needed, DESIGN.md can be exported into a W3C Design Tokens Community Group-style `design-tokens.json`, making it usable in Claude Design and other token-aware tools. That format is support infrastructure, not the destination. The core value of DESIGN.md is the authored decision layer above tokens: why a choice exists, what to prioritize when principles conflict, and how to preserve tone across new screens. In practice, that continuity from prototype to production feels especially strong.

---

## Who This Is For

- **PMs and engineers** — You're shipping AI-generated prototypes fast, but every new screen looks different from the last
- **Designers** — You want AI to follow your design intent, but it keeps missing the mark
- **Engineers without a design background** — You ask for "something that looks good" and get something different every time
- **Teams with a live product** — You want AI to build new screens that match your existing look and feel

No design expertise needed. If you're unsure about something, just say so — the AI fills in the gaps.

---

## Getting Started

### Using with Claude Code

Run the following commands in Claude Code, in order.

```
/plugin marketplace add saladdays/agent-skills
```

Once the marketplace is added, install the plugin.

```
/plugin install design-md@saladdays-skills
```

After installation, run `/design-md:generate` to start generating your DESIGN.md.

> This uses Claude Code's plugin system. Install once, use anytime.

### Using with Cursor

Search for `design-md` in the Cursor plugin marketplace and install it.

> [Cursor Marketplace](https://cursor.com/marketplace) — one-click install available once published.

### Using with Other AI Tools

DESIGN.md is just a Markdown file, so it works with any AI tool. Hand over the [spec](./DESIGN-MD-SPEC.md) and a [completed example](#completed-examples), and tell the AI to create one for your product.

---

## How It Works

Just have a conversation with the AI. It will ask you things like:

```
AI: What does your service do? Who uses it?
AI: Describe the UI vibe in three words. Any services you draw inspiration from?
AI: Do you have an existing site or Figma? I can take a look.
AI: Any impressions you want to avoid?
```

If you share an existing URL or Figma link, the AI analyzes your current design and builds the DESIGN.md from there. If you already have token files in W3C DTCG JSON or CSS variables, it can use those as a starting point too. Starting from nothing works just as well.

You don't need to answer every question. The AI fills in reasonable defaults where needed. The whole conversation takes about 5-10 minutes.

Once it's ready, just place `DESIGN.md` in your project root. If you use CLAUDE.md, add `@DESIGN.md` so the AI picks it up automatically.

---

## What's Inside a DESIGN.md?

| Section | In a Nutshell |
|---|---|
| **TL;DR** | Your design in 5 lines |
| **Design Principles** | What makes your product feel like *your product*, put into words |
| **Color Strategy** | Which colors to use and the rules for how to apply them |
| **Typography** | Font sizes, typefaces, and line-height decisions |
| **Spacing & Layout** | How whitespace and layout are structured |
| **Component Patterns** | When to use which buttons, cards, and other components |

The key difference: it doesn't just list values — it explains **why each value was chosen and how to make decisions when things are ambiguous**. That's what lets AI make consistent choices on screens it has never seen before.

---

## Completed Examples

DESIGN.md files created for three different products.

- [SaaS Dashboard](./skills/generate/resources/examples/saas-dashboard.md) — A data-heavy admin panel
- [Creator Platform](./skills/generate/resources/examples/creator-platform.md) — A service for sharing articles and photos
- [Meditation Timer](./skills/generate/resources/examples/minimal-zen.md) — A radically simple app

---

## Design Philosophy

- **Tailored to your product** — Not a cookie-cutter template. The conversation draws out rules from your existing design and brand identity
- **Preserves AI creativity** — Colors and fonts are locked down, but there's deliberate room for the AI to improvise where it matters
- **Perfection not required** — Start with something, then refine it as you go

---

## Learn More

- [DESIGN-MD-SPEC.md](./DESIGN-MD-SPEC.md) — Format specification
- [writing-guide.md](./skills/generate/resources/writing-guide.md) — Good vs. bad writing patterns, side by side

## Background

Inspired by the [DESIGN.md format](https://stitch.withgoogle.com/docs/design-md/overview) introduced by [Google Stitch](https://stitch.withgoogle.com/) in March 2026. This project carries forward Stitch's vision of "design system definitions that AI can read," while evolving it into a tool-agnostic format that doesn't depend on any specific platform. It's not fully compatible with Stitch, but it shares the same underlying motivation.

## License

MIT
