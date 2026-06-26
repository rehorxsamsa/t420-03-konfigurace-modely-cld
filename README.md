# Díl 03 — Konfigurace, modely a klíčové soubory

> **Co se naučíš:** `claude config list/set/add`, výběr modelu (`/model`, `--model`, `settings.json`),
> `/config`, a všechny **klíčové soubory** (`CLAUDE.md`, `settings.json` v user vs. project scope).

---

## 1. Model se nastavuje třemi způsoby:

### A) Pro jednu session — flag `--model`
```bash
claude --model claude-sonnet-4-6
claude --model claude-opus-4-8
claude --model opusplan          # hybrid: Opus přemýšlí, Sonnet píše
```

### B) Během session — `/model`
```
> /model claude-opus-4-8
```
Nebo jen `/model` a vyber z nabídky. Přepnutí mid-session bez ztráty kontextu: `Option+P` (Mac) / `Alt+P` (Linux/Windows).

### C) Pro projekt natrvalo — `settings.json`
```json
{
  "model": "claude-sonnet-4-6"
}
```
(Viz `.claude/settings.json` v tomto dílu.)

---

## 2. aktuální modely 2026

Cheatsheet uvádí Opus 4.1 / Sonnet 4 / Haiku 3.5. To jsou **staré názvy**. Aktuální lineup (červen 2026):

| Tier | Model | API string | Kdy použít |
|---|---|---|---|
| **Fable** | Fable 5 | `claude-fable-5` | Nově nad Opusem; nejnáročnější dlouhé úlohy |
| **Opus** | Opus 4.8 | `claude-opus-4-8` | Maximální reasoning — složitý refaktor, architektura, debugging |
| **Sonnet** | Sonnet 4.6 | `claude-sonnet-4-6` | **Default pro většinu práce** — skvělý poměr cena/výkon |
| **Haiku** | Haiku 4.5 | `claude-haiku-4-5-20251001` | Rychlé/levné — čtení souborů, drobné edity, routování |
| alias | opusplan | `opusplan` | Opus pro plánování, Sonnet pro psaní kódu |

> **Doporučení:** Default `claude-sonnet-4-6`. Na náš největší refaktor v dílu 06 sáhni po `claude-opus-4-8` nebo `opusplan`. Claude Code navíc umí **smart model switching** — jednoduché úkoly sám pošle na Haiku.

**Pozn.:** Vždy používej plný verzovaný string (`claude-sonnet-4-6`), ne generický `claude-sonnet` — ten by mohl časem ukázat na jiný model.

---

## 3. `claude config` — list / set / add

```bash
claude config list                    # vypíše všechna nastavení
claude config set <klíč> <hodnota>    # změní hodnotu
claude config add <klíč> <hodnota>    # přidá do pole (array setting)
```

**Praktické příklady:**
```bash
claude config list                    # co všechno mám nastaveno
```

> ⚠️ Konkrétní klíče (např. téma) se mezi verzemi měnily, a `model` se přes `config set` **nenastavuje** (viz výše). Bezpečnější a přehlednější cesta je dnes `/config` v session nebo přímo editace `settings.json`
— pokud ti to v tvojí verzi nezabere, nastav téma přes `/config`.

### `/config` — interaktivně
```
> /config
```
Zobrazí a umožní měnit konfiguraci přímo v session. Pro většinu nastavení pohodlnější než pamatovat si klíče.

---

## 4. Klíčové soubory (z cheatsheetu „KEY FILE LOCATIONS")

| Soubor | Scope | Co tam patří |
|---|---|---|
| `~/.claude/settings.json` | **User global** | Tvoje osobní nastavení napříč všemi projekty |
| `.claude/settings.json` | **Project** | Nastavení projektu (sdílíš přes git s týmem) |
| `~/.claude/skills/` | User global | Tvoje skilly napříč projekty (dříve `commands/`) |
| `.claude/skills/` | Project | Skilly projektu (sdílíš s týmem) |
| `CLAUDE.md` | Project | Paměť projektu — načítá se na startu, zůstává v kontextu |

> **Klíčové pravidlo merge:** Project nastavení se **slučuje** s user nastavením a při konfliktu **vyhrává project**. Takže si můžeš mít osobní defaulty v `~/.claude/`, a konkrétní projekt je přebije přes `.claude/`.

### Ukázkový `settings.json` (v tomto dílu)

Mrkni na `.claude/settings.json`:
```json
{
  "model": "claude-sonnet-4-6",
  "permissions": {
    "allow": [
      "Read", "Edit",
      "Bash(php tests/run.php:*)",
      "Bash(docker compose *:*)",
      "Bash(git status:*)"
    ],
    "deny": [
      "Bash(rm -rf *:*)",
      "Edit(.env*)",
      "Bash(git push *:*)"
    ]
  }
}
```

