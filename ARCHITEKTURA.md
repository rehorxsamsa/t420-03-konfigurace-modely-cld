# Architektura projektu — t420-03-konfigurace-modely-cld

## Účel

Vzdělávací díl 03 série **t420** (PHP OOP s Claude Code). Jde o referenční dokumentaci a ukázkové konfigurační soubory — **neobsahuje spustitelný kód ani testy**. Cílem je naučit správnou konfiguraci Claude Code: volbu modelu, strukturu `settings.json`, CLAUDE.md a permissions.

---

## Struktura souborů

```
t420-03-konfigurace-modely-cld/
├── CLAUDE.md                        ← instrukce pro Claude Code (paměť projektu)
├── README.md                        ← hlavní výukový obsah (teorie + testové otázky)
├── ARCHITEKTURA.md                  ← tento soubor
├── .claude/
│   ├── settings.json                ← produkční konfigurace projektu (sdílená přes git)
│   └── settings.local.json          ← lokální rozšíření (git ignorováno, jen pro autora)
└── examples/
    └── settings.opusplan.json       ← varianta pro velké refaktory (opusplan + plan mode)
```

---

## Konfigurační vrstva

Projekt demonstruje dvouvrstvý konfigurační model Claude Code:

### `.claude/settings.json` — project scope (sdílený)

```json
{
  "model": "claude-sonnet-4-6",
  "env": { "CLAUDE_CODE_MAX_OUTPUT_TOKENS": "32000" },
  "permissions": {
    "allow": ["Read", "Edit", "Bash(php tests/run.php:*)", "Bash(docker compose *:*)", ...],
    "deny":  ["Bash(rm -rf *:*)", "Edit(.env*)", "Bash(git push *:*)"]
  }
}
```

Klonuje ho celý tým — nový člen má rovnou bezpečné defaulty (trust boundary).

### `.claude/settings.local.json` — local scope (jen autor)

Rozšiřuje project settings o práva, která autor potřebuje lokálně a nechce sdílet (git operace: `git init`, `git add`, `git commit`).

### Pravidlo merge

```
user (~/.claude/settings.json)  ←  project (.claude/settings.json)  ←  local (.claude/settings.local.json)
```

Každá vrstva přebíjí tu předchozí. Project vyhrává nad user; local vyhrává nad project.

---

## Modely (stav červen 2026)

| Tier | API string | Použití |
|---|---|---|
| Fable 5 | `claude-fable-5` | Nový tier nad Opusem; nejnáročnější dlouhé úlohy |
| Opus 4.8 | `claude-opus-4-8` | Maximální reasoning — složitý refaktor, architektura |
| **Sonnet 4.6** | `claude-sonnet-4-6` | **Default** — nejlepší poměr cena/výkon |
| Haiku 4.5 | `claude-haiku-4-5-20251001` | Rychlé/levné — čtení, drobné edity, routování |
| opusplan | `opusplan` | Hybrid: Opus plánuje, Sonnet píše (levněji než čistý Opus) |

Model lze nastavit třemi způsoby (sestupná priorita): `--model` flag → `/model` příkaz v session → `"model"` v `settings.json`.

---

## Klíčový rozdíl: CLAUDE.md vs. settings.json

| Soubor | Co je to | Kdo ho čte | Příklad obsahu |
|---|---|---|---|
| `CLAUDE.md` | Text / prompt | **Claude** (jako instrukce) | „Business logika patří do Service" |
| `settings.json` | JSON konfigurace | **Claude Code** (nástroj) | `permissions.deny: ["Bash(git push *:*)"]` |

---

## Varianta pro velké refaktory (`examples/settings.opusplan.json`)

```json
{
  "model": "opusplan",
  "permissions": { "defaultMode": "plan" }
}
```

`defaultMode: plan` — Claude nejdříve navrhne plán a čeká na schválení, než cokoliv změní. Vhodné pro citlivou codebase nebo týmové projekty s review procesem.

---

## Návaznost v sérii

| Předchozí | Tento díl | Následující |
|---|---|---|
| t420-02 | **t420-03** — konfigurace, modely, klíčové soubory | t420-04 — MCP servery |
