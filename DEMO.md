# Jak prezentovat tento projekt někomu, kdo nezná AI ani Claude

Cílová skupina: člověk bez zkušenosti s AI nástroji — manažer, kolega z jiného oboru, student,
rodinný příslušník. Nepředpokládej žádnou znalost Claude ani programování.

---

## Co vlastně ukazuješ

Tento projekt je **vzdělávací materiál o tom, jak ovládat AI asistenta pro vývojáře**.
Neukazuješ hotovou aplikaci — ukazuješ, jak se takový nástroj konfiguruje a nastavuje,
aby byl bezpečný a použitelný v týmu.

Analogie pro neznalé: je to jako naučit někoho, jak nastavit nového zaměstnance —
co smí dělat, co nesmí, a jakým stylem má pracovat.

---

## Doporučená struktura prezentace (30–45 minut)

### Krok 1 — Úvod: Co je Claude Code? (5 minut)

Začni bez technického žargonu:

> „Představ si, že máš vedle sebe asistenta, který umí číst a psát kód.
> Řekneš mu česky: ‚Oprav tuhle chybu' nebo ‚Přidej do formuláře pole pro e-mail'
> — a on to udělá. Nejenom navrhne řešení, ale rovnou soubory upraví.
> Jmenuje se Claude a je to produkt firmy Anthropic."

> „Claude Code je verze tohoto asistenta, která běží přímo v terminálu vývojáře
> a může pracovat s celým projektem — číst soubory, spouštět testy, commitovat do gitu."

Pokud máš Claude Code nainstalovaný, ukaž terminál a napiš jednoduchou instrukci.
Nech posluchače vidět, jak Claude odpovídá. Nemusí to být složité — stačí `claude` a otázka.

---

### Krok 2 — Proč nestačí jen „zapnout AI"? (5 minut)

> „Každý tým a každý projekt je jiný. Vývojář na e-shopu potřebuje jiná pravidla
> než výzkumný tým nebo junior programátor. Proto Claude Code umožňuje nastavit,
> jak se má chovat — a ta nastavení sdílet s celým týmem přes git."

Tady otevři soubor `.claude/settings.json` (v editoru nebo terminálu) a ukaž ho na obrazovce.
Není potřeba vysvětlovat JSON — stačí ukázat tři části:

1. **Model** — jaký „mozek" Claude použije
2. **Co smí dělat bez ptaní** (`allow`)
3. **Co nesmí nikdy** (`deny`)

---

### Krok 3 — Ukázka: Trust boundary (10 minut)

Tohle je jádro prezentace. Ukaž konkrétní bezpečnostní pravidla ze souboru:

```
POVOLENO (bez ptaní):
- číst soubory
- upravovat soubory
- spouštět testy (php tests/run.php)
- pracovat s Dockerem
- zobrazit git status

ZAKÁZÁNO (nikdy):
- rekurzivní mazání (rm -rf)
- editace .env souborů (hesla, API klíče)
- git push (nahrání na server)
```

Vysvětli to lidsky:

> „Představ si, že najmeš brigádníka. Řekneš mu: ‚Smíš uklízet kancelář a připravovat
> kávu, ale klíče od trezoru nedostaneš a ven z budovy sám nepůjdeš.'
> Tohle jsou přesně taková pravidla — ale pro AI asistenta."

> „Když nový člen týmu naklonuje repozitář, tato pravidla se aplikují automaticky.
> Nemusí nic nastavovat ručně."

---

### Krok 4 — Různé modely = různý výkon a cena (5 minut)

Ukaž tabulku modelů z README.md. Přelož ji do normálních slov:

> „Claude existuje ve více verzích — říkáme jim modely. Liší se výkonem a cenou,
> podobně jako auta: Haiku je jako Fabia — rychlá, levná, na každodenní jízdu.
> Sonnet je jako Octavia — výkon i cena v rovnováze. Opus je jako sportovní auto —
> pomalé na startování, ale zvládne to, co ostatní ne."

