# Tahák: JavaScript + formuláře v Bootstrapu

> Layout, komponenty a stavbu formuláře najdeš v taháku z předlekce: [predlekce/tahak_layout_formular.md](predlekce/tahak_layout_formular.md).

---

## 1. Kam psát JavaScript a kde hledat chyby

```html
<body>
    …celé HTML…
    <script>
        console.log('JS běží');     // vypíše do konzole
    </script>
</body>
```

- `<script>` dávej **na konec `<body>`**, aby HTML prvky už existovaly, když je JS hledá.
- **F12 → Console**: výpisy z `console.log` a **červené chyby** (s číslem řádku, klikni na něj).
- Do konzole můžeš psát i přímo: `2 + 3`, `nahodne(1, 6)`, `vytvorBalonek()`…
- Změnil(a) jsem kód a nic? **Ulož** (Ctrl + S) a **obnov** (Ctrl + F5).

## 2. Proměnné a datové typy

```js
const plocha = document.querySelector('#plocha');   // const = nepřepíšu ji
let skore = 0;                                       // let = budu ji měnit
skore = skore + 1;     // nebo skore += 1;  nebo skore++;

let cislo = 42;            // number
let text = 'Ahoj';         // string (text), uvozovky '…' nebo "…"
let hotovo = false;        // boolean: true / false
let nic = null;            // „žádná hodnota“

const jmeno = 'Pepa';
`Ahoj ${jmeno}, máš ${skore} bodů`;    // šablona: zpětné apostrofy `…` + ${výraz}
```

| Pozor | Výsledek | Proč |
|---|---|---|
| `'2' + 3` | `'23'` | text + číslo = slepí se texty |
| `Number('2') + 3` | `5` | nejdřív převedu text na číslo |
| `Number('')` | `0` | prázdný text je 0 |
| `Number('abc')` | `NaN` | „Not a Number“ |
| `(3.14159).toFixed(1)` | `'3.1'` | zaokrouhlí a vrátí **text** |

## 3. Podmínky a porovnání

```js
if (skore === 10) {
    console.log('výhra');
} else if (skore > 5) {
    console.log('už skoro');
} else {
    console.log('hraj dál');
}
```

| Zápis | Význam |
|---|---|
| `===` / `!==` | rovná se / nerovná se (**vždy 3 znaky**, `=` je přiřazení!) |
| `<` `>` `<=` `>=` | menší, větší… |
| `&&` / `\|\|` / `!` | a zároveň / nebo / negace (`!hotovo` = „není hotovo“) |

## 4. Funkce

```js
function nahodne(min, max) {                 // parametry
    return Math.floor(Math.random() * (max - min + 1)) + min;   // vrátí výsledek
}
nahodne(1, 6);                               // zavolání → např. 4

// šipková funkce (arrow function): totéž kratší, hodí se pro události
const dvakrat = (x) => x * 2;
tlacitko.addEventListener('click', () => { … });
```

## 5. Cykly a pole

```js
for (let i = 0; i < 10; i++) {   // i = 0, 1, …, 9  → 10×
    vytvorBalonek();
}

const barvy = ['red', 'green', 'blue'];
barvy.length;          // 3
barvy[0];              // 'red' (počítá se od nuly!)
barvy.push('pink');    // přidá na konec
for (const barva of barvy) { console.log(barva); }

// filter = nové pole jen s prvky, pro které platí podmínka
const bezZelene = barvy.filter((b) => b !== 'green');
```

## 6. Objekty

```js
const b = { el: balonek, x: 50, y: 100, rychlost: 20 };
b.x;              // 50
b.y -= 5;         // změna vlastnosti
```

## 7. Matematika

| Zápis | Co dělá |
|---|---|
| `Math.random()` | náhodné číslo 0 až 0.999… |
| `Math.floor(4.9)` | 4 (zaokrouhlí dolů) |
| `Math.round(4.5)` | 5 |
| `Math.min(a, b)`, `Math.max(a, b)` | menší / větší z čísel |
| `Math.sin(x)` | sinus (x v radiánech), vrací −1 až 1 |
| `Math.PI` | 3.14159… |

## 8. Práce s HTML (DOM)

```js
// NAJÍT
const el = document.querySelector('#skore');         // první shoda (podle CSS selektoru)
const vse = document.querySelectorAll('.balonek');   // všechny shody

// VYTVOŘIT a VLOŽIT
const btn = document.createElement('button');
btn.classList.add('balonek');
plocha.appendChild(btn);

// SMAZAT
btn.remove();                 // jeden prvek
plocha.innerHTML = '';        // celý obsah plochy

// OBSAH
el.textContent = 42;

// TŘÍDY: zapínání/vypínání vzhledu
el.classList.add('d-none');       // skrýt (Bootstrap)
el.classList.remove('d-none');    // ukázat
el.classList.toggle('aktivni');
el.classList.contains('bomba');   // true / false

// STYL: jen pro „spojité“ hodnoty, které se nedají napsat dopředu do CSS (poloha, náhodná barva)
el.style.left = 40 + '%';         // NEZAPOMEŇ jednotku!
el.style.backgroundColor = 'hsl(200 80% 55%)';
el.style.backgroundImage = "url('img/beruska.svg')";   // obrázek jako pozadí
el.style.rotate = 45 + 'deg';                          // natočení
```

> Pravidlo: **stav** (skrytý / prasklý / otočený) měň **třídou**, **čísla** (poloha, barva) přes `style`.

## 9. Události

