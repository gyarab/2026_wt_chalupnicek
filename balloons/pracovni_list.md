# Pracovní list: Balloons, úvod do JavaScriptu

**Jméno:** ______________________  **Třída:** ______  **Datum:** ____________

Postavíme hru: na obloze jsou „balónky“ (zatím barevné čtverečky) a ty je klikáním praskáš. Kolik jich bude,
nastavíš ve formuláři a formulář si ohlídá, že do něj nikdo nenapíše nesmysl.
Po ruce měj [tahak_js.md](tahak_js.md). Hotové úkoly si odškrtávej `[x]`, odpovědi piš přímo sem.

> **Ztratil(a) ses?** Ve složce [ukazky/](ukazky/) je stav po každém kroku:
> `01_balonky.html` (po úkolu 4), `02_praskani.html` (po úkolu 5), `03_formular.html` (po úkolu 7).
> Zkopíruj si celý `<script>` odtud a pokračuj s ostatními.
> V ukázkách jsou cesty k obrázkům `../img/…`, u tebe v `index.html` jen `img/…`.

---

### Úkol 1: příprava (3 min)
- [ ] **Dělal(a) jsi předlekci?** Pokračuj ve svém `balloons/index.html`.
      **Nedělal(a)?** Ulož [sablona.html](sablona.html) jako `balloons/index.html`.
- [ ] Zkopíruj si do `balloons/` i složky [img/](img/) (obrázky hmyzu) a [zvuky/](zvuky/) (zvuk na bonus).
- [ ] V kořeni repozitáře spusť `py -3 -m http.server` a otevři <http://localhost:8000/balloons/>.
- [ ] **F12** → záložka **Console**. Musí tam být `Balloons: JavaScript běží!`.

> Nevidíš to? Ulož soubor (Ctrl + S) a obnov stránku (Ctrl + F5).

### Úkol 2: JavaScript v konzoli (5 min)
Do konzole piš výrazy a Enterem je spusť. Výsledek opiš do tabulky.

| Výraz | Výsledek |
|---|---|
| `2 + 3` | |
| `'2' + 3` | |
| `Number('2') + 3` | |
| `Math.random()` (spusť 3×) | |
| `Math.floor(4.9)` | |
| `Math.floor(Math.random() * 6) + 1` (spusť 3×) | |

Poslední výraz je hod kostkou. Napiš do `<script>` (sekce **3) FUNKCE**) funkci, která to umí pro libovolný rozsah:

```js
// náhodné celé číslo od min do max (včetně obou)
function nahodne(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}
```

- [ ] Ulož, obnov stránku a v konzoli vyzkoušej `nahodne(1, 6)` a `nahodne(0, 100)`.

**Otázka 2a:** Proč je výsledek `'2' + 3` jiný než `2 + 3`?

> ______________________________________________________________

### Úkol 3: jeden balónek (6 min)
Do sekce **1) PRVKY ZE STRÁNKY** si najdi plochu, kam budeme balónky dávat:

```js
const plocha = document.querySelector('#plocha');
```

Do sekce **3) FUNKCE** přidej funkci, která vyrobí jeden balónek. CSS třídu `.balonek` už šablona má:
zatím je to barevný čtverec s kulatými rohy, pěkné balónky přijdou později.

```js
function vytvorBalonek() {
    const balonek = document.createElement('button');   // nový <button>
    balonek.classList.add('balonek');                   // class="balonek"
    balonek.style.left = nahodne(10, 90) + '%';         // náhodně vodorovně
    balonek.style.top = nahodne(15, 80) + '%';          // náhodně svisle
    // náhodná barva: odstín 0–359 (kolo barev), sytost 80 %, světlost 55 %
    balonek.style.backgroundColor = `hsl(${nahodne(0, 359)} 80% 55%)`;
    plocha.appendChild(balonek);                        // vložit do stránky
}
```

- [ ] Ulož, obnov a do konzole napiš `vytvorBalonek()`. Zkus to 5×.
- [ ] Najdi balónek v záložce **Elements** (pravým na balónek → *Prozkoumat*). Jaké má `style`?

**Otázka 3a:** Co dělá `` `hsl(${…} 80% 55%)` ``? Zkus v konzoli `` `Ahoj ${1 + 1}` ``.

> ______________________________________________________________

- [ ] ⭐ **Brouci místo čtverců (smash a bug):** ve složce `img/` je 5 obrázků hmyzu. Řádek s `backgroundColor` nahraď:
  ```js
  balonek.classList.add('obrazek');                        // CSS: průhledné pozadí, obrázek přes celý čtverec
  balonek.style.backgroundImage = "url('img/beruska.svg')";
  ```
  ⭐⭐ Náhodný brouk a náhodné natočení:
  ```js
  const BROUCI = ['beruska', 'vcela', 'mravenec', 'moucha', 'brouk'];   // dej do sekce 2)
  balonek.style.backgroundImage = `url('img/${BROUCI[nahodne(0, 4)]}.svg')`;
  balonek.style.rotate = nahodne(0, 359) + 'deg';
  ```

