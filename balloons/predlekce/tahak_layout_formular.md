# Tahák: responzivní layout a formulář v Bootstrapu

> Podrobnější tahák ke gridu a utilitám: [../../boxes/cheatsheet.md](../../boxes/cheatsheet.md).

---

## 1. Breakpointy a mobile-first

| Prefix | Od šířky okna | Zařízení |
|---|---:|---|
| *(žádný)* | 0 px | mobil |
| `sm:` | 576 px | mobil na šířku |
| `md:` | 768 px | tablet |
| `lg:` | 1024 px | notebook |
| `xl:` | 1280 px | monitor |
| `2xl:` | 1536 px | velký monitor |

**Třída bez prefixu platí všude, třída s prefixem od té šířky výš.** Prefixy se řídí šířkou **okna**,
i když je prvek schovaný v úzké kartě.

```
col-12 lg:col-4
│       └── od 1024 px: 4 díly z 12 (třetina)
└────────── všude, tedy i na mobilu: 12 z 12 (celá šířka)
```

## 2. Layout stránky hry

```html
<nav class="navbar bg-1">                         <!-- lišta přes celou šířku -->
    <div class="container">…</div>                <!-- obsah lišty zarovnaný s obsahem stránky -->
</nav>

<main class="container py-8">                     <!-- kontejner = vycentrovaný obsah s okraji -->
    <div class="row g-5">                         <!-- řádek, g-5 = mezery mezi sloupci -->
        <aside class="col-12 lg:col-4">…</aside>  <!-- mobil: celá šířka, od LG: 4/12 -->
        <section class="col-12 lg:col-8">…</section>
    </div>
</main>
```

- Součet `col-*` v řádku má být **12**. Víc → poslední sloupec spadne na nový řádek.
- Sloupce jsou na mobilu **pod sebou v pořadí z HTML**. Jiné pořadí: `order-2 lg:order-1` / `order-1 lg:order-2`.
- **Vnořený řádek:** uvnitř sloupce (nebo karty) můžeš udělat další `row` s vlastními `col-*`, opět z 12 dílů.

## 3. Užitečné třídy na této stránce

| Třída | Co dělá |
|---|---|
| `d-flex flex-wrap gap-3` | prvky vedle sebe, když se nevejdou, zalomí se, mezera 0,5 rem |
| `d-flex flex-column gap-5` | prvky pod sebou s mezerou 1 rem (náš formulář) |
| `mb-5`, `py-8`, `p-5` | mezery: margin bottom / padding nahoře+dole / padding všude |
| `d-none`, `lg:d-none`, `d-none lg:d-inline` | skrýt všude / skrýt od LG / ukázat až od LG |
| `rounded-4`, `shadow` | zaoblené rohy, stín |
| `w-100` | šířka 100 % (tlačítko přes celou kartu) |
| `fs-md`, `fw-bold`, `h5` | velikost písma, tučně, vzhled nadpisu h5 |
| `fg-3` | šedý text |

## 4. Komponenty

```html
<!-- karta -->
<div class="card">
    <div class="card-body">
        <h2 class="h5 mb-5">Nadpis</h2>
        …
    </div>
</div>

<!-- odznak (badge) -->
<span class="badge theme-primary fs-md">Prasknuto: 0</span>

<!-- upozornění (alert), barva přes theme-* -->
<div class="alert theme-success">Hotovo!</div>

<!-- tlačítko -->
<button type="submit" class="btn-solid theme-primary w-100">Start</button>
```

Barvy `theme-*`: `primary` (modrá), `secondary` (šedá), `success` (zelená), `danger` (červená), `warning` (žlutá), `info` (světle modrá).

## 5. Formuláře

### Stavba jednoho pole

```html
<div class="form-field">                                   <!-- obal: popisek a pole pod sebou -->
    <label for="jmeno" class="form-label">Jméno</label>    <!-- for = id pole -->
    <input type="text" id="jmeno" class="form-control" placeholder="např. Pepa">
</div>
```

- `for` a `id` musí být **stejné**: klik na popisek skočí do pole, čtečka pro nevidomé přečte popisek.
- `placeholder` je jen nápověda uvnitř pole, **nenahrazuje** `<label>`. Po napsání zmizí.
- `id` je na stránce **jedinečné**, podle něj pole najde JavaScript.

### Typy polí

| Chci | HTML |
|---|---|
| text | `<input type="text" class="form-control">` |
| číslo (na mobilu číselná klávesnice) | `<input type="number" class="form-control" value="10">` |
| e-mail | `<input type="email" class="form-control">` |
| výběr ze seznamu | `<select class="form-control"><option value="a">A</option>…</select>` |
| víceřádkový text | `<textarea class="form-control" rows="3"></textarea>` |
| zaškrtávátko | `<input type="checkbox" class="check" id="x">` + `<label for="x">` |
| přepínač (switch) | `<div class="switch"><input type="checkbox" id="x" role="switch"></div>` + `<label for="x">` |
| jedna z možností (radio) | `<input type="radio" class="radio" name="skupina" id="x" value="…">` + `<label for="x">`; stejné `name` = jedna skupina |
| posuvník | `<div class="form-range"><input type="range" class="form-range-input" min="1" max="10"></div>` |

U `check`, `switch` a `radio` je `<label>` **za** prvkem, oba uvnitř `.form-field`:

```html
<div class="form-field">
    <div class="switch"><input type="checkbox" id="zvuk" role="switch" checked></div>
    <label for="zvuk">Zvuk</label>
</div>
```

### Celý formulář

```html
<form id="nastaveni" class="d-flex flex-column gap-5">      <!-- pole pod sebou s mezerou -->
    <div class="form-field">…</div>

    <div class="row g-5">                                     <!-- dvě pole vedle sebe -->
        <div class="col-12 sm:col-6 form-field">…</div>
        <div class="col-12 sm:col-6 form-field">…</div>
    </div>

    <button type="submit" class="btn-solid theme-primary w-100">Start</button>
</form>
```

- `type="submit"`: tlačítko odešle formulář (i Enter v poli). Bez JavaScriptu se stránka znovu načte.
- Kontrolu správnosti (validaci) přidáme v hodině s JavaScriptem.

## 6. Testování

1. **F12** → **Ctrl + Shift + M** (režim zařízení) → nahoře *Responsive* a táhni šířkou.
2. Vždy zkontroluj **375 px**, **768 px** a **1280 px**. Nesmí být vodorovný posuvník.
3. Pole na mobilu: v režimu zařízení vyber konkrétní telefon. Číselnou klávesnici ale uvidíš jen na skutečném mobilu.
