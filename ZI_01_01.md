# Základné dátové typy v jazyku C

## Úvod

V jazyku **C** dátový typ určuje:

- aký druh hodnoty môže premenná obsahovať,
- koľko pamäte zaberá,
- aký rozsah hodnôt vie uložiť,
- aké operácie sa s ňou dajú vykonávať.

Základné dátové typy v C môžeme rozdeliť na:

1. celočíselné typy,
2. znakové typy,
3. reálne typy.

---

## 1. Celočíselné dátové typy

Celočíselné typy slúžia na ukladanie celých čísel, teda čísel bez desatinnej časti.

Medzi základné celočíselné typy patria:

```c
char
short
int
long
long long
```

Každý z nich môže byť:

```c
signed
unsigned
```

---

## `signed` a `unsigned`

### `signed`

Typ `signed` môže obsahovať **kladné aj záporné hodnoty**.

Príklad:

```c
signed int a = -10;
int b = 20;
```

Pri type `int` je `signed` väčšinou implicitné, takže:

```c
int x;
```

znamená prakticky to isté ako:

```c
signed int x;
```

---

### `unsigned`

Typ `unsigned` môže obsahovať **iba nezáporné hodnoty**, teda hodnoty od `0` vyššie.

Príklad:

```c
unsigned int age = 25;
```

Výhoda je, že keďže sa nepoužíva znamienko, typ vie uložiť väčšie kladné hodnoty.

Približne:

```text
signed int:   -2147483648 až 2147483647
unsigned int: 0 až 4294967295
```

Presné rozsahy závisia od platformy a kompilátora.

---

## `char`

Typ `char` sa používa hlavne na uloženie **jedného znaku**.

```c
char letter = 'A';
```

Znak sa zapisuje do apostrofov:

```c
char c = 'x';
```

V skutočnosti je `char` malý celočíselný typ. Znak je uložený ako číslo podľa znakovej tabuľky, napríklad ASCII.

```c
char c = 'A';
printf("%d", c);
```

Výstup:

```text
65
```

Znak `'A'` má v ASCII hodnotu `65`.

---

## `short`

Typ `short` je menší celočíselný typ.

```c
short number = 1000;
```

Zvyčajne zaberá **2 bajty**.

Približný rozsah:

```text
signed short:   -32768 až 32767
unsigned short: 0 až 65535
```

---

## `int`

Typ `int` je najbežnejší celočíselný typ.

```c
int count = 50;
```

Používa sa na bežné celé čísla, cykly, počítadlá a indexy.

```c
for (int i = 0; i < 10; i++)
{
    printf("%d\n", i);
}
```

---

## `long`

Typ `long` slúži na uloženie väčších celých čísel.

```c
long population = 5400000;
```

Na niektorých systémoch má rovnakú veľkosť ako `int`, na iných je väčší.

Ešte väčší typ je:

```c
long long bigNumber = 9000000000LL;
```

---

## 2. Reálne dátové typy

Reálne typy slúžia na ukladanie čísel s desatinnou časťou.

V C sú hlavné reálne typy:

```c
float
double
long double
```

---

## `float`

Typ `float` používa tzv. **single precision**, čiže jednoduchú presnosť.

Zvyčajne zaberá **4 bajty**.

```c
float price = 12.99f;
```

Pri literáli typu `float` sa často pridáva prípona `f`:

```c
float x = 3.14f;
```

Bez `f` by sa hodnota `3.14` brala ako `double`.

---

## `double`

Typ `double` používa tzv. **double precision**, čiže dvojitú presnosť.

Zvyčajne zaberá **8 bajtov**.

```c
double pi = 3.141592653589793;
```

Typ `double` je presnejší ako `float` a používa sa častejšie pri výpočtoch s reálnymi číslami.

---

## Single precision a double precision

| Typ | Presnosť | Veľkosť | Príklad |
|---|---:|---:|---|
| `float` | single precision | zvyčajne 4 bajty | `3.14f` |
| `double` | double precision | zvyčajne 8 bajtov | `3.141592653589793` |

`float` má menšiu presnosť a vie uložiť menej platných číslic.

`double` má väčšiu presnosť, preto je vhodnejší pre presnejšie výpočty.

---

## Celé a reálne čísla

### Celé čísla

Celé čísla nemajú desatinnú časť.

```c
int a = 10;
int b = -5;
unsigned int c = 20;
```

Používajú sa napríklad na:

- počítadlá,
- indexy polí,
- počet kusov,
- identifikátory.

---

### Reálne čísla

Reálne čísla majú desatinnú časť.

```c
float x = 3.5f;
double y = -12.75;
```

Používajú sa napríklad na:

- merania,
- fyzikálne výpočty,
- priemery,
- vzdialenosti,
- percentá.

---

## Pevná a pohyblivá desatinná čiarka

### Pevná desatinná čiarka

Pri pevnej desatinnej čiarke je počet desatinných miest pevne daný.

