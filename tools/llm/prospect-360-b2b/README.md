# Prospect 360 — B2B sales intelligence

End-to-end **prospect research** for B2B sellers: from a single URL (site, LinkedIn, Instagram, etc.) to a **meeting-ready** Markdown report plus optional self-contained HTML.

---

## Install — read this first

This skill is published in **[rafaelgrasa/skills](https://github.com/rafaelgrasa/skills)** (a fork of `inference-sh/skills`). Until it is merged upstream, use **`rafaelgrasa/skills`** in the commands below when you want **this** skill.

### Option A — Full inference.sh catalog + Prospect 360 (one command)

Installs **every** skill from the fork (same catalog as upstream **plus** Prospect 360):

```bash
npx skills add rafaelgrasa/skills -g -y
```

### Option B — Prospect 360 only (minimal)

Run **in this order**:

| Step | What | Command |
|------|------|---------|
| **1** | Install **Prospect 360** from this fork | `npx skills add rafaelgrasa/skills@prospect-360-b2b -g -y` |
| **2** | Install required **web-search** (official package) | `npx skills add inference-sh/skills@web-search -g -y` |
| **3** *(optional)* | Deeper competitor analysis — see `SKILL.md` Tier 3 | `npx skills add roi-ops/skills@competitor-teardown -g -y` |

**Why does step 2 use `inference-sh`?** The **web-search** skill is maintained in the official repo. That does **not** replace step 1: step 1 is what pulls **Prospect 360** from **`rafaelgrasa/skills`**.

### After upstream merge

When **prospect-360-b2b** exists on `inference-sh/skills`, step 1 may also be:

```bash
npx skills add inference-sh/skills@prospect-360-b2b -g -y
```

Until the PR is merged, keep using **`rafaelgrasa/skills@prospect-360-b2b`** for step 1 in Option B.

---

## Who it is for

- SDRs, AEs, founders, and consultants preparing **first meetings** or **outbound** with a specific company.
- Teams that want **one structured document**: company card, digital presence, stakeholders, pains, competitive context, ROI framing, and **ready-to-send** messages (email, DM, WhatsApp, LinkedIn).

## Prerequisites

| Requirement | Why |
|-------------|-----|
| **Claude Code** (or compatible agent runtime) | Skill is authored for agent execution with tools. |
| **`web-search` skill** | Open-web research (required) — included in Option A, or install via Option B step 2. |
| **`npx` / Node** | To run `npx skills add`. |
| **Optional: context-mode MCP** (`ctx_batch_execute`) | Parallel research batches when available. |
| **Optional: `competitor-teardown` skill** | Richer competitive analysis in research round 4 — see `SKILL.md` Tier 3. |

No API keys are required **by this skill itself**; `web-search` may use your inference.sh / Tavily / Exa setup as documented in that skill.

## Configure

- **WebSearch**: must be available in the session; otherwise the skill will ask you to install `web-search`.
- **context-mode**: add the MCP in Claude Code if you want parallel `ctx_batch_execute` batches.

## Usage examples

**Full dossier**

```text
Run prospect-360-b2b for https://www.example.com — they are a mid-market logistics company in Brazil.
```

**Urgent-style keywords** (*urgent*, *today*, *tomorrow*, etc. — see `SKILL.md`)

```text
Urgent: meeting tomorrow with Acme Corp, LinkedIn https://linkedin.com/company/acme
```

After install, invoke via natural language (e.g. *prospect 360*, *sales prep*). The skill asks for language, what you sell, your name/company, and Full / Lite / Urgent mode.

## Output modes

| Mode | Research | Sections |
|------|----------|----------|
| **Full** | Up to 6 rounds | Full report + call script and action plan |
| **Lite** | Subset | Priority brief + core sections |
| **Urgent** | Fast path | Minimal sections for same-day meetings |

## Limitations & good practices

- **Confidence**: each section is High/Medium/Low; refresh after ~30 days for live markets.
- **Compliance**: public information and ethical outreach only; respect GDPR/LGPD and platform terms.

## Related skills

```bash
npx skills add inference-sh/skills@web-search
npx skills add inference-sh/skills@llm-models
```

## License

Same as the parent [inference-sh/skills](https://github.com/inference-sh/skills) repository (MIT).

**Public fork (author):** https://github.com/rafaelgrasa/skills
