# Prospect 360 — B2B sales intelligence

End-to-end **prospect research** for B2B sellers: from a single URL (site, LinkedIn, Instagram, etc.) to a **meeting-ready** Markdown report plus optional self-contained HTML.

---

## Baixar esta skill — ordem dos comandos (leia isto primeiro)

Esta skill está publicada no repositório **`rafaelgrasa/skills`** (não use só `inference-sh/skills` se quiser **esta** skill antes do merge oficial).

No terminal (Claude Code, Antigravity, Cursor, etc.), execute **nesta ordem**:

| Ordem | O quê | Comando |
|-------|--------|---------|
| **1º** | Instalar **Prospect 360** a partir **do repositório do autor** | `npx skills add rafaelgrasa/skills@prospect-360-b2b -g -y` |
| **2º** | Instalar a dependência **obrigatória** `web-search` (vem do catálogo inference-sh) | `npx skills add inference-sh/skills@web-search -g -y` |
| **3º** *(opcional)* | Análise de concorrentes mais profunda — ver `SKILL.md` Tier 3 | `npx skills add roi-ops/skills@competitor-teardown -g -y` |

**Por que o 2º comando ainda diz `inference-sh`?**  
O pacote `web-search` é mantido no repo oficial `inference-sh/skills`. Isso **não** substitui o 1º comando: o 1º é que traz **a tua** skill do **`rafaelgrasa/skills`**.

**Depois do merge no upstream** (quando existir no repo oficial), o 1º passo poderá ser também:

```bash
npx skills add inference-sh/skills@prospect-360-b2b -g -y
```

*(Enquanto o PR não for aceite, use sempre `rafaelgrasa/skills@prospect-360-b2b` no passo 1.)*

---

## Who it is for

- SDRs, AEs, founders, and consultants preparing **first meetings** or **outbound** with a specific company.
- Teams that want **one structured document**: company card, digital presence, stakeholders, pains, competitive context, ROI framing, and **ready-to-send** messages (email, DM, WhatsApp, LinkedIn).

## Prerequisites

| Requirement | Why |
|-------------|-----|
| **Claude Code** (or compatible agent runtime) | Skill is authored for agent execution with tools. |
| **`web-search` skill** | Open-web research (required) — install with command **2º** above. |
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

**Urgent-style keywords** (*urgente*, *hoje*, *tomorrow*, etc. — see `SKILL.md`)

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
- **Compliance**: public information and ethical outreach only; respect LGPD/GDPR and platform terms.

## Related skills

```bash
npx skills add inference-sh/skills@web-search
npx skills add inference-sh/skills@llm-models
```

## License

Same as the parent [inference-sh/skills](https://github.com/inference-sh/skills) repository (MIT).

**Repo desta skill (fork público):** https://github.com/rafaelgrasa/skills