Napríklad pri práci s peniazmi si môžeme povedať, že všetko budeme ukladať v centoch:

```c
int priceInCents = 1299;
```

To znamená:

```text
12,99 €
```

Výhoda:

- presné počítanie,
- vhodné pre peniaze.

Nevýhoda:

- programátor si musí sám strážiť mierku.

---

### Pohyblivá desatinná čiarka

Typy `float` a `double` používajú pohyblivú desatinnú čiarku.

Číslo sa interne ukladá približne v tvare:

```text
znamienko × mantisa × základ^exponent
```

Napríklad:

```text
123000 = 1.23 × 10^5
```

V C môžeme taký zápis použiť priamo:

```c
double x = 1.23e5;
```

To znamená:

```text
1.23 × 10^5 = 123000
```

---

## Exponenciálny tvar reálnych čísel

Exponenciálny tvar sa používa na zápis veľmi veľkých alebo veľmi malých čísel.

V C sa používa písmeno `e` alebo `E`.

```c
double a = 1.5e3;
double b = 2.5e-4;
```

Význam:

```text
1.5e3  = 1.5 × 10^3  = 1500
2.5e-4 = 2.5 × 10^-4 = 0.00025
```

Príklad:

```c
#include <stdio.h>

int main(void)
{
    double distance = 1.496e8;

    printf("Distance: %f km\n", distance);

    return 0;
}
```

---

## Aritmetické operácie

Aritmetické operácie slúžia na matematické výpočty.

| Operátor | Význam | Príklad |
|---|---|---|
| `+` | sčítanie | `a + b` |
| `-` | odčítanie | `a - b` |
| `*` | násobenie | `a * b` |
| `/` | delenie | `a / b` |
| `%` | zvyšok po delení | `a % b` |

---

### Príklad aritmetických operácií

```c
int a = 10;
int b = 3;

printf("%d\n", a + b); // 13
printf("%d\n", a - b); // 7
printf("%d\n", a * b); // 30
printf("%d\n", a / b); // 3
printf("%d\n", a % b); // 1
```

Pozor pri celočíselnom delení:

```c
int result = 10 / 3;
```

Výsledok bude:

```text
3
```

Nie:

```text
3.333
```

Ak chceme reálny výsledok:

```c
double result = 10.0 / 3.0;
```

Výsledok bude približne:

```text
3.333333
```

---

## Relačné operácie

Relačné operácie porovnávajú dve hodnoty.

Výsledkom je pravdivostná hodnota:

- `0` znamená nepravda,
- nenulová hodnota znamená pravda,
- najčastejšie `1`.

| Operátor | Význam | Príklad |
|---|---|---|
| `==` | rovná sa | `a == b` |
| `!=` | nerovná sa | `a != b` |
| `<` | menšie ako | `a < b` |
| `>` | väčšie ako | `a > b` |
| `<=` | menšie alebo rovné | `a <= b` |
| `>=` | väčšie alebo rovné | `a >= b` |

---

### Príklad relačných operácií

```c
int a = 5;
int b = 10;

printf("%d\n", a < b);  // 1
printf("%d\n", a > b);  // 0
printf("%d\n", a == b); // 0
printf("%d\n", a != b); // 1
```

---

## Priority operátorov

Operátory majú rôznu prioritu. To znamená, že niektoré sa vyhodnocujú skôr ako iné.

Základné poradie:

| Priorita | Operátory | Význam |
|---:|---|---|
| 1 | `()` | zátvorky |
| 2 | `*`, `/`, `%` | násobenie, delenie, zvyšok |
| 3 | `+`, `-` | sčítanie, odčítanie |
| 4 | `<`, `<=`, `>`, `>=` | porovnanie |
| 5 | `==`, `!=` | rovnosť, nerovnosť |
| 6 | `=` | priradenie |

Príklad:

```c
int result = 2 + 3 * 4;
```

Výsledok je:

```text
14
```

Lebo násobenie má vyššiu prioritu ako sčítanie.

Ak chceme iné poradie:

```c
int result = (2 + 3) * 4;
```

Výsledok je:

```text
20
```

---

## Pretečenie

Pretečenie nastane vtedy, keď sa do dátového typu pokúsime uložiť hodnotu väčšiu, než je jeho maximálny rozsah.

Príklad pre `unsigned char`:

```c
unsigned char x = 255;
x = x + 1;

printf("%u\n", x);
```

Výsledok bude často:

```text
0
```

Pretože `unsigned char` má typicky rozsah:

```text
0 až 255
```

Po hodnote `255` sa pri pripočítaní `1` hodnota „pretočí“ späť na `0`.

---

### Pretečenie pri `signed` typoch

Pri `signed` typoch je pretečenie problémovejšie.

```c
int x = 2147483647;
x = x + 1;
```

Toto je v C **nedefinované správanie** pre signed integer overflow.

To znamená, že program sa nemusí správať spoľahlivo.

---

## Podtečenie