### Úkol 4: hodně balónků najednou (3 min)
Do sekce **4)** dej cyklus, který zavolá funkci 10×:

```js
plocha.innerHTML = '';              // smaže text „Nastav hru…“
for (let i = 0; i < 10; i++) {
    vytvorBalonek();
}
```

- [ ] Po obnovení je na obloze 10 balónků. Změň číslo na 30.

**Otázka 4a:** Kolik balónků vznikne, když napíšu `for (let i = 1; i <= 10; i++)`? ______
A kolik při `for (let i = 0; i <= 10; i++)`? ______

### Úkol 5: praskání a skóre (7 min)

**a)** Do sekce **1)** přidej prvky, kam budeme psát čísla a zprávu:

```js
const prasknutoEl = document.querySelector('#prasknuto');
const celkemEl = document.querySelector('#celkem');
const zpravaEl = document.querySelector('#zprava');
```

**b)** Do sekce **2) STAV HRY** přidej proměnné. `let` = hodnota se bude měnit.

```js
let prasknuto = 0;      // kolik balónků už hráč praskl
let celkem = 0;         // kolik balónků je ve hře
```

**c)** Do funkce `vytvorBalonek()` přidej **před** `plocha.appendChild(balonek);` reakci na kliknutí:

```js
balonek.addEventListener('click', () => {
    balonek.remove();           // zmizí ze stránky
    prasknuto++;                // prasknuto = prasknuto + 1
    ukazStav();
    if (prasknuto === celkem) {
        zpravaEl.textContent = '🎉 Všechny balónky prasknuté!';
        zpravaEl.classList.remove('d-none');    // zobrazí zelený pruh
    }
});
```

**d)** Přidej funkce `ukazStav()` a `novaHra(pocet)` a cyklus z úkolu 4 nahraď voláním `novaHra(10);`:

```js
function ukazStav() {
    prasknutoEl.textContent = prasknuto;
    celkemEl.textContent = celkem;
}

function novaHra(pocet) {
    plocha.innerHTML = '';
    prasknuto = 0;
    celkem = pocet;
    zpravaEl.classList.add('d-none');
    for (let i = 0; i < pocet; i++) {
        vytvorBalonek();
    }
    ukazStav();
}
```

- [ ] Klikáním praskáš balónky, odznak ukazuje „Prasknuto: 3 / 10“, na konci se objeví zelená zpráva.
- [ ] ⭐ Změř čas: na konec `novaHra` dej `start = Date.now();` (a `let start = 0;` do stavu).
      Ve zprávě pak ukaž `((Date.now() - start) / 1000).toFixed(1)` sekund.

**Otázka 5a:** Proč je v podmínce `===` a ne `=`?

> ______________________________________________________________

> 🎈 **Odměna (až řekne vyučující): skutečné balónky.** Zkopíruj si [balonek.css](balonek.css) do `balloons/`
> a do `<head>` přidej řádek `<link rel="stylesheet" href="balonek.css">`. Z čtverců jsou balónky s provázkem,
> v JavaScriptu se nemění nic. Brouci zůstanou brouky.

### Úkol 6: formulář spustí hru (6 min)
Smaž řádek `novaHra(10);`. Hra se teď spustí až tlačítkem **Start**.

```js
// sekce 1)
const formular = document.querySelector('#nastaveni');
const hracEl = document.querySelector('#hrac');

// sekce 4)
formular.addEventListener('submit', (udalost) => {
    udalost.preventDefault();       // NEodesílat formulář = nenačítat stránku znovu

    const jmeno = document.querySelector('#jmeno').value;
    const pocet = Number(document.querySelector('#pocet').value);   // value je vždy TEXT
    hracEl.textContent = jmeno;
    novaHra(pocet);
});
```

- [ ] Vyplň jméno a počet, klikni **Start**. Funguje i **Enter** v políčku?
- [ ] ⭐ Použij i výběr **Vzhled**: do stavu dej `let vzhled = 'barvy';`, v posluchači
      `vzhled = document.querySelector('#vzhled').value;` a ve `vytvorBalonek()` podle něj
      `if (vzhled === 'brouci') { …brouk… } else { …barva… }`.
- [ ] Zkus hře „ublížit“. Co se stane, když do formuláře zadáš:

