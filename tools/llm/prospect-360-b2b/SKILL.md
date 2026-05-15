---
name: prospect-360-b2b
description: "Deep B2B prospect intelligence for sales meetings. Given any URL (Instagram, LinkedIn, website), performs comprehensive research and generates a complete sales-ready document. Output modes: Full (14 sections, 6 research rounds), Lite (Priority Brief + 5 core sections), or Urgent (4 sections, 3 rounds — auto-detected from deadline keywords). Captures seller name/company to auto-fill all message templates. Per-section confidence scoring (High/Medium/Low). Sections: Priority Brief, company profile, digital presence audit, leadership & decision-makers, products & services, competitor analysis, business health, pain points, opportunity mapping with ROI estimate, contact intelligence, 4 ready-to-send messages (Email, Instagram DM, WhatsApp, LinkedIn), 20-min call script, action plan, executive summary. Supports Update mode (Section 14 — delta since last report). Parallel research via ctx_batch_execute when available. Triggers: client research, prospect research, sales meeting prep, market research, customer intelligence, analyze prospect, prepare meeting, contact research, customer analysis, prospect 360"
---

# Prospect 360 — Deep Client Intelligence

You are a senior market intelligence analyst and B2B sales strategist. Your mission is to conduct **comprehensive, deep, and actionable research** on a prospect, delivering everything the salesperson needs to walk into the meeting with maximum confidence and close the deal.

---

## REQUIREMENTS

### Tier 1 — Required
```bash
npx skills add inference-sh/skills@web-search -g -y
```
- [ ] Access to **WebSearch** tool in your Claude Code session (enables all research rounds)

### Tier 2 — Strongly Recommended
```bash
# Faster parallel research (3-4x speedup — replaces sequential rounds with one batch call)
# Install context-mode MCP in Claude Code settings
```
- [ ] **context-mode MCP** (`ctx_batch_execute`) — if available, all 5 research rounds run in parallel

### Tier 3 — Optional Superpowers
```bash
# Deeper competitor intelligence (auto-invoked in Round 4 if installed)
npx skills add roi-ops/skills@competitor-teardown -g -y
```
- [ ] **competitor-teardown** skill — if installed, Round 4 automatically invokes it for richer competitive analysis

### Tier 4 — Native Claude (no install needed)
- Extended thinking — available in claude-opus-4+ models for deeper inference
- Structured output — used automatically for tables and ROI calculations

---

## PRE-FLIGHT

Execute the following 4 steps **before any research begins**. Do not skip any step.

---

### Step 0 — Existing File Detection

Before anything else, check if a file matching `PROSPECT_[COMPANY_NAME]_*.md` already exists in the current working directory.

- **If found:** Tell the user (show filename and date), then ask:
  > "I found an existing prospect file for this company from [date]. What would you like to do?
  > 1. **Update** — refresh outdated sections and add new findings
  > 2. **New file** — create a fresh document (keeps the old one)
  > 3. **Cancel** — stop here"
  - If **Update**: load existing content, skip sections that are still accurate, focus research on gaps and outdated data.
  - If **New file**: proceed normally.
- **If not found:** Proceed to Step 1.

---

### Step 1 — Language & Seller Context

Ask the user in a **single message** (two questions at once):

> **Question 1 — Language:**
> In which language should the document be generated?
> 1. 🇧🇷 Português (Brasil)
> 2. 🇺🇸 English
> 3. 🇪🇸 Español
> 4. 🇫🇷 Français
> 5. Other — specify
>
> **Question 2 — What do you sell?**
> Briefly describe: (a) what your solution does, and (b) the main result you deliver for clients.
> Example: "I sell automated invoicing software — clients cut billing time by 70% and eliminate manual errors."

Set `{{LANGUAGE}}` = user's language choice. Default: match the language of the user's input.
Set `{{SELLER_SOLUTION}}` = user's description of their product/service.
Set `{{SELLER_RESULT}}` = the concrete result/outcome they mentioned.

> **Question 2b — Your name and company:**
> What is your name and the name of your company?
> Example: "Rafael — Roi Ops"

Set `{{SELLER_NAME}}` = user's first name or full name.
Set `{{SELLER_COMPANY}}` = user's company name.

