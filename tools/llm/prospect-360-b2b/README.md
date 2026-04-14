# Prospect 360 — B2B sales intelligence

End-to-end **prospect research** for B2B sellers: from a single URL (site, LinkedIn, Instagram, etc.) to a **meeting-ready** Markdown report plus optional self-contained HTML.

## Who it is for

- SDRs, AEs, founders, and consultants preparing **first meetings** or **outbound** with a specific company.
- Teams that want **one structured document**: company card, digital presence, stakeholders, pains, competitive context, ROI framing, and **ready-to-send** messages (email, DM, WhatsApp, LinkedIn).

## Prerequisites

| Requirement | Why |
|-------------|-----|
| **Claude Code** (or compatible agent runtime) | Skill is authored for agent execution with tools. |
| **`web-search` skill** | Open-web research (required). |
| **`npx` / Node** | To install skills from this repository. |
| **Optional: context-mode MCP** (`ctx_batch_execute`) | Parallel research batches when available. |
| **Optional: `competitor-teardown` skill** | Richer competitive analysis in research round 4 (`roi-ops/skills` or `inference-sh/skills` — see `SKILL.md`). |

No API keys are required **by this skill itself**; `web-search` may use your inference.sh / Tavily / Exa setup as documented in that skill.

## Install

**A partir deste repositório (fork público):**

```bash
npx skills add rafaelgrasa/skills@prospect-360-b2b -g -y
```

**Quando existir no upstream `inference-sh/skills` (após merge do PR):**

```bash
npx skills add inference-sh/skills@prospect-360-b2b -g -y
```

Dependências:

```bash
npx skills add inference-sh/skills@web-search -g -y
# opcional — ver comando exato no SKILL.md (Tier 3)
npx skills add roi-ops/skills@competitor-teardown -g -y
```

## Configure

- **WebSearch**: deve estar disponível na sessão; caso contrário a skill pede a instalação do `web-search`.
- **context-mode**: configure o MCP no Claude Code se quiser batches paralelos.

## Usage examples

**Dossiê completo**

```text
Run prospect-360-b2b for https://www.example.com — they are a mid-market logistics company in Brazil.
```

**Modo urgente** (palavras-chave como *urgente*, *hoje*, *tomorrow* podem ser detetadas — ver `SKILL.md`)

```text
Urgent: meeting tomorrow with Acme Corp, LinkedIn https://linkedin.com/company/acme
```

Depois de instalar, invoca pela linguagem natural (ex.: *prospect 360*, *sales prep*). A skill pergunta idioma, o que vendes, nome/empresa e modo Full / Lite / Urgent.

## Output modes

| Mode | Research | Sections |
|------|----------|----------|
| **Full** | Até 6 rondas | Relatório completo + script de chamada e plano de ação |
| **Lite** | Subconjunto | Priority brief + secções centrais |
| **Urgent** | Rápido | Secções mínimas para reunião no mesmo dia |

## Limitations & good practices

- **Confiança**: cada secção tem High/Medium/Low; mercados mudam — atualizar após ~30 dias.
- **Compliance**: só informação pública e abordagem ética; respeitar LGPD/GDPR e termos das plataformas.

## Related skills

```bash
npx skills add inference-sh/skills@web-search
npx skills add inference-sh/skills@llm-models
```

## License

Same as the parent [inference-sh/skills](https://github.com/inference-sh/skills) repository (MIT).