| Jméno | Počet | Co se stalo? |
|---|---|---|
| *(prázdné)* | 10 | |
| Pepa | 1000 | |
| Pepa | -5 | |
| Pepa | 2.5 | |

**Otázka 6a:** Proč hra s počtem `2.5` nikdy neskončí? (Kolik balónků vznikne a kolik je `celkem`?)

> ______________________________________________________________

### Úkol 7: validace formuláře v Bootstrapu (8 min)

**a)** Do HTML políček přidej **pravidla** (atributy):

```html
<input type="text" id="jmeno" class="form-control" placeholder="např. Pepa"
       required minlength="2" maxlength="20">

<input type="number" id="pocet" class="form-control" value="10"
       required min="1" max="50">
```

- [ ] Klikni na Start s prázdným jménem. Kdo ukázal chybu, prohlížeč, nebo Bootstrap? ______________

**b)** Necháme chyby ukázat Bootstrap. Do `<form>` přidej `novalidate data-bs-validate`
a pod každé `<input>` (uvnitř `.form-field`) text chyby:

```html
<form id="nastaveni" class="d-flex flex-column gap-5" novalidate data-bs-validate>
    <div class="form-field">
        <label for="jmeno" class="form-label">Jméno hráče</label>
        <input type="text" id="jmeno" … >
        <div class="invalid-feedback">Napiš jméno, aspoň 2 znaky.</div>
    </div>
    …
```

**c)** `novalidate` vypne bubliny prohlížeče, ale **zároveň mu dovolí formulář odeslat**.
Kontrolu proto musíme udělat sami v JS, hned za `preventDefault()`:

```js
if (!formular.checkValidity()) {    // ! = NE → „když formulář NENÍ v pořádku“
    return;                          // konec, hru nespouštíme
}
```

- [ ] Otestuj a vyplň:

| Jméno | Počet | Červené pole? | Spustí se hra? |
|---|---|---|---|
| *(prázdné)* | 10 | | |
| P | 10 | | |
| Pepa | 0 | | |
| Pepa | 51 | | |
| Pepa | 2.5 | | |
| Pepa | 20 | | |

- [ ] ⭐ Změň `data-bs-validate` na `data-bs-validate="valid"`. Co se stane se správně vyplněnými poli?

**Otázka 7a:** Kdy se červená chyba objeví? Hned po načtení stránky, nebo až později? Proč je to tak lepší?

> ______________________________________________________________

**Otázka 7b:** Spolužák v DevTools smaže z `<input>` atribut `max="50"` a spustí hru s 10 000 balónky.
Co z toho plyne pro validaci formulářů na skutečných webech (přihlášení, e-shop)?

> ______________________________________________________________

### Úkol 8: git commit + git push (2 min)

---

## ⭐ Bonusy (pro rychlé a na doma)

Řešení všech bonusů najdeš v ukázce [ukazky/04_bonusy.html](ukazky/04_bonusy.html), ale zkus to nejdřív sám/sama.

### B1: zvuk 🔊
Do reakce na kliknutí přidej:
```js
new Audio('zvuky/pop.wav').play();
```
Přepínač „Zvuk“ už ve formuláři máš: přehrávej jen když `document.querySelector('#zvuk').checked` je `true`.

### B2: animace prasknutí 💥
Do `<style>` přidej:
```css
.balonek.prask {
    animation: prask .6s ease-out forwards;
    pointer-events: none;            /* na praskající balónek už nejde kliknout */
}
@keyframes prask {
    40%  { scale: 1.4; opacity: 1; }     /* nafouknout */
    100% { scale: 2; opacity: 0; }       /* prasknout a zmizet */
}
```
V JS místo `balonek.remove();` napiš:
```js
balonek.classList.add('prask');
setTimeout(() => balonek.remove(), 600);    // smazat až po animaci (600 ms)
```

### B3: balónky se houpou 🎈
Jen CSS (nejlépe s `balonek.css`, ale houpou se i čtverce). Do `.balonek` přidej `transform-origin: 50% 130%;` a `animation: houpani 1.4s ease-in-out infinite alternate;`
a pod něj:
```css
@keyframes houpani {
    from { rotate: -6deg; }
    to   { rotate: 6deg; }
}
```
Houpou se všechny stejně? V `vytvorBalonek()` přidej `balonek.style.animationDelay = -nahodne(0, 1400) + 'ms';`.

> Máš i B2? Pak v reakci na klik **před** `classList.add('prask')` přidej `balonek.style.animationDelay = '0s';`.
> Jinak by animace prasknutí převzala náhodné zpoždění z houpání a začala by „v půlce“, nebo by ji vůbec nebylo vidět.