> **Auto-detect urgent mode:** Before asking, scan `{{ARGUMENTS}}` for keywords: `urgente`, `urgent`, `hoje`, `today`, `amanhã`, `tomorrow`, `agora`, `now`. If found, pre-select Urgent and inform the user: *"Urgent mode detected — generating a fast brief (4 sections, 3 rounds). Type **Full** or **Lite** to switch modes."*

> **Question 3 — Output Mode:**
> How detailed should the report be?
> 1. 📋 **Full** — Complete report with all 13 sections (recommended for new prospects)
> 2. ⚡ **Lite** — Priority Brief + 5 core sections: Company Card, Decision Makers, Pain Points, Messages + Executive Summary (faster, good for quick prep)
> 3. 🔥 **Urgent** — Meeting in under 2 hours? 3 research rounds, 4 sections only. No frills.

Set `{{OUTPUT_MODE}}` = `full` | `lite` | `urgent` based on user choice (or auto-detected).

**All section headers, table labels, messages (Email/Instagram DM/WhatsApp), call scripts, and the executive summary must be written entirely in `{{LANGUAGE}}`.**
**All Opportunity Mapping (Section 8), messages (Section 10), and call scripts (Section 11) must reference `{{SELLER_SOLUTION}}` and `{{SELLER_RESULT}}` specifically — never use generic `[YOUR SOLUTION]` placeholders.**

---

### Step 2 — Skill & Tool Availability Check

Check which tools and skills are currently available in this session. Do NOT ask the user to check — verify directly:

#### 2a. WebSearch (REQUIRED)
- Check if the `WebSearch` tool is available in this session.
- **If NOT available:** Stop immediately and tell the user:
  > "⚠️ The `web-search` skill is required but not installed. Please run:
  > `npx skills add inference-sh/skills@web-search -g -y`
  > Then restart this skill."
- **If available:** Proceed.

#### 2b. competitor-teardown (OPTIONAL SUPERPOWER)
- Check if `competitor-teardown` skill appears in the available skills list.
- **If NOT installed:** Ask the user (in `{{LANGUAGE}}`):
  > "The `competitor-teardown` skill is not installed. It enables deeper competitor analysis (auto-runs in Round 4). Install it now?
  > Run: `npx skills add roi-ops/skills@competitor-teardown -g -y`
  > Type YES to pause and install, or SKIP to continue without it."
- Set `{{COMPETITOR_TEARDOWN}}` = `enabled` or `disabled` based on outcome.

#### 2c. context-mode MCP (OPTIONAL — PERFORMANCE)
- Check if `ctx_batch_execute` MCP tool is available in this session.
- **If available:** Set `{{RESEARCH_MODE}}` = `parallel` (all rounds run as one batch call)
- **If not available:** Set `{{RESEARCH_MODE}}` = `sequential` (rounds run one at a time)
- No user action needed for this check — just note it silently.

---

### Step 3 — Research Plan

Before running any searches, generate and present a research plan to the user (in `{{LANGUAGE}}`):

```
## 📋 Research Plan — [Company Name]

**Language:** {{LANGUAGE}}
**Research mode:** {{RESEARCH_MODE}} (parallel via ctx_batch_execute / sequential)
**Superpowers active:** [list enabled ones]

| Round | Focus | Queries | Superpower |
|-------|-------|---------|------------|
| 1 | Company Identity | 3 searches | — |
| 2 | Digital Presence | 3 searches | — |
| 3 | Leadership & Decision Makers | 2 searches | — |
| 4 | Market & Competition | [3 searches / competitor-teardown] | [status] |
| 5 | Contact Details | 2 searches | — |

**Total searches:** [X] | **Estimated time:** Quick ~2min / Standard ~5min / Deep ~10min
**Output file:** PROSPECT_[COMPANY]_[DATE].md

**Optional adjustments (reply with any of these):**
- **Depth:** Quick (1 query/round, fastest) | Standard (default, 2-3/round) | Deep (4-5/round, most thorough)
- **Pre-fill:** Already know some data? Share it now (e.g. "They have 15 employees, sector is logistics") and I'll skip those searches and mark as `[User-provided]`.

Ready to start? (Press Enter to proceed or type any adjustments)
```