| Model | Přirovnání | Kdy ho použiješ |
|---|---|---|
| Haiku 4.5 | Fabia | Čtení souborů, drobné opravy |
| Sonnet 4.6 | Octavia | Každodenní práce — default |
| Opus 4.8 | Sportovní auto | Složitý refaktor, architektura |
| Fable 5 | Sportovní auto+ | Nejtěžší dlouhé úlohy |
| opusplan | Opus + Sonnet dohromady | Plánuje Opus, píše Sonnet |

> „Model si vývojář vybírá podle toho, co dělá. A může ho změnit jednou větou
> přímo v terminálu, aniž by ztratil kontext rozhovoru."

---

### Krok 5 — CLAUDE.md: paměť projektu (5 minut)

Otevři `CLAUDE.md` tohoto projektu a ukaž ho.

> „Tenhle soubor je jako ‚nástupní příručka' pro AI. Říká mu:
> jak se projekt jmenuje, jakou technologii používá, co nesmí dělat,
> jak pojmenovávat třídy — prostě vše, co by musel vědět nový člen týmu."

> „Rozdíl od `settings.json` je v tom, že `settings.json` jsou technická pravidla —
> stroj je buď splní nebo nesplní. `CLAUDE.md` je text, který Claude čte jako instrukci —
> je to kontext, ne seznam zákazů."

Praktická ukázka: ukaž, že `CLAUDE.md` obsahuje větu „Komunikovat s uživatelem vždy v češtině."
A ukázat, že Claude skutečně odpovídá česky.

---

### Krok 6 — Varianta pro opatrné týmy: plan mode (5 minut)

Ukaž soubor `examples/settings.opusplan.json`:

> „Toto je konfigurace pro týmy, které chtějí, aby AI nejdřív navrhla co udělá,
> a teprve po schválení to provedla. Vývojář vidí plán, odsouhlasí ho nebo opraví —
> a pak Claude teprve začne měnit soubory."

> „Je to jako když architekt nejdřív ukáže výkres a teprve po podpisu klienta
> začne stavět. Žádná překvapení."

---

### Krok 7 — Shrnutí a otázky (5 minut)

Uzavři třemi větami:

> „Claude Code je AI asistent, který pracuje přímo v projektu. Konfigurací v JSON souboru
> určíš, co smí a co nesmí — a tato pravidla sdílíš s celým týmem.
> CLAUDE.md mu řekne kontext projektu, models.json vybere ‚mozek' podle náročnosti úkolu."

Nech prostor na otázky. Časté:

- **„Nahradí to vývojáře?"** — Ne. Vývojář stále rozhoduje, kontroluje a opravuje.
  Claude zrychluje rutinní práci a pomáhá s návrhy.
- **„Je to bezpečné?"** — Záleží na konfiguraci. Přesně proto existují `permissions.deny`
  a `defaultMode: plan`.
- **„Kolik to stojí?"** — Platí se za tokeny (množství textu zpracovaného AI).
  Haiku je nejlevnější, Opus nejdražší. Sonnet je dobrý kompromis pro každodenní práci.

---

## Co mít připravené před prezentací

- [ ] Terminál s Claude Code nainstalovaným a přihlášeným
- [ ] Editor nebo prohlížeč souborů s otevřeným `.claude/settings.json` a `CLAUDE.md`
- [ ] Připravená jednoduchá ukázka — např. „zeptej se Clauda česky na cokoliv"
- [ ] Tabulku modelů z README.md zobrazenou na obrazovce

---

## Co raději neukazovat nezkušenému publiku

- Příkazy `git` — odvedou pozornost od tématu
- Chybové hlášky z terminálu — zbytečně zastrašují
- Detailní JSON syntaxi — není důležitá pro pochopení konceptu

---

## Délka

- **10 minut:** kroky 1, 2, 3 (co je Claude, proč nastavení, trust boundary)
- **20 minut:** kroky 1–5 (přidej modely a CLAUDE.md)
- **30–45 minut:** celá prezentace s otázkami a živou ukázkou