### B4: balónky stoupají po přímce ⬆️
Balónek si teď musí pamatovat svou polohu. Místo samotného `<button>` budeme mít **objekt**:

```js
let balonky = [];                       // sekce 2): všechny balónky ve hře

// ve vytvorBalonek():
const b = {
    el: balonek,                        // HTML prvek
    x: nahodne(12, 88),                 // poloha v %
    y: nahodne(105, 160),               // začíná POD plochou
    rychlost: nahodne(10, 25),          // % výšky za sekundu
};
balonky.push(b);
balonek.style.left = b.x + '%';
balonek.style.top = b.y + '%';
// v reakci na klik: vyřadit z pole, ať se dál nehýbe
balonky = balonky.filter((jiny) => jiny !== b);

// v novaHra(): na začátek
balonky = [];
```

A **herní smyčka**, kterou prohlížeč volá před každým vykreslením obrazovky:

```js
let posledni = 0;
function krok(ted) {
    const dt = Math.min((ted - posledni) / 1000, 0.1);   // sekund od minulého snímku
    posledni = ted;
    for (const b of balonky) {
        b.y -= b.rychlost * dt;         // nahoru = menší top
        if (b.y < -15) {                // uletěl nahoru → znovu zespodu
            b.y = 115;
        }
        b.el.style.top = b.y + '%';
    }
    requestAnimationFrame(krok);        // a zase příště
}
requestAnimationFrame(krok);            // nastartovat (jednou, na konci skriptu)
```

> Proč `dt`? Monitor může mít 60 Hz i 144 Hz. Kdybychom posouvali „o kousek za snímek“,
> na rychlém monitoru by balónky letěly 2,4× rychleji.

### B5: stoupání po sinusovce 〰️
Do objektu přidej `faze: Math.random() * 2 * Math.PI` a v `krok()` počítej `left` přes sinus:
```js
b.el.style.left = (b.x + 6 * Math.sin(b.y / 6 + b.faze)) + '%';
```
Zkus měnit `6` (šířka vlny) a `/ 6` (hustota vln). Proč tam je `faze`? Zkus ji smazat.

### B6: herní módy 🎮
Přidej do formuláře `<select id="pohyb" class="form-control" required>` s volbami
`""` (Vyber…), `klid`, `primka`, `sinus`. Pod něj `.invalid-feedback`: validace hlídá i `select`.
V `krok()` pak podle `pohyb` balónky buď nehýbej, nebo posouvej rovně, nebo po vlnovce.
Dej ho do vnořeného řádku vedle „Vzhledu“ stejně jako „Počet“ (`col-12 sm:col-6 lg:col-12`).

Další nápady:
- **Na čas:** 20 sekund, prasklý balónek se hned objeví nový, cílem je co nejvyšší skóre.
- **Bomby 💣:** pár černých balónků; kdo trefí bombu, prohrál. (Třída `bomba` + `if` v reakci na klik.)
- **Jen jedna barva:** praskat se smí jen červené, za jinou barvu −1 bod.

### B7: vlastní pravidlo validace
Atributy neumí porovnat dvě pole. Přidej políčko „Počet bomb“ a pravidlo „bomb musí být méně než balónků“:
```js
function zkontrolujBomby() {
    const bomby = document.querySelector('#bomby');
    const pocet = document.querySelector('#pocet');
    if (Number(bomby.value) >= Number(pocet.value)) {
        bomby.setCustomValidity('Bomb musí být méně než balónků.');  // pole je teď NEplatné
    } else {
        bomby.setCustomValidity('');                                  // prázdný text = OK
    }
}
```
Volej ji při psaní (`addEventListener('input', zkontrolujBomby)` na obě pole) a před `checkValidity()`.

### B8: rekord 🏆
Nejlepší výsledek si zapamatuj i po zavření prohlížeče:
```js
localStorage.setItem('balloons-rekord', 42);          // uložit
const rekord = localStorage.getItem('balloons-rekord');  // načíst (je to TEXT, nebo null)
```

---

## Kontrolní otázky (na závěr)

1. Jaký je rozdíl mezi `let` a `const`? Proč je `prasknuto` `let` a `plocha` `const`?
2. Co vrátí `document.querySelector('#plocha')`, když se v HTML spletu a napíšu `id="ploha"`?
   Jakou chybu pak uvidíš v konzoli?
3. Proč musíme `input.value` převádět přes `Number()`?
4. Co dělají v `<form>` atributy `novalidate` a `data-bs-validate`? Co by se stalo, kdybys dal(a) jen `novalidate`
   a v JS zapomněl(a) na `checkValidity()`?
5. Které tři atributy použiješ, aby políčko „věk“ bylo povinné a bralo jen čísla 15 až 99?