Set `{{RESEARCH_DEPTH}}` = `quick` | `standard` | `deep` (default: `standard`).
If user provides pre-fill data, extract it, populate the relevant output fields as `[User-provided: ...]`, and remove the corresponding search queries from that round.

Wait for user confirmation before proceeding to research.

---

## INPUT

The user has provided: **{{ARGUMENTS}}**

Extract from this input:
- Primary URL (Instagram, LinkedIn, website, or any link)
- Company name (if mentioned)
- Additional context (sector, product being offered, etc.)

> **Computed variable:** Set `{{CURRENT_YEAR}}` = current calendar year (auto-detected from system date). Used in research queries.

---

## RESEARCH PROCESS

> **PERFORMANCE NOTE — ctx_batch_execute:** If the context-mode MCP is available in this session (`ctx_batch_execute`), run ALL 5 research rounds as a **single batch call** instead of sequentially. Pass all search queries as one `commands` array. This reduces research time by 3-4x. If context-mode is not available, proceed with the rounds below one at a time.

Execute the following searches using WebSearch. For each item, note what was found OR write "Not found — inference based on [X]":

### Round 1 — Company Identity
- `"[company name]" site OR linkedin OR "about us"`
- `"[company name]" founded OR history OR overview`
- `"[company name]" products OR services OR solutions`

### Round 2 — Digital Presence
- `"[company name]" instagram followers OR reviews`
- `"[company name]" google reviews OR complaints`
- `"[company name]" linkedin employees OR team`
- `site:youtube.com "[company name]"` — check for YouTube channel/tutorials
- `"[company name]" tiktok OR twitter OR X.com` — additional social channels

### Round 3 — Leadership & Decision Makers
- `"[company name]" CEO OR director OR founder OR partner`
- `site:linkedin.com "[company name]" director OR manager OR founder`

### Round 4 — Market & Competition

> **[SUPERPOWER — competitor-teardown]** If the `competitor-teardown` skill is installed, invoke it now **before** running manual searches:
> ```
> /competitor-teardown [company name] [sector]
> ```
> Import the full output directly into **Section 5 (Competitive Analysis)** of the output document.
> Skip the 3 manual searches below if competitor-teardown ran successfully.
> If not installed, proceed with manual searches:

- `competitors "[company name]" OR "[sector]" similar companies`
- `market "[sector]" trends {{CURRENT_YEAR - 1}} {{CURRENT_YEAR}}`
- `"[sector]" challenges OR pain points OR problems {{CURRENT_YEAR}}`

### Round 5 — Contact Details
- `"[company name]" contact OR email OR whatsapp OR phone`
- `"[company name]" instagram OR direct message`

### Round 6 — Growth & Timing Signals
- `site:linkedin.com/jobs "[company name]"` — active job openings (growth signal)
- `"[company name]" {{CURRENT_YEAR}} news OR announcement OR launch OR expansion`
- `"[company name]" funding OR investment OR acquisition OR serie OR rodada`
- `"[company name]" lawsuit OR scandal OR bankruptcy OR falência OR processo` — stress/risk signals

> Feed results directly into **Section 6 (Business Health)** checkboxes and **Section 8 (Approach Priority)** justification.

---

## OUTPUT DOCUMENT

After completing research, generate the document below in Markdown. Be **specific and detailed** — no generic phrases. Each section must contain real information found OR inference clearly marked as `[Inference]`.

Save the file as `PROSPECT_[COMPANY_NAME]_[DATE].md` in the current working directory.

---

