[English](./README.en.md) | [日本語](./README.md)

# Roundtable — Expert Panel Discussion

**Have you ever felt uneasy making an important decision with only your own perspective?**

Code architecture, business strategy, design direction — the harder it is to pin down a single right answer, the more you need multiple perspectives. But assembling several experts for a discussion is rarely practical.

Roundtable solves this problem.

Based on your topic, it automatically selects a panel of world-class experts, conducts a structured discussion, and delivers a multi-perspective evaluation with actionable recommendations. Rather than simply gathering opinions, it digs into points of disagreement and has built-in mechanisms to ensure minority views are not overlooked — so you never end up with "everyone said the same thing and called it a day."

---

## Who This Is For

- **Stuck on a design or technical decision** — You have multiple architecture options and don't know which is right
- **Deciding on a business or product direction** — Thinking alone feels like it introduces blind spots
- **Looking for review or feedback** — You want multi-perspective evaluation of a website, article, design, or anything else
- **Want to surface blind spots and risks** — You're worried about things you can't see on your own

No specialized knowledge is required. Just describe your topic, and everything from expert selection to discussion facilitation is handled automatically.

---

## Getting Started

### Claude Code

Run the following commands in order in your Claude Code session.

```
/plugin marketplace add saladdays/agent-skills
```

Once the marketplace is added, install the plugin.

```
/plugin install roundtable@saladdays-skills
```

After installation, start a roundtable session with `/roundtable:start [topic]`.

> This uses Claude Code's "plugin" system. Install it once, and it's available whenever you need it.

### Cursor

Search for `roundtable` in the Cursor plugin marketplace and install.

> [Cursor Marketplace](https://cursor.com/marketplace) — available for one-click installation once published.

### Other AI Tools

Copy [SKILL.md](./skills/start/SKILL.md) to your AI tool's skill directory.

---

## Usage

Just describe your topic. You can pass in a URL, file, text — anything.

```
/roundtable:start Review this website: https://example.com
/roundtable:start Review our new business strategy (attach document)
/roundtable:start Evaluate this code architecture
/roundtable:start Discuss our team's organizational structure
```

### How the Discussion Works

1. **Expert panel proposal** — A team of experts (3-7 members + a Devil's Advocate) is automatically selected based on the topic. You can request additions or changes
2. **Independent analysis** — Each expert completes their analysis independently, without seeing others' opinions
3. **Structured debate** — Points of disagreement are discussed in depth, with each side presenting evidence
4. **Report** — You receive a structured report covering consensus, disagreements, minority opinions, and recommended actions

---

## What's in the Report?

| Section | In a Nutshell |
|---|---|
| **Executive Summary** | The conclusion in 3-5 lines. The most important findings and recommended actions |
| **Expert Panel** | Who participated and from what perspective. Includes ratings |
| **Points of Consensus** | Where all experts agreed |
| **Key Debates and Conclusions** | Points of disagreement, what was decided, and why |
| **Minority Opinions** | Points raised by only one expert that should not be overlooked |
| **Recommended Actions** | A prioritized list of actions |
| **Limitations** | Perspectives this discussion did not cover |

The key point is that **minority opinions are structurally protected**. Even opinions that contradict the majority are included in the report as long as they are supported by evidence. This prevents the complacency of "everyone agrees, so it must be fine."

---

## Discussion Depth

The depth is automatically determined based on the topic's impact and risk level (manual override is also available).

| Depth | Phase 2 (Debate) | Second Chance | Use Case |
|---|---|---|---|
| Quick | Skipped | None | Personal projects, casual consultations |
| Standard | Up to 2 rounds | None | General evaluations and reviews |
| Deep | Up to 2 rounds | Yes | Business decisions, high-risk decision-making |

The number of experts scales from 3 to 7 (plus a Devil's Advocate) based on the complexity of the topic, independent of the depth setting.

---

## How It Works

### How Experts Are Selected

Simply gathering "people who know the field" leads to biased perspectives. Roundtable selects experts according to five principles.

1. **Failure detector** — If this initiative fails, who would notice the warning signs first? Include an expert with that perspective
2. **Recipient's viewpoint** — Include not only the creators' perspective but also the perspective of those affected: users, readers, and end consumers
3. **Adjacent domain expertise** — Add experts from neighboring fields who bring perspectives that are often overlooked
4. **Unexpected expertise** — Is there a field not normally consulted that holds important insights? An ecologist for organizational design, an ethics philosopher for AI development — cross-domain perspectives can yield fundamental discoveries
5. **Structural critic** — A Devil's Advocate is always present as a separate role from the domain experts, tasked with finding weaknesses that everyone else has missed

The system is also designed to avoid duplicating experts who share the same vantage point. For example, "editor and strategist" both represent the creator's side. Replacing one with a reader-side expert shifts the balance of the discussion.

This applies not just to code review but to business strategy, design evaluation, organizational design, editorial planning — for any topic, appropriate experts are automatically selected based on these principles.

### Why the Discussion Is Structured

When you ask an AI to "discuss from multiple perspectives," the result tends to be similar opinions lining up and reaching a polite consensus. This happens in human organizations too — in psychology, it is known as Groupthink. While everyone feels "it's probably fine," critical risks go unnoticed.

Roundtable addresses this by building structure into the discussion process itself.

- **Independent analysis comes first** — All participants complete their analysis without seeing others' opinions. This eliminates the anchoring effect, where people are swayed by the first opinion they hear (a principle of the Delphi Method)
- **Focus on points of disagreement** — Rather than repeatedly confirming what everyone already agrees on, discussion resources are concentrated on areas where opinions diverge
- **No more than two iterations** — Research on multi-agent debate has confirmed that three or more rounds of iteration lead to Degeneration of Thought, where minority voices converge toward the majority. The round limit is a structural mechanism for preserving diversity of opinion
- **Structural protection of minority opinions** — Even points that contradict the majority are retained in the report as long as they are supported by evidence. This prevents the reasoning that "everyone agrees, so it must be correct"

### Transparency of the Decision Process

The report contains not just conclusions but the reasoning process that led to them. It includes the evidence for and against each position, which side was adopted and why, and which minority opinions were set aside along with the rationale. Because the decision-making process is visible, whoever receives the report can independently evaluate whether the conclusions are trustworthy.

These design choices are grounded in academic research on Multi-Agent Debate, the Delphi Method, Kahneman's Adversarial Collaboration, and Janis's (1972) research on Groupthink. For details, see [SKILL.md](./skills/start/SKILL.md).

## License

MIT