```js
btn.addEventListener('click', () => { … });          // kliknutí (i ťuknutí na mobilu)
input.addEventListener('input', () => { … });        // každá změna v políčku
formular.addEventListener('submit', (udalost) => {   // odeslání formuláře (tlačítko i Enter)
    udalost.preventDefault();                         // NEnačítat stránku znovu!
    …
});
document.addEventListener('keydown', (e) => { console.log(e.key); });   // klávesa
```

## 10. Formuláře a validace v Bootstrapu

### Hodnoty z formuláře

```js
document.querySelector('#jmeno').value;              // text
Number(document.querySelector('#pocet').value);      // číslo (value je VŽDY text)
document.querySelector('#zvuk').checked;             // checkbox / switch: true / false
document.querySelector('#pohyb').value;              // select: value vybrané <option>
document.querySelector('input[name="rezim"]:checked').value;   // vybraný radio button
```

### Pravidla = HTML atributy

| Atribut | Platí pro | Význam |
|---|---|---|
| `required` | vše | povinné |
| `minlength="2"` `maxlength="20"` | text | délka textu |
| `min="1"` `max="50"` | number, range, date | rozsah čísla |
| `step="1"` | number | krok (výchozí 1 = jen celá čísla; `step="any"` = cokoli) |
| `type="email"`, `type="url"` | input | musí vypadat jako e-mail / adresa |
| `pattern="[0-9]{3} ?[0-9]{2}"` | text | regulární výraz (tady PSČ) |

### Značkování (markup)

```html
<form id="nastaveni" novalidate data-bs-validate>
    <div class="form-field">
        <label for="jmeno" class="form-label">Jméno</label>
        <input type="text" id="jmeno" class="form-control" required minlength="2">
        <div class="invalid-feedback">Napiš aspoň 2 znaky.</div>    <!-- AŽ ZA inputem -->
    </div>

    <div class="form-field">
        <label for="pohyb" class="form-label">Pohyb</label>
        <select id="pohyb" class="form-control" required>
            <option value="">Vyber…</option>          <!-- prázdná hodnota = nevybráno -->
            <option value="klid">Klid</option>
        </select>
        <div class="invalid-feedback">Vyber pohyb.</div>
    </div>

    <div class="form-field">                          <!-- přepínač -->
        <div class="switch"><input type="checkbox" id="zvuk" role="switch"></div>
        <label for="zvuk">Zvuk</label>
    </div>

    <button type="submit" class="btn-solid theme-primary">Start</button>
</form>
```

| Atribut na `<form>` | Co dělá |
|---|---|
| `novalidate` | vypne bubliny prohlížeče **a nechá formulář odeslat i se špatnými údaji** → kontrola v JS |
| `data-bs-validate` | Bootstrap obarví chybná pole červeně a ukáže `.invalid-feedback`, ale až když s polem uživatel pracoval nebo zkusil formulář odeslat |
| `data-bs-validate="valid"` | navíc obarví správná pole zeleně (a ukáže `.valid-feedback`) |

### Kontrola v JS

```js
formular.addEventListener('submit', (udalost) => {
    udalost.preventDefault();
    if (!formular.checkValidity()) {    // nějaké pole porušuje pravidla
        return;                          // Bootstrap už chyby ukázal, dál nepokračujeme
    }
    // … tady je všechno v pořádku
});

// vlastní pravidlo, které atributy neumí
pole.setCustomValidity('Chybová zpráva');   // pole je NEplatné
pole.setCustomValidity('');                  // pole je zase OK
```

> **Validace v prohlížeči je pro pohodlí uživatele, ne ochrana.** Kdokoli ji v DevTools vypne.
> Skutečné weby musí všechno zkontrolovat znovu na serveru.

## 11. Čas a animace

```js
setTimeout(() => { … }, 1000);                 // jednou, za 1000 ms
const id = setInterval(() => { … }, 1000);     // každých 1000 ms…
clearInterval(id);                              // …dokud to nezastavím

Date.now();                // aktuální čas v ms (od roku 1970)
performance.now();         // ms od načtení stránky (přesnější)

// herní smyčka: prohlížeč zavolá funkci před každým snímkem obrazovky
function krok(ted) {
    // … posuň věci …
    requestAnimationFrame(krok);
}
requestAnimationFrame(krok);
```

## 12. Zvuk a paměť prohlížeče

```js
new Audio('zvuky/pop.wav').play();             // přehraje zvuk (jen po akci uživatele, např. kliku)

localStorage.setItem('rekord', 42);            // uloží (přežije zavření prohlížeče)
localStorage.getItem('rekord');                // '42' jako TEXT, nebo null když nic není
localStorage.removeItem('rekord');
```

## 13. Nejčastější chyby

| Chyba v konzoli / příznak | Příčina |
|---|---|
| `Cannot read properties of null (reading 'addEventListener')` | `querySelector` nic nenašel: překlep v `#id`, chybí `#`, nebo je `<script>` **nad** HTML |
| `xyz is not defined` | překlep v názvu proměnné/funkce (rozlišují se velká a malá písmena!) |
| `Unexpected token` / `missing )` | chybí závorka `)`, `}` nebo čárka. Odsazuj, ať to vidíš. |
| Po Startu se stránka „bliknutím“ obnoví | chybí `udalost.preventDefault()` |
| `5 + 5` z formuláře dává `55` | zapomněl(a) jsi `Number(…)` |
| Hra se spustí i se špatným formulářem | chybí `if (!formular.checkValidity()) return;` |
| Chyby se neukážou červeně | chybí `data-bs-validate`, nebo `.invalid-feedback` není **za** inputem |
| `left = 40` nic nedělá | chybí jednotka: `40 + '%'` nebo `40 + 'px'` |