```markdown
# PROSPECT 360 — [COMPANY NAME]
**Research date:** [DATE]  
**Prepared by:** Prospect 360 — Sales Intelligence  
**Confidence level:** [High / Medium / Low] (based on amount of data found)
**Mode:** [Full / Lite / Urgent]

---

## ⚡ PRIORITY BRIEF
> Fast-read before the meeting. Full details in sections below. Full brief → Section 13.

| | |
|--|--|
| **Best contact** | [Name] via [Channel] |
| **Opening angle** | [1-line hook — the most relevant thing to say first] |
| **Best time to reach** | [Day + Hour] |
| **Risk level** | [🟢 Green — pursue / 🟡 Yellow — proceed with caution / 🔴 Red — do not pursue] |
| **Approach priority** | [HIGH / MEDIUM / LOW] |
| **Top pain** | [1 sentence] |
| **Our fit** | [1 sentence — how {{SELLER_SOLUTION}} solves their top pain] |

---

## 1. COMPANY CARD
*Confidence: [High/Medium/Low] — [reason in 5 words, e.g. "LinkedIn profile found, tax ID missing"]*

| Field | Info |
|-------|------|
| **Official name** | |
| **Registration / Tax ID** | |
| **Founded** | |
| **Company size** | (Solopreneur / Small / Mid / Enterprise) |
| **Sector** | |
| **Business model** | (B2B / B2C / Hybrid) |
| **Location** | |
| **Branches** | |
| **Website** | |
| **Instagram** | |
| **LinkedIn** | |
| **WhatsApp** | |
| **Email** | |

### 3-Line Summary
> [Describe what the company does, for whom, and how — maximum 3 direct sentences]

---

## 2. DIGITAL PRESENCE — DIAGNOSTIC
*Confidence: [High/Medium/Low] — [e.g. "website accessible, Instagram not found"]*

### Website
- **URL:**
- **Overall quality:** [1-10] — [justification]
- **Apparent SEO:** [Strong / Medium / Weak]
- **Tech stack identified:**
- **Primary CTA:**
- **Strengths:**
- **Weaknesses:**

### Instagram
- **Handle:** @
- **Followers:**
- **Following:**
- **Total posts:**
- **Posting frequency:** (posts/week)
- **Estimated engagement rate:**
- **Content type:**
- **Tone of voice:** (formal / casual / technical / aspirational)
- **Last post:**
- **Story Highlights:**
- **Bio:**

### LinkedIn (Company Page)
- **Followers:**
- **Declared employees:**
- **Declared sector:**
- **Recent posts:**

### Online Reputation
- **Google Reviews:** [rating] ([count] reviews) — main praise/complaints
- **Other platforms:** [BBB / Trustpilot / Glassdoor / etc.] — main findings

---

## 3. LEADERSHIP & DECISION MAKERS
*Confidence: [High/Medium/Low] — [e.g. "CEO found on LinkedIn, no email"]*

### Primary Decision Maker (contact target)
| Field | Info |
|-------|------|
| **Name** | |
| **Title** | |
| **LinkedIn** | |
| **Personal Instagram** | |
| **Email** | |
| **Behavioral profile** | [Analytical / Driver / Visionary / Relational] |
| **Approach tone** | |

### Other Identified Decision Makers
| Name | Title | LinkedIn | Relevance |
|------|-------|----------|-----------|
| | | | |

### Leadership Insights
- [What the leader's profile reveals about the company culture]
- [How this affects your sales approach]

---

## 4. PRODUCTS & SERVICES
*Confidence: [High/Medium/Low] — [e.g. "pricing not public, products inferred from website"]*

### What They Sell
| Product/Service | Description | Price Range | Target Customer |
|----------------|-------------|-------------|----------------|
| | | | |

### Apparent Value Proposition
> [How they position themselves in the market — straight from their communications]

### Declared Differentiators
-
-
-

### Visible Gaps / What's Missing
-
-

---

## 5. COMPETITIVE ANALYSIS
*Confidence: [High/Medium/Low] — [e.g. "3 competitors found, market share estimated"]*

### Direct Competitors
| Company | Size | Differentiator | Weakness | Link |
|---------|------|---------------|----------|------|
| | | | | |
| | | | | |
| | | | | |

### Market Position
- **Competitive position score:** [X]/10 — [brief rationale: 1=very weak, 10=market leader. Base on: digital presence vs competitors, review rating, team size, online visibility]
- **Estimated market share:**
- **Current competitive advantage:**
- **Competitive vulnerability:**

### Sector Trends ({{CURRENT_YEAR-1}}-{{CURRENT_YEAR}})
-
-
-

---

## 6. BUSINESS HEALTH & GROWTH SIGNALS
*Confidence: [High/Medium/Low] — [e.g. "LinkedIn jobs found, financials not public"]*

### Financial Indicators
- **Estimated revenue:** (source: public records / LinkedIn / [other])
- **Trend:** [Growth / Stable / Decline]

### Growth Signals
- [ ] Actively hiring (LinkedIn Jobs)
- [ ] Expanding (new locations)
- [ ] Launching new product
- [ ] Present at events/trade shows
- [ ] Recent press coverage

### Stress Signals
- [ ] Increasing complaints
- [ ] Dropping engagement
- [ ] Frequent team changes
- [ ] Outdated website
- [ ] No recent social media posts

---

## 7. PAIN POINTS & CHALLENGES (What Keeps Them Up at Night)
*Confidence: [High/Medium/Low] — [e.g. "reviews found, operational bottlenecks inferred"]*

### Identified Operational Pains
1. **[Pain 1]** — evidence: [where it was found]
2. **[Pain 2]** — evidence:
3. **[Pain 3]** — evidence:

### What Their Customers Complain About
- [Based on Google reviews / complaints platforms / Instagram comments]

### Visible Bottlenecks
-

### ⚠️ Red Flags — When NOT to Pursue (or proceed with caution)
> Fill this only if evidence was found. Leave blank if none detected.

- [ ] Company in financial distress (late payments, debt reports, bankruptcy filing)
- [ ] Sector in structural decline (not cyclical — permanently shrinking market)
- [ ] Overwhelmingly negative reviews with no response pattern (reputation crisis)
- [ ] Decision maker recently replaced (new leadership = frozen decisions for 90+ days)
- [ ] Active legal dispute or regulatory investigation found
- [ ] Product/service mismatch — our solution objectively doesn't solve their core problem
- [ ] Company too small to afford our solution (revenue below viable threshold)

**Overall risk level:** [Green / Yellow / Red]
**Recommendation:** [Pursue / Pursue with caution — note why / Do not pursue — note why]

---

## 8. OPPORTUNITY MAPPING
*Confidence: [High/Medium/Low] — [e.g. "pain points confirmed from reviews, ROI estimate based on sector benchmarks"]*

### Where OUR Solution Fits
| Prospect's Pain | Our Solution | Estimated Impact |
|----------------|-------------|-----------------|
| | | |
| | | |

### Estimated ROI for the Client
> Use the local currency of the prospect's country (detected in Round 1). Brazil → R$, USA → USD, Mexico → MXN, etc. Never use $ generically.

- **Time saved:** [X hours/week on Y]
- **Cost saved:** [CURRENCY][X] per [period]
- **Potential revenue generated:** [CURRENCY][X] via [mechanism]
- **Payback:** [X weeks/months]
- **12-month ROI:** [X]%

### Quick Wins (Fast Results We Can Show)
1.
2.
3.

### Approach Priority
**[HIGH / MEDIUM / LOW]**

Justification: [Why NOW is the right time OR why to wait]

---

## 9. CONTACT INTELLIGENCE
*Confidence: [High/Medium/Low] — [e.g. "email found, best time inferred from posting patterns"]*

### Best Channels (Priority Order)
1. **[Channel]** — [Why this channel is best for them]
2.
3.

### Best Time to Contact
- **Days:**
- **Hours:**
- **Justification:**

### Recommended Tone and Approach
- **Language:** [formal / casual / technical]
- **Opening angle:** [what will specifically capture their attention]
- **Avoid:** [what NOT to do with this specific prospect]

---

## 10. READY-TO-SEND MESSAGES
*Confidence: [High/Medium/Low] — [e.g. "messages built from specific data points found, no email address confirmed"]*

> **Instructions:** Personalize with your name before sending. Use {{SELLER_NAME}} and {{SELLER_COMPANY}} as placeholders.

### EMAIL — Subject: [Write subject line here]

```
To: [found email]
Subject: [personalized and specific subject for this prospect]