Rozbor:
- `model` — default model projektu
- `permissions.allow` — co Claude smí bez ptaní (pustit testy, docker, číst git status)
- `permissions.deny` — co nesmí nikdy (smazat rekurzivně, editovat `.env`, pushnout)

To je **trust boundary pro tým**: nový člen klonuje repo a má rovnou bezpečné defaulty.

A varianta pro velké refaktory — `examples/settings.opusplan.json`:
```json
{
  "model": "opusplan",
  "permissions": {
    "defaultMode": "plan"
  }
}
```
`defaultMode: plan` znamená, že Claude v tomhle projektu **defaultně plánuje** (než cokoliv udělá) — ideální pro citlivou codebase.

---

## 5. CLAUDE.md vs. settings.json — kdy co

Častý zmatek. Jednoduše:

- **`CLAUDE.md`** = *co Claude potřebuje vědět* o projektu (stack, konvence, příkazy, „co nedělat"). Je to **text/prompt**, čte ho Claude jako instrukce.
- **`settings.json`** = *jak se Claude Code chová* (model, oprávnění, env proměnné, hooky). Je to **konfigurace**, čte ji nástroj.

Příklad: „business logika patří do Service" → `CLAUDE.md`. „nesmíš pustit `git push`" → `settings.json` (`permissions.deny`).

---

## Shrnutí dílu 03

Víš, že model **nenastavuješ** přes `claude config set model`, ale přes `--model`, `/model`, nebo `settings.json`. Znáš aktuální modely 2026 (Opus 4.8 / Sonnet 4.6 / Haiku 4.5 / Fable 5 / opusplan) a kdy který. Rozumíš scope souborů (user `~/.claude/` vs. project `.claude/`, project při konfliktu vyhrává) a rozdílu mezi `CLAUDE.md` (instrukce) a `settings.json` (konfigurace + permissions).

---

## ✅ Test dílu 03

**1. `claude config set model claude-opus-4-1-20250805`. Co je na tom špatně a jak se model nastavuje správně?**

<details><summary>Odpověď</summary>

Špatně je obojí: takhle se model **už nenastavuje** a Opus 4.1 je superseded. Správně: flag `claude --model <model>` pro session, `/model` během session, nebo klíč `"model"` v `settings.json` pro projekt natrvalo.
</details>

**2. Vyjmenuj aktuální modely 2026 a jejich typické použití.**

<details><summary>Odpověď</summary>

**Opus 4.8** (`claude-opus-4-8`) — max reasoning, složitý refaktor/architektura. **Sonnet 4.6** (`claude-sonnet-4-6`) — default pro většinu práce, nejlepší cena/výkon. **Haiku 4.5** (`claude-haiku-4-5-20251001`) — rychlé/levné, čtení a drobné edity. **Fable 5** (`claude-fable-5`) — nový tier nad Opusem. **opusplan** — hybrid: Opus plánuje, Sonnet píše.
</details>

**3. Proč používat `claude-sonnet-4-6` a ne `claude-sonnet` v produkční konfiguraci?**

<details><summary>Odpověď</summary>

Generický string bez verze může časem ukázat na jiný model, až Anthropic změní defaulty — tím by se tiše změnilo chování. Plný verzovaný string to fixuje.
</details>

**4. Máš `model: opus-4-8` v `~/.claude/settings.json` a `model: claude-sonnet-4-6` v `.claude/settings.json` projektu. Který se použije?**

<details><summary>Odpověď</summary>

**Project vyhrává** — `claude-sonnet-4-6`. Project nastavení se slučuje s user nastavením a při konfliktu má přednost project.
</details>

**5. Pravidlo „business logika patří do Service" a zákaz „nesmíš pustit git push" — kam každé z nich patří?**

<details><summary>Odpověď</summary>

„Business logika do Service" = instrukce → **`CLAUDE.md`**. Zákaz `git push` = chování/oprávnění → **`settings.json`** (`permissions.deny`).
</details>

**6. Co dělá `"defaultMode": "plan"` v settings.json?**

<details><summary>Odpověď</summary>

Claude v daném projektu **defaultně plánuje** (navrhne plán a čeká na schválení) místo toho, aby rovnou editoval. Vhodné pro citlivou codebase.
</details>

**7. Co znamenají položky v `permissions.allow` jako `Bash(php tests/run.php:*)`?**

<details><summary>Odpověď</summary>

Whitelist příkazů, které Claude smí spustit **bez ptaní**. `Bash(php tests/run.php:*)` povolí pouštět testy s libovolnými argumenty. `permissions.deny` naopak zakazuje (např. `Bash(rm -rf *:*)`). Dohromady tvoří trust boundary, kterou sdílíš s týmem přes git.
</details>

→ Pokračuj na [Díl 04 — MCP servery](../t420-04-mcp-servery-cld/README.md)
