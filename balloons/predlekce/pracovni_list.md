# Pracovní list: Balloons, předlekce: stránka hry a formulář v Bootstrapu

**Jméno:** ______________________  **Třída:** ______  **Datum:** ____________

Dnes postavíme **stránku** pro hru Balloons: hlavičku, formulář s nastavením a hrací plochu.
Žádný JavaScript, jen HTML a třídy Bootstrapu. Celé to musí fungovat na mobilu i na monitoru.
Příští hodinu stránku rozpohybujeme.

Po ruce měj [tahak_layout_formular.md](tahak_layout_formular.md). Hotové úkoly si odškrtávej `[x]`, odpovědi piš přímo sem.

Takhle bude stránka vypadat:

```
MOBIL (pod 1024 px)                 NOTEBOOK (od 1024 px, LG)
┌──────────────────────┐            ┌──────────────────────────────────────────┐
│ Balloons             │            │ Balloons                                 │
├──────────────────────┤            ├──────────────────────────────────────────┤
│ ┌──────────────────┐ │            │ ┌────────────┐  [Hráč: ?] [Prasknuto 0/0]│
│ │ Nastavení hry    │ │            │ │ Nastavení  │  ┌──────────────────────┐ │
│ │ Jméno  [_______] │ │            │ │ Jméno [__] │  │                      │ │
│ │ Počet [__] Vzhled│ │            │ │ Počet [__] │  │    hrací plocha      │ │
│ │ ◉ Zvuk           │ │            │ │ Vzhled[__] │  │                      │ │
│ │ [    Start     ] │ │            │ │ ◉ Zvuk     │  │                      │ │
│ └──────────────────┘ │            │ │ [ Start  ] │  │                      │ │
│ [Hráč: ?] [0 / 0]    │            │ └────────────┘  └──────────────────────┘ │
│ ┌──────────────────┐ │            │       4/12                 8/12          │
│ │  hrací plocha    │ │            └──────────────────────────────────────────┘
│ └──────────────────┘ │
└──────────────────────┘
```

---

### Úkol P1: příprava (3 min)
- [ ] Ve svém repozitáři vytvoř složku `balloons` a ulož do ní [kostra.html](kostra.html) jako `balloons/index.html`.
- [ ] V kořeni repozitáře spusť `py -3 -m http.server` a otevři <http://localhost:8000/balloons/>.
- [ ] **F12** → režim zařízení **Ctrl + Shift + M**. Vpravo dole je černý štítek s breakpointem (známe z Boxes).
- [ ] Prohlédni si v `index.html` `<head>`: je tam Bootstrap a **hotové vlastní CSS** pro `.plocha` a `.balonek`.
      Dnes ho neměníme, píšeme jen HTML do `<body>`.

### Úkol P2: rozmysli si layout (3 min)
Podívej se na nákres nahoře a doplň:

| | Mobil (< 1024 px) | Od LG (≥ 1024 px) |
|---|---|---|
| Formulář a hra jsou | pod sebou / vedle sebe | pod sebou / vedle sebe |
| Formulář zabírá dílů z 12 | | |
| Hra zabírá dílů z 12 | | |

### Úkol P3: hlavička a kontejner (3 min)
Do sekce **HLAVIČKA** vlož lištu a pod ni hlavní obsah stránky:

```html
<nav class="navbar bg-1">
    <div class="container">
        <span class="navbar-brand fw-bold">🎈 Balloons</span>
    </div>
</nav>

<main class="container py-8">
    <!-- sem přijde řádek se dvěma sloupci -->
</main>
```

- [ ] Proč je `container` v liště **i** v `<main>`? Zkus ho v liště smazat a porovnej, kde začíná nápis. ______________

### Úkol P4: dva sloupce, srdce responzivity (10 min)
Do `<main>` vlož **řádek** se dvěma sloupci. Zatím do nich dej jen dočasný text s šedým pozadím, ať je vidíš:

```html
<div class="row g-5">
    <aside class="col-12 lg:col-4">
        <div class="bg-1 p-5">FORMULÁŘ</div>
    </aside>
    <section class="col-12 lg:col-8">
        <div class="bg-1 p-5">HRA</div>
    </section>
</div>
```

- [ ] Otestuj v režimu zařízení a vyplň:

| Šířka okna | 375 px | 800 px | 1100 px | 1400 px |
|---|---|---|---|---|
| Štítek ukazuje | | | | |
| Sloupce jsou pod sebou / vedle sebe | | | | |

**Otázka P4a:** Přečti nahlas třídy `col-12 lg:col-4`. Co znamená část bez prefixu a co část s `lg:`?

> ______________________________________________________________

**Otázka P4b:** Kdybys napsal(a) `md:col-4` a `md:col-8`, od kolika px by byly sloupce vedle sebe? ______
Proč je to pro naši hru horší? (Zkus to na 800 px.)

> ______________________________________________________________

- [ ] ⭐ Zkus `lg:col-3` + `lg:col-9`. Sedí součet 12? Co se stane s `lg:col-5` + `lg:col-8`?
- [ ] Vrať `lg:col-4` + `lg:col-8`. Proč zrovna `<aside>` a `<section>`, a ne dva `<div>`? ______________

### Úkol P5: stav hry a hrací plocha (5 min)
Dočasné `HRA` v `<section>` nahraď:

```html
<!-- odznaky se stavem hry: vedle sebe, a když se nevejdou, zalomí se -->
<div class="d-flex flex-wrap gap-3 mb-5">
    <span class="badge theme-secondary fs-md">Hráč: <span id="hrac">?</span></span>
    <span class="badge theme-primary fs-md">Prasknuto: <span id="prasknuto">0</span> / <span id="celkem">0</span></span>
</div>

<!-- zpráva o výhře, zatím skrytá (d-none) -->
<div id="zprava" class="alert theme-success d-none"></div>

<!-- hrací plocha -->
<div id="plocha" class="plocha rounded-4 shadow">
    <p class="plocha-text">
        <span class="lg:d-none">👆</span><span class="d-none lg:d-inline">👈</span>
        <span>Nastav hru a klikni na Start</span>
    </p>
</div>
```

**Otázka P5a:** Proč jsou v textu na ploše dvě ručičky a kdy je vidět která?

> ______________________________________________________________

- [ ] Smaž dočasně `d-none` u `#zprava` a napiš do ní „Test“. Jak vypadá? Pak `d-none` vrať.

### Úkol P6: formulář v kartě (12 min)
Dočasné `FORMULÁŘ` v `<aside>` nahraď kartou:

```html
<div class="card">
    <div class="card-body">
        <h2 class="h5 mb-5">Nastavení hry</h2>

        <form id="nastaveni" class="d-flex flex-column gap-5">
            <!-- sem pole formuláře -->
        </form>
    </div>
</div>
```

V Bootstrapu má **každé pole stejnou stavbu**: obal `.form-field`, v něm `<label>` a pole.

```html
<div class="form-field">
    <label for="jmeno" class="form-label">Jméno hráče</label>
    <input type="text" id="jmeno" class="form-control" placeholder="např. Pepa">
</div>
```

`for` v labelu musí být **stejné** jako `id` pole. Pak klik na popisek skočí do pole a čtečka pro nevidomé ví, co pole znamená.

Podle vzoru přidej do formuláře:

- [ ] **Jméno hráče:** `type="text"`, `id="jmeno"` (vzor nahoře).
- [ ] **Počet:** `type="number"`, `id="pocet"`, `value="10"`.
- [ ] **Vzhled:** výběr ze seznamu. `select` má stejnou třídu jako input:
  ```html
  <select id="vzhled" class="form-control">
      <option value="barvy">Barevné čtverce</option>
      <option value="brouci">Brouci 🐞</option>
  </select>
  ```
- [ ] **Zvuk:** přepínač. Pozor, tady je label až **za** přepínačem:
  ```html
  <div class="form-field">
      <div class="switch">
          <input type="checkbox" id="zvuk" role="switch" checked>
      </div>
      <label for="zvuk">Zvuk 🔊</label>
  </div>
  ```
- [ ] **Tlačítko** na konec formuláře: `<button type="submit" class="btn-solid theme-primary w-100">Start 🎈</button>`

Otestuj:
- [ ] Klikni na text **„Jméno hráče“**. Kde je teď kurzor? ______________ Funguje to i u ostatních polí?
- [ ] Do „Počet“ zkus napsat písmena. Co se stane? ______________
- [ ] Klikni na **Start**. Co se stalo se stránkou a s adresou v prohlížeči? ______________
      *(Opravíme příští hodinu JavaScriptem.)*

