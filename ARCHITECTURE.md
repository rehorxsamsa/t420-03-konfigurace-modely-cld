# Architektura projektu — t420-03-konfigurace-modely-cld

## Kontext

Referenční konfigurace pro díl 03 série **t420**. Neobsahuje spustitelný kód ani testy — jde čistě o demonstraci konfigurační vrstvy Claude Code: scopování `settings.json`, CLAUDE.md a permissions model.

---

## Struktura souborů

```
t420-03-konfigurace-modely-cld/
├── CLAUDE.md                        ← prompt context pro Claude (instrukce projektu)
├── README.md                        ← výukový obsah série
├── ARCHITECTURE.md                  ← tento soubor
├── .claude/
│   ├── settings.json                ← project scope — sdílený přes git, platí pro celý tým
│   └── settings.local.json          ← local scope — gitignored, rozšíření pro autora
└── examples/
    └── settings.opusplan.json       ← config varianta pro velké refaktory
```

---

## Konfigurační vrstva

Claude Code používá třívrstvý merge model s přesně definovanou prioritou:

```
user  (~/.claude/settings.json)
  ↑ přebito
project  (.claude/settings.json)
  ↑ přebito
local  (.claude/settings.local.json)
```

Každá vrstva override předchozí. `deny` pravidla jsou aditivní napříč vrstvami — nelze je vyšší vrstvou zrušit (allowlist má project scope, local ho může rozšířit, ne oslabit).

### `.claude/settings.json` — project scope

```json
{
  "model": "claude-sonnet-4-6",
  "env": { "CLAUDE_CODE_MAX_OUTPUT_TOKENS": "32000" },
  "permissions": {
    "allow": ["Read", "Edit", "Bash(php tests/run.php:*)", "Bash(docker compose *:*)"],
    "deny":  ["Bash(rm -rf *:*)", "Edit(.env*)", "Bash(git push *:*)"]
  }
}
```

Commituje se do repozitáře — nový člen týmu dostane bezpečné defaulty bez nutnosti manuální konfigurace. `deny` pravidla definují trust boundary: destruktivní operace a modifikace `.env` souborů jsou blokované i pro Claude.

### `.claude/settings.local.json` — local scope

Rozšiřuje project permissions o operace, které tým nesdílí. Typicky git workflow (init, add, commit) pro autora projektu. Gitignored — nevstupuje do CI ani code review.

---

## Modely (stav červen 2026)

| Tier | API string | Doporučené použití |
|---|---|---|
| Fable 5 | `claude-fable-5` | Extrémně náročné long-context úlohy |
| Opus 4.8 | `claude-opus-4-8` | Deep reasoning — architektura, složitý refaktor |
| **Sonnet 4.6** | `claude-sonnet-4-6` | **Výchozí** — optimální poměr latence/cena/kvalita |
| Haiku 4.5 | `claude-haiku-4-5-20251001` | Triviální operace, routování, rychlé čtení |
| opusplan | `opusplan` | Hybridní režim: Opus plánuje, Sonnet provádí |

Priorita přepisu modelu (sestupně): `--model` CLI flag → `/model` v session → `"model"` v `settings.json`.

---

## CLAUDE.md vs. settings.json — typová hranice

| Soubor | Typ artefaktu | Konzument | Charakter obsahu |
|---|---|---|---|
| `CLAUDE.md` | Přirozený jazyk / prompt | Claude (LLM) | Architektonické konvence, doménová pravidla |
| `settings.json` | Strukturovaná konfigurace | Claude Code (nástroj) | Permissions, model, env vars |

Záměna těchto souborů je typická chyba — bezpečnostní pravidlo v `CLAUDE.md` nemá žádný enforcement effect; musí být v `settings.json` jako `deny` pravidlo.

---

## Varianta `examples/settings.opusplan.json`

```json
{
  "model": "opusplan",
  "permissions": { "defaultMode": "plan" }
}
```

`defaultMode: plan` vynucuje schvalovací krok před každou změnou — Claude navrhne plán a čeká na explicitní souhlas. Vhodné pro citlivé codebases nebo týmy s mandatory review procesem. Kombinace s `opusplan` modelem dává maximální kontrolu za přijatelnou cenu (Opus generuje plán, Sonnet píše kód).

---

## Návaznost v sérii

| Předchozí | Tento díl | Následující |
|---|---|---|
| t420-02 | **t420-03** — konfigurace, modely, permissions model | t420-04 — MCP servery |
