---
name: competitor-teardown
description: "Structured competitive analysis with feature matrices, SWOT, positioning maps, and UX review. Covers research frameworks, pricing comparison, review mining, and visual deliverables. Use for: market research, competitive intelligence, investor decks, product strategy, sales enablement. Triggers: competitor analysis, competitive analysis, competitor teardown, market research, competitive intelligence, swot analysis, competitor comparison, market landscape, competitor review, competitive landscape, feature comparison, market positioning"
allowed-tools: Bash(belt *)
---

> **Install the belt CLI skill:** `npx skills add belt-sh/cli`

# Competitor Teardown

Structured competitive analysis with research and screenshots via [inference.sh](https://inference.sh) CLI.

## Quick Start

> Requires inference.sh CLI (`belt`). [Install instructions](https://raw.githubusercontent.com/inference-sh/skills/refs/heads/main/cli-install.md)

```bash
belt login

# Research competitor landscape
belt app run tavily/search-assistant --input '{
  "query": "top project management tools comparison 2024 market share"
}'

# Screenshot competitor's website
belt app run infsh/agent-browser --input '{
  "url": "https://competitor.com"
}'
```


## Teardown Framework

### The 7-Layer Analysis

| Layer | What to Analyze | Data Source |
|-------|----------------|-------------|
| 1. **Product** | Features, UX, quality | Screenshots, free trial |
| 2. **Pricing** | Plans, pricing model, hidden costs | Pricing page, sales call |
| 3. **Positioning** | Messaging, tagline, ICP | Website, ads |
| 4. **Traction** | Users, revenue, growth | Web search, press, funding |
| 5. **Reviews** | Strengths, weaknesses from users | G2, Capterra, App Store |
| 6. **Content** | Blog, social, SEO strategy | Website, social profiles |
| 7. **Team** | Size, key hires, background | LinkedIn, About page |

## Research Commands

### Company Overview

```bash
# General intelligence
belt app run tavily/search-assistant --input '{
  "query": "CompetitorX company overview funding team size 2024"
}'

# Funding and financials
belt app run exa/search --input '{
  "query": "CompetitorX funding round series valuation investors"
}'

# Recent news
belt app run tavily/search-assistant --input '{
  "query": "CompetitorX latest news announcements 2024"
}'
```

### Product Analysis

```bash
# Feature comparison
belt app run exa/search --input '{
  "query": "CompetitorX vs alternatives feature comparison review"
}'

# Pricing details
belt app run tavily/extract --input '{
  "urls": ["https://competitor.com/pricing"]
}'

# User reviews
belt app run tavily/search-assistant --input '{
  "query": "CompetitorX reviews G2 Capterra pros cons 2024"
}'
```

### UX Screenshots

```bash
# Homepage
belt app run infsh/agent-browser --input '{
  "url": "https://competitor.com"
}'

# Pricing page
belt app run infsh/agent-browser --input '{
  "url": "https://competitor.com/pricing"
}'

# Signup flow
belt app run infsh/agent-browser --input '{
  "url": "https://competitor.com/signup"
}'
```

## Feature Matrix

### Structure

```markdown
| Feature | Your Product | Competitor A | Competitor B | Competitor C |
|---------|:---:|:---:|:---:|:---:|
| Real-time collaboration | ✅ | ✅ | ❌ | ✅ |
| API access | ✅ | Paid only | ✅ | ❌ |
| SSO/SAML | ✅ | Enterprise | ✅ | Enterprise |
| Custom reports | ✅ | Limited | ✅ | ❌ |
| Mobile app | ✅ | iOS only | ✅ | ✅ |
| Free tier | ✅ (unlimited) | ✅ (3 users) | ❌ | ✅ (1 project) |
| Integrations | 50+ | 100+ | 30+ | 20+ |
```

### Rules

- ✅ = Full support
- ⚠️ or "Partial" = Limited or conditional
- ❌ = Not available
- Note conditions: "Paid only", "Enterprise tier", "Beta"
- Lead with features where YOU win
- Be honest about competitor strengths — credibility matters

## Pricing Comparison

### Structure

```markdown
| | Your Product | Competitor A | Competitor B |
|---------|:---:|:---:|:---:|
| **Free tier** | Yes, 5 users | Yes, 3 users | No |
| **Starter** | $10/user/mo | $15/user/mo | $12/user/mo |
| **Pro** | $25/user/mo | $30/user/mo | $29/user/mo |
| **Enterprise** | Custom | Custom | $50/user/mo |
| **Billing** | Monthly/Annual | Annual only | Monthly/Annual |
| **Annual discount** | 20% | 15% | 25% |
| **Min seats** | 1 | 5 | 3 |
| **Hidden costs** | None | Setup fee $500 | API calls metered |
```

### What to Look For

- Minimum seat requirements
- Annual-only billing (reduces flexibility)
- Feature gating between tiers
- Overage charges
- Setup/onboarding fees
- Contract lock-in periods

## SWOT Analysis

Create a SWOT for each competitor:

```markdown
### Competitor A — SWOT

| Strengths | Weaknesses |
|-----------|------------|
| • Strong brand recognition | • Slow feature development |
| • Large integration ecosystem | • Complex onboarding (30+ min) |
| • Enterprise sales team | • No free tier |

| Opportunities | Threats |
|--------------|---------|
| • AI features not yet shipped | • New AI-native competitors |
| • Expanding into mid-market | • Customer complaints about pricing |
| • International markets untapped | • Key engineer departures (LinkedIn) |
```

## Positioning Map

A 2x2 matrix showing where competitors sit on two meaningful dimensions.

### Choose Meaningful Axes

| Good Axes | Bad Axes |
|-----------|----------|
| Simple ↔ Complex | Good ↔ Bad |
| SMB ↔ Enterprise | Cheap ↔ Expensive (too obvious) |
| Self-serve ↔ Sales-led | Old ↔ New |
| Specialized ↔ General | Small ↔ Large |
| Opinionated ↔ Flexible | — |

### Template

```
                    Enterprise
                        │
           Competitor C │  Competitor A
                ●       │       ●
                        │
  Simple ──────────────────────────── Complex
                        │
            You ●       │  Competitor B
                        │       ●
                        │
                      SMB
```

### Generating the Visual

```bash
# Create positioning map with Python
belt app run infsh/python-executor --input '{
  "code": "import matplotlib.pyplot as plt\nimport matplotlib\nmatplotlib.use(\"Agg\")\n\nfig, ax = plt.subplots(figsize=(10, 10))\n\n# Competitors\ncompetitors = {\n    \"You\": (-0.3, -0.3),\n    \"Competitor A\": (0.5, 0.6),\n    \"Competitor B\": (0.6, -0.4),\n    \"Competitor C\": (-0.4, 0.5)\n}\n\nfor name, (x, y) in competitors.items():\n    color = \"#22c55e\" if name == \"You\" else \"#6366f1\"\n    size = 200 if name == \"You\" else 150\n    ax.scatter(x, y, s=size, c=color, zorder=5)\n    ax.annotate(name, (x, y), textcoords=\"offset points\", xytext=(10, 10), fontsize=12, fontweight=\"bold\")\n\nax.axhline(y=0, color=\"grey\", linewidth=0.5)\nax.axvline(x=0, color=\"grey\", linewidth=0.5)\nax.set_xlim(-1, 1)\nax.set_ylim(-1, 1)\nax.set_xlabel(\"Simple ← → Complex\", fontsize=14)\nax.set_ylabel(\"SMB ← → Enterprise\", fontsize=14)\nax.set_title(\"Competitive Positioning Map\", fontsize=16, fontweight=\"bold\")\nax.grid(True, alpha=0.3)\nplt.tight_layout()\nplt.savefig(\"positioning-map.png\", dpi=150)\nprint(\"Saved\")"
}'
```

## Review Mining

### Where to Find Reviews

| Platform | Best For | URL Pattern |
|----------|----------|-------------|
| G2 | B2B SaaS | g2.com/products/[product]/reviews |
| Capterra | Business software | capterra.com/software/[id]/reviews |
| App Store | iOS apps | apps.apple.com |
| Google Play | Android apps | play.google.com |
| Product Hunt | Launches | producthunt.com/posts/[product] |
| Reddit | Honest opinions | reddit.com/r/[relevant-sub] |

### What to Extract

| Category | Look For |
|----------|---------|
| **Most praised** | What features do happy users mention most? |
| **Most complained** | What do unhappy users say? (= your opportunity) |
| **Switching reasons** | Why do users leave? What triggers switching? |
| **Feature requests** | What's missing that users want? |
| **Comparison mentions** | When users compare, what do they say? |

```bash
# Mine G2 reviews
belt app run tavily/search-assistant --input '{
  "query": "CompetitorX G2 reviews complaints issues 2024"
}'

# Reddit sentiment
belt app run exa/search --input '{
  "query": "reddit CompetitorX alternative frustration switching"
}'
```

## Deliverable Formats

### Executive Summary (1 page)

```markdown
## Competitive Landscape Summary

**Market:** [Category] — $[X]B market growing [Y]% annually

**Key competitors:** A (leader), B (challenger), C (niche)

**Our positioning:** [Where you sit and why it matters]

**Key insight:** [One sentence about the biggest opportunity]

| Metric | You | A | B | C |
|--------|-----|---|---|---|
| Users | X | Y | Z | W |
| Pricing (starter) | $X | $Y | $Z | $W |
| Rating (G2) | X.X | Y.Y | Z.Z | W.W |
```

### Detailed Report (per competitor)

1. Company overview (size, funding, team)
2. Product analysis (features, UX screenshots)
3. Pricing breakdown
4. SWOT analysis
5. Review analysis (top praised, top complained)
6. Positioning vs. you
7. Opportunity summary

## Comparison Grid Visual

```bash
# Stitch competitor screenshots into comparison
belt app run infsh/stitch-images --input '{
  "image1": "your-homepage.png",
  "image2": "competitorA-homepage.png",
  "orientation": "horizontal"
}'
# Stitches two images per run; stitch the result with competitorB-homepage.png to add a third
```

## Common Mistakes

| Mistake | Problem | Fix |
|---------|---------|-----|
| Only looking at features | Misses positioning, pricing, traction | Use the 7-layer framework |
| Biased analysis | Loses credibility | Be honest about competitor strengths |
| Outdated data | Wrong conclusions | Date all research, refresh quarterly |
| Too many competitors | Analysis paralysis | Focus on top 3-5 direct competitors |
| No "so what" | Data without insight | End each section with implications for you |
| Feature-only comparison | Doesn't show positioning | Include pricing, reviews, positioning map |

## Related Skills

```bash
npx skills add inference-sh/skills@web-search
npx skills add inference-sh/skills@prompt-engineering
```

Browse all apps: `belt app list`