Podtečenie môže znamenať dve veci podľa kontextu.

---

### Podtečenie pri celočíselných typoch

Pri `unsigned` typoch vznikne, keď ideme pod nulu.

```c
unsigned int x = 0;
x = x - 1;

printf("%u\n", x);
```

Výsledok bude veľmi veľké číslo, napríklad:

```text
4294967295
```

Lebo `unsigned int` nevie uložiť zápornú hodnotu.

---

### Podtečenie pri reálnych typoch

Pri `float` alebo `double` podtečenie nastane, keď je číslo príliš malé na to, aby sa dalo presne reprezentovať.

```c
float x = 1.0e-45f;
float y = x / 10.0f;
```

Hodnota môže byť zaokrúhlená na `0`.

---

## Presnosť aritmetických operácií

Pri celočíselných typoch sú výpočty presné, pokiaľ nedôjde k pretečeniu alebo podtečeniu.

```c
int a = 10;
int b = 3;

int c = a / b; // výsledok je 3
```

Výsledok je presný v rámci celočíselnej aritmetiky, ale stratí sa desatinná časť.

---

### Presnosť pri `float` a `double`

Reálne čísla sa v počítači často nedajú uložiť úplne presne.

Typický príklad:

```c
double x = 0.1 + 0.2;

printf("%.17f\n", x);
```

Výstup môže byť:

```text
0.30000000000000004
```

Nie presne:

```text
0.3
```

Dôvod je, že niektoré desatinné čísla sa v binárnej sústave nedajú reprezentovať presne.

---

## Porovnávanie reálnych čísel

Pri `float` a `double` sa nemá vždy používať priame porovnanie `==`.

Nevhodné:

```c
double x = 0.1 + 0.2;

if (x == 0.3)
{
    printf("Equal\n");
}
```

Lepšie je porovnávať s toleranciou:

```c
#include <math.h>
#include <stdio.h>

int main(void)
{
    double x = 0.1 + 0.2;
    double expected = 0.3;
    double epsilon = 0.000001;

    if (fabs(x - expected) < epsilon)
    {
        printf("Approximately equal\n");
    }

    return 0;
}
```

---

## Súhrnný príklad

```c
#include <stdio.h>

int main(void)
{
    int count = 10;
    unsigned int positive = 20;
    char letter = 'A';
    float temperature = 36.6f;
    double pi = 3.141592653589793;

    printf("Count: %d\n", count);
    printf("Positive: %u\n", positive);
    printf("Letter: %c\n", letter);
    printf("Letter as number: %d\n", letter);
    printf("Temperature: %.1f\n", temperature);
    printf("Pi: %.15f\n", pi);

    printf("Arithmetic: %d\n", count + 5);
    printf("Relation: %d\n", count > 5);

    return 0;
}
```

Možný výstup:

```text
Count: 10
Positive: 20
Letter: A
Letter as number: 65
Temperature: 36.6
Pi: 3.141592653589793
Arithmetic: 15
Relation: 1
```

---

## Krátke zhrnutie

| Pojem | Význam |
|---|---|
| `char` | znak alebo malé celé číslo |
| `short` | menšie celé číslo |
| `int` | bežné celé číslo |
| `long` | väčšie celé číslo |
| `signed` | kladné aj záporné hodnoty |
| `unsigned` | iba nezáporné hodnoty |
| `float` | reálne číslo s jednoduchou presnosťou |
| `double` | reálne číslo s dvojitou presnosťou |
| pretečenie | hodnota prekročí maximum typu |
| podtečenie | hodnota ide pod minimum alebo je príliš malá |
| pevná čiarka | pevne určený počet desatinných miest |
| pohyblivá čiarka | reprezentácia pomocou mantisy a exponentu |
| `1.5e3` | exponenciálny zápis, znamená `1500` |

---

## Mini odpoveď vhodná na skúšku

Základné dátové typy v jazyku C slúžia na uchovávanie rôznych druhov údajov. Medzi celočíselné typy patria `char`, `short`, `int`, `long` a ich varianty `signed` a `unsigned`. Typ `signed` umožňuje ukladať kladné aj záporné hodnoty, zatiaľ čo `unsigned` iba nezáporné hodnoty. Reálne čísla sa ukladajú pomocou typov `float` a `double`, pričom `float` používa jednoduchú presnosť a `double` dvojitú presnosť. V C existujú aritmetické operácie ako sčítanie, odčítanie, násobenie, delenie a zvyšok po delení, a relačné operácie ako `<`, `>`, `==` alebo `!=`. Operátory majú určenú prioritu, napríklad násobenie sa vykoná skôr ako sčítanie. Pri výpočtoch môže dôjsť k pretečeniu, keď hodnota prekročí rozsah typu, alebo k podtečeniu, keď je hodnota príliš malá. Reálne čísla sa často ukladajú v pohyblivej desatinnej čiarke a môžu byť zapísané aj exponenciálne, napríklad `1.5e3`, čo znamená `1500`.
