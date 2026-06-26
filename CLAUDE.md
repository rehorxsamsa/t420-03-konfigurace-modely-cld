# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Účel projektu

Vzdělávací díl 03 série t420 — pokrývá konfiguraci Claude Code: výběr modelu, `settings.json`, CLAUDE.md a `permissions`. Neobsahuje spustitelný kód ani testy — jde o referenční dokumentaci a ukázkové soubory.

## Struktura

- `README.md` — hlavní výukový obsah (teorii čti zde, ne v CLAUDE.md)
- `.claude/settings.json` — ukázka produkční konfigurace projektu (model + permissions)
- `examples/settings.opusplan.json` — varianta pro velké refaktory (`opusplan` + `defaultMode: plan`)

## Klíčová pravidla settings.json

Produkční konfigurace v `.claude/settings.json` zakazuje `git push`, rekurzivní mazání a editaci `.env` souborů. Při úpravách tato pravidla zachovej — jsou demonstrací trust boundary pro týmové projekty.

## Modely 2026 (pro aktualizaci README)

| Alias | API string | Použití |
|---|---|---|
| Fable 5 | `claude-fable-5` | Nad Opusem, nejnáročnější úlohy |
| Opus 4.8 | `claude-opus-4-8` | Reasoning, refaktor, architektura |
| Sonnet 4.6 | `claude-sonnet-4-6` | Default — nejlepší cena/výkon |
| Haiku 4.5 | `claude-haiku-4-5-20251001` | Rychlé čtení, drobné edity |
| opusplan | `opusplan` | Opus plánuje, Sonnet píše |

Vždy používej plný verzovaný string (např. `claude-sonnet-4-6`), ne generický alias bez verze.

## Návaznost v sérii

Předchozí: `t420-02-*` | Následující: `t420-04-mcp-servery-cld`