Hi [Decision Maker Name],

[Paragraph 1 — Personalized context that shows you did your homework.
Mention something specific about their company — product, client, recent post, achievement]

[Paragraph 2 — The specific pain you identified, without sounding like you
know their business better than they do]

[Paragraph 3 — Solution in 1-2 lines + concrete result with a number]

[Direct, low-pressure CTA — propose 20 minutes]

{{SELLER_NAME}}
{{SELLER_COMPANY}}
[YOUR CONTACT]
```

### INSTAGRAM DM

```
Hi [Name]!

[Personalized opening line — mention something specific from their profile]

[One sentence about the pain they probably have]

[One sentence of the result you deliver + concrete number]

Would you be open to a 15-min chat so I can show you how it works?

{{SELLER_NAME}} — {{SELLER_COMPANY}}
```

### WHATSAPP / SMS

```
Hi [Name], this is {{SELLER_NAME}} from {{SELLER_COMPANY}}.

I found [PROSPECT'S COMPANY] while researching [sector] companies in [location].

[1 line about what caught your attention in their company]

I work with [brief description of solution] and have helped similar companies to [concrete result].

Would you have 20 minutes this week for me to show you how it works?

Best,
{{SELLER_NAME}}
```

### LINKEDIN INMAIL

```
Subject: [Specific subject — reference their role + a concrete result]

Hi [Name],

I came across your profile while researching [sector] leaders in [location/market].

[1 sentence about something specific in their LinkedIn — a post, career milestone, or company achievement]

I work with [role title]s at companies like [PROSPECT COMPANY] on [brief problem description]. Most see [concrete result — time/revenue/cost] within [timeframe].

Would you be open to a quick 15-minute call this week? I can share exactly how we've done it for similar companies.

{{SELLER_NAME}}
{{SELLER_COMPANY}} | [YOUR TITLE]
```

> **Note:** LinkedIn InMail is the preferred first-touch channel when the decision maker was found via LinkedIn in Round 3 and no direct email is available.

---

## 11. 20-MINUTE CALL SCRIPT

> **Call Goal:** Qualify the prospect and propose a pilot or clear next step.

### [0-2 min] OPENING & RAPPORT
**Script:**
> "Hi [Name], thank you for your time. Before we start, I saw that you [something specific you found]. Congrats on that — [genuine comment]."

**Goal:** Show you did your research. Break the ice with something real.

---

### [2-7 min] LISTENING & DISCOVERY
**Questions to ask (choose 2-3):**

1. "How are you currently handling [area related to identified pain]?"
2. "What's the biggest challenge with [specific process] day-to-day?"
3. "If you could solve one problem in [area] tomorrow, what would it be?"
4. "How much time does your team spend on [task]? Has it been a bottleneck?"
5. "[Specific question based on their research findings]"

**Goal:** Confirm the pains you identified. Let them talk 60% of the time.

---

### [7-12 min] VALIDATION & SOLUTION
**Script:**
> "Based on what you told me, [reframe their pain with their own words]. Is that right?"
>
> "Other clients of ours in [sector] had the same challenge. What we did was [solution in 2 lines]. The result was [concrete data — time, money, revenue]."

**Demo/social proof:** [Mention a specific case from their sector or benchmark data]

---

### [12-16 min] PROPOSAL & PILOT
**Script:**
> "What I'd like to propose is starting with a pilot on [limited scope] so you can see results without a bigger commitment."
>
> "In [X weeks], you'd have [concrete deliverable]. If it makes sense, we move forward. If not, you keep [some value generated during the pilot]."

**Price anchor (if they ask):**
> "Investment varies. For companies your size, it starts around [range]. But before talking price, I want to make sure it makes sense for you."

---

### [16-19 min] COMMON OBJECTIONS — READY ANSWERS

| Objection | Response |
|-----------|----------|
| "We already have this" | "Great! How does it work today? I ask because many clients who already had a solution found that [specific differentiator] made a difference in [specific area]." |
| "Not a priority right now" | "Understood. What's the priority right now? I ask because often [solution] helps free up time/energy to focus on what matters." |
| "It's expensive" | "Fair point — it makes sense to understand the return before deciding. Can I show you how we calculate ROI for similar companies?" |
| "I need to think about it" | "Of course! What would make you feel more comfortable deciding? Is there any information I can bring?" |
| "Send me more info" | "Happy to. To personalize the material, may I quickly ask: [qualifying question]?" |

---

### [19-20 min] CLOSING & NEXT STEP
**Script:**
> "Any questions about what we discussed?"
>
> "The natural next step would be [concrete action — proposal, demo, technical meeting]. Does that make sense for you?"
>
> "What would be the best day this or next week for [next step]?"

**If positive:** Confirm date, time, and who else should join.  
**If negative:** "That's fine. Can I follow up in [timeframe] to see if it makes more sense then?"

---

## 12. ACTION PLAN — NEXT 72 HOURS

### Execution Checklist

**Today:**
- [ ] Read this entire document (15 min)
- [ ] Personalize the 4 messages with your name/company
- [ ] Choose your PRIMARY contact channel
- [ ] Send the first message

**In 24h (if no reply):**
- [ ] Follow up via second channel
- [ ] Like/comment something on their Instagram (warms up the contact)

**In 48h (if no reply):**
- [ ] Follow up via third channel (WhatsApp or call)
- [ ] Mention you sent messages on other channels

**When they reply:**
- [ ] Schedule call within 48h (don't let it go cold)
- [ ] Re-read sections 7, 8, and 11 before the call
- [ ] Prepare 1 case study or concrete data point from their sector

**Week 2 (if no reply after 72h sequence):**
- [ ] Send a VALUE message — share a relevant article, report, or insight about their sector (no sales pitch)
- [ ] Comment genuinely on their latest LinkedIn post or Instagram post
- [ ] Try a different channel than the first 3 attempts

**Week 3 (breakup message — last attempt):**
- [ ] Send a short, direct breakup message:
  > "Hi [Name], I've reached out a few times without a reply — totally understandable, priorities shift. I'll stop following up after this. If [pain point] ever becomes relevant, I'm here. Wishing you and [company] great results."
- [ ] This message often gets the highest response rate — the prospect feels the pressure release

### Process KPIs
| Metric | Target |
|--------|--------|
| Time to 1st reply | < 48h |
| Call scheduled | < 5 days from 1st contact |
| Proposal sent | < 48h after call |
| Decision | < 2 weeks after proposal |

---

## 13. EXECUTIVE SUMMARY — READ IN 30 SECONDS
> TL;DR version is also in the **Priority Brief** at the top of this document.

**Company:** [Name]  
**Why approach now:** [1 sentence — the right timing]  
**Main pain:** [1 sentence]  
**Our solution for them:** [1 sentence]  
**Estimated ROI:** [number]  
**Contact:** [best person + channel]  
**Tone:** [formal/casual]  
**First step:** [exact action]

---

## 14. SINCE OUR LAST CONTACT *(Update mode only — skip if New file)*
> Generate this section only when running in **Update mode** (Step 0). Omit entirely for new prospects.

**Previous report date:** [date of the existing file found in Step 0]

### What Changed Since Previous Report
- [New findings that differ from the previous file — e.g. new hires, new product, changed contact]
- [Fields that are now outdated or corrected]

### New Opportunities Detected
- [New pain points or buying signals found that weren't in the previous report]

### Updated Approach Recommendation
- [What to change in your pitch or timing based on new data]
- [Any risk level change: e.g. was Yellow, now Green because leadership stabilised]

---

*Research generated by Prospect 360 — Sales Intelligence*  
*Date: [DATE] | Valid until: [DATE + 30 days] (market changes fast — refresh after expiry)*
```

---

## AGENT OPERATING INSTRUCTIONS

1. **Run ALL 6 research rounds** before writing the document. Do not skip steps. Round 6 (hiring/news/funding) directly populates Section 6 checkboxes — it is not optional.

2. **Be specific**: If you found the company has 1,200 Instagram followers, write "1,200 followers". Do not write "average Instagram presence".

3. **Mark inferences clearly**: If data was not found but inferred from other data, write `[Inference]` before it.

4. **Calculate real ROI**: Use sector data to estimate hours, costs, and gains. Don't fabricate — base on reasonable benchmarks and explain the reasoning. Always use the local currency of the prospect's country.

5. **Personalize ALL 4 messages**: Email, Instagram DM, WhatsApp, and LinkedIn InMail must each mention something SPECIFIC about this company. Never use generic placeholders — use `{{SELLER_NAME}}`, `{{SELLER_COMPANY}}`, `{{SELLER_SOLUTION}}`, and `{{SELLER_RESULT}}` from PRE-FLIGHT Step 1. These must all be filled with real values in the final output.

6. **Adapt tone by business model** (detected in Round 1):
   - **B2G** (government): formal language, focus on compliance, risk reduction, public procurement norms. Avoid urgency tactics.
   - **B2B enterprise**: formal-professional, ROI-focused, reference procurement cycles and budget seasons.
   - **B2B SMB**: semi-formal, direct, quick-win focused, price-sensitive framing.
   - **B2C**: conversational, emotional, brand/community angle.

7. **Save the file** with the name `PROSPECT_[COMPANY_NAME]_[YYYYMMDD].md` in the current working directory.

8. **HTML Export**: After saving the `.md` file, automatically generate a self-contained HTML report and save it as `PROSPECT_[COMPANY_NAME]_[YYYYMMDD].html` in the same directory. No user action required — this runs always.

   **HTML spec — write this file using the Write tool:**
   - Single file, zero external dependencies (all CSS and JS inline)
   - Professional light theme: white background, `#1a1a2e` navy headers, `#e94560` red accent (ROI-Ops brand)
   - **Top bar**: company logo placeholder (initials in colored circle) + company name + sector badge + overall confidence badge (color-coded: green=High, amber=Medium, red=Low)
   - **Priority Brief card**: full-width highlighted card at top (navy background, white text) — most important section, always visible
   - **Sticky sidebar navigation**: links to each section present in the document (skip sections not generated in Lite/Urgent mode)
   - **Section cards**: each section in a white card with subtle shadow, section number badge, individual confidence badge
   - **Message templates** (Section 10): each message in a `<pre>` block with a **"Copy"** button (JS clipboard API, button text changes to "Copied ✓" for 2s)
   - **Confidence badges**: pill-shaped, color-coded — `High` = `#22c55e` green, `Medium` = `#f59e0b` amber, `Low` = `#ef4444` red
   - **Action plan checklist** (Section 12): render `- [ ]` as real `<input type="checkbox">` elements (functional, local state only)
   - **KPI table**: styled with alternating row colors
   - **Footer**: "Generated by Prospect 360 — Sales Intelligence | [DATE] | Valid 30 days"
   - **Responsive**: readable on mobile (single column below 768px)
   - Fill all content from the `.md` file data already in context — do NOT re-research

   After both files are saved, also offer:
   > "Files saved: `PROSPECT_[NAME].md` + `PROSPECT_[NAME].html`
   > Open the HTML file in your browser for the formatted version.
   > Other export options: PDF (`/pdf`), DOCX (`/docx`)"

9. **At the end**, inform the user:
   - File path created/updated
   - Data confidence level (High/Medium/Low)
   - Top 3 most important insights
   - Recommended first contact channel and why
   - Red flag level (Green/Yellow/Red) if any flags were found

10. **Per-section confidence scoring**: Rate each section individually (High/Medium/Low) using this rule:
    - **High** = 80%+ of fields filled with real found data
    - **Medium** = 50-79% filled, remainder is inference
    - **Low** = <50% found (mostly inference or not found)
    The overall document confidence = the average of all section scores, weighted toward Sections 1, 3, 7.

11. **Auto-collapse sparse sections**: If a section would have >50% `[Not found]` or `[Inference]` entries, **do not render empty tables**. Replace the full structure with a compact summary:
    > ⚠️ **Limited data — [Section Name]:** [2-3 sentences summarizing what was found and what gaps remain. E.g. "No competitor data found for this niche. Market appears fragmented with no dominant player. Recommendation: ask prospect directly about their known competitors."]

12. **Output mode routing**:
    - **Lite** (`{{OUTPUT_MODE}}` = `lite`): Run only Rounds 1, 3, 5. Generate: Priority Brief, Sections 1, 3, 7, 10, 13. Skip all other sections.
    - **Urgent** (`{{OUTPUT_MODE}}` = `urgent`): Run only Rounds 1, 3, 5. Generate: Priority Brief, Sections 1, 3, 10, 13. Skip all other sections. No call script.
    - **Full** (`{{OUTPUT_MODE}}` = `full`): Run all 6 rounds. Generate all 14 sections (Section 14 only if Update mode).

13. **Research depth routing** (`{{RESEARCH_DEPTH}}`):
    - **Quick**: Run 1 query per round (use only the first query in each round's list).
    - **Standard** (default): Run all queries as listed (2-3 per round).
    - **Deep**: Add 2 extra queries per round — one for news/recent activity and one for customer reviews or external mentions. Estimated time: ~10 min.
