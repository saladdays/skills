[English](./README.en.md) | [日本語](./README.md)

# Best Practice — Research Best Practices

**Have you ever searched for "best practices" only to be misled by outdated information or personal opinions?**

Technology selection, security measures, pricing strategy — you want to find the right approach, but search results are a mixed bag. A three-year-old article ranks at the top, or a vendor's product promotion is presented as a best practice.

Search Best Practice solves this problem.

It researches the latest best practices via web search and produces a structured report that accounts for freshness, reliability, and multiple perspectives. By separating "principles (things that don't change)" from "tools and methods (things that change quickly)," you can distinguish between information that will be outdated next month and insights that will still be useful three years from now.

---

## Who This Is For

- **Making a technology decision** — You want to know the "current right answer" for frameworks, libraries, or infrastructure architecture
- **Entering an unfamiliar domain** — You want to efficiently catch up on best practices outside your area of expertise
- **Building decision criteria for your team** — You want to explain "why this approach is best" with supporting evidence
- **Concerned about information freshness** — You want to avoid the risk of making decisions based on outdated articles

No specialized knowledge is required. Just tell it what you want to know, and the research and structuring are handled automatically.

---

## Getting Started

### Claude Code

Run the following commands in order in your Claude Code session.

```
/plugin marketplace add saladdays/agent-skills
```

Once the marketplace is added, install the plugin.

```
/plugin install best-practice@saladdays-skills
```

After installation, start a research session with `/best-practice:search [topic]`.

> This uses Claude Code's "plugin" system. Install it once, and it's available whenever you need it.

### Cursor

Search for `best-practice` in the Cursor plugin marketplace and install.

> [Cursor Marketplace](https://cursor.com/marketplace) — available for one-click installation once published.

### Other AI Tools

Copy [SKILL.md](./skills/search/SKILL.md) to your AI tool's skill directory.

---

## Usage

Just describe the topic you want to research. The domain is determined automatically.

```
/best-practice:search Kubernetes secret management best practices
/best-practice:search SaaS pricing strategy best practices
/best-practice:search How to build reading habits in children
```

### How the Research Works

1. **Automatic assessment** — The theme's freshness requirements, risk level, and necessary perspectives are automatically analyzed
2. **Web research** — Research is conducted from reliable sources, focusing on official documentation, academic papers, and industry reports
3. **Counter-evidence search** — Anti-patterns and opposing viewpoints are deliberately sought to prevent one-sided conclusions
4. **Structured report** — You receive a ready-to-use report with principles and tools clearly separated

---

## What's in the Report?

| Section | In a Nutshell |
|---|---|
| **Executive Summary** | The conclusion in brief. The most important best practices |
| **Principles (Layer S)** | Universal principles that will hold true three years from now. Includes rationale for each |
| **Tools & Tactics (Layer V)** | The best options available today. Includes freshness and recommendation ratings |
| **Anti-Patterns** | Things to avoid. Common mistakes and why they happen |
| **Decision Matrix** | Situational selection criteria ("In this case, choose A") |
| **Sources** | All referenced sources with reliability assessments |

The key point is that **principles and tools are separated**. You can distinguish between "Kubernetes secrets should be stored in an external store" (principle = stable) and "Use Vault" (tool = subject to change), so even when tools change, your decision criteria endure.

---

## Supported Domains

Covers technology, business, education, healthcare, legal, design, creative, and any other domain. Because it is designed on a principle-based approach rather than keyword lists, it adapts to unfamiliar domains as well.

Verified with 15 topics x 3 rounds of testing, all completed successfully.

---

## Research Depth

The depth is automatically determined based on the topic's risk level and freshness requirements.

| Depth | Research Scope | Use Case |
|---|---|---|
| Quick | Focused on key sources, concise | General questions, low risk |
| Standard | Multiple sources + counter-evidence search | Technology selection, business processes |
| Deep | Comprehensive research + risk analysis | Security, healthcare, legal, finance |

---

## How It Works

### How Information Is Evaluated

Articles that rank highly in web searches are not necessarily accurate. Search Best Practice applies three evaluation criteria during the information-gathering phase.

1. **Source authority** — Official documentation, academic papers, and publications from industry standards bodies are more reliable than individual anecdotes. Sources are rated on a four-tier scale — official, specialized institutions, practitioners, individuals — and higher-reliability sources are prioritized
2. **Information freshness** — The "shelf life" varies by topic. Security practices can change in six months, while organizational design principles remain stable for decades. Appropriate freshness thresholds are automatically determined per topic, and information exceeding those thresholds is flagged with a note
3. **Counter-evidence search** — Before reaching a "this is best" conclusion, opposing viewpoints and anti-patterns are deliberately sought. This structurally eliminates confirmation bias — the tendency to collect only information that supports your existing hypothesis

These are adapted from information literacy research frameworks — SIFT (Stop, Investigate, Find, Trace) and the CRAAP test (Currency, Relevance, Authority, Accuracy, Purpose) — integrated into the AI research process.

The same evaluation criteria apply not only to technical topics but also to pricing strategy, educational methods, organizational management, and best practice research in any field.

### Why Principles and Tools Are Separated

Among what are called "best practices," insights that will hold up for a decade coexist with information that will be outdated by next year.

For example, "encrypt customer data at rest" is a principle — it is unaffected by technology trends. On the other hand, "use AES-256" is a current recommendation that may be replaced by a more appropriate method in the future. "Base pricing on customer willingness to pay" is a principle, but "use Stripe Billing" is a tool choice.

If you bundle these together without distinguishing them, you end up having to revisit principles whenever a tool is updated — or, conversely, clinging to an outdated tool because it's packaged with a still-valid principle. By keeping them separate, your decision-making framework remains intact even as the methods change.

### The Value of This Approach Itself

The reports from Search Best Practice are of course useful in practice, but what is even more valuable is the **information evaluation framework that the reports embody**.

"Verify the authority of the source." "Be aware of information freshness." "Look for counter-evidence." "Separate principles from methods." These are ways of thinking that apply even without AI. By engaging with the structure of these reports, your judgment in everyday information gathering naturally sharpens over time.

For details, see [SKILL.md](./skills/search/SKILL.md).

## License

MIT