**Otázka P6a:** Proč je mezi poli mezera, i když žádné pole nemá `mb-…`? (Nápověda: třídy na `<form>`.)

> ______________________________________________________________

### Úkol P7: dvě pole vedle sebe (5 min)
Na tabletu je formulář široký a pole „Počet“ a „Vzhled“ se vejdou vedle sebe. V úzkém levém sloupci od LG už ne.
Zabal oba `.form-field` do **vnořeného řádku**:

```html
<div class="row g-5">
    <div class="col-12 sm:col-6 lg:col-12 form-field">
        … Počet …
    </div>
    <div class="col-12 sm:col-6 lg:col-12 form-field">
        … Vzhled …
    </div>
</div>
```

| Šířka okna | 375 px | 700 px | 1100 px |
|---|---|---|---|
| Počet a Vzhled jsou pod sebou / vedle sebe | | | |

**Otázka P7a:** Na 1100 px je okno široké, a přesto jsou pole pod sebou. Proč? Podle čeho se rozhoduje `lg:`,
podle šířky **okna**, nebo šířky **karty**?

> ______________________________________________________________

### Úkol P8: kontrola (3 min)
- [ ] Projdi stránku na **375 px**, **768 px** a **1280 px**. Nikde není vodorovný posuvník.
- [ ] Příští hodinu bude JavaScript hledat prvky podle `id`. Zkontroluj, že máš **přesně** tyhle:

| `id` | prvek | | `id` | prvek |
|---|---|---|---|---|
| `nastaveni` | `<form>` | | `hrac` | jméno v odznaku |
| `jmeno` | pole jméno | | `prasknuto` | číslo v odznaku |
| `pocet` | pole počet | | `celkem` | číslo v odznaku |
| `vzhled` | `<select>` | | `zprava` | zelený pruh |
| `zvuk` | přepínač | | `plocha` | hrací plocha |

- [ ] Volitelně: smaž černý štítek s breakpointem a přidej patičku
      `<footer class="py-8 text-center fg-3">Balloons · tvoje jméno</footer>`.

### Úkol P9: git commit + git push (2 min)

---

## ⭐ Bonusy

- [ ] **Další typy polí.** Přidej do formuláře obtížnost jako **přepínače (radio)**. Stejné `name` = vybrat jde jen jeden:
  ```html
  <div class="form-field">
      <input type="radio" class="radio" name="obtiznost" id="lehka" value="lehka" checked>
      <label for="lehka">Lehká</label>
  </div>
  <div class="form-field">
      <input type="radio" class="radio" name="obtiznost" id="tezka" value="tezka">
      <label for="tezka">Těžká</label>
  </div>
  ```
- [ ] **Posuvník** (rychlost 1–10):
  `<div class="form-range"><input type="range" class="form-range-input" id="rychlost" min="1" max="10" value="5"></div>`
- [ ] **Sekce „Jak hrát“** pod hrou: tři karty (1. Nastav, 2. Start, 3. Praskej), na mobilu pod sebou,
      od MD vedle sebe (`row row-cols-1 md:row-cols-3 g-5`, viz Boxes).
- [ ] **Tmavý režim:** na `<html>` změň `data-bs-theme="light"` na `dark`. Co se změnilo samo a co ne?
- [ ] **Na mobilu nejdřív hra:** pomocí `order-…` dej na mobilu hrací plochu **nad** formulář, od LG zůstane vpravo.
      Je to pro hráče lepší, nebo horší?

---

## Kontrolní otázky

1. Co znamená **mobile-first**? Platí třída `col-12` i na velkém monitoru, když je u ní `lg:col-4`?
2. Proč musí být `for` v `<label>` stejné jako `id` pole? Co by se stalo, kdyby se lišily?
3. Napiš celé pole „E-mail“ (obal, popisek, pole), tak aby klik na popisek skočil do pole.
4. Podle čeho se řídí `sm:` / `lg:` ve vnořeném řádku uvnitř karty: podle šířky okna, nebo karty?
5. Jaký je rozdíl mezi `id="jmeno"` a `class="form-control"`? Kolik prvků na stránce smí mít stejné `id`?
