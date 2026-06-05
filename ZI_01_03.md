# Smerníky a dynamické údaje v jazyku C

## Úvod

V jazyku **C** patria smerníky medzi najdôležitejšie, ale aj najproblematickejšie časti jazyka.

Smerníky umožňujú:

- pracovať priamo s adresami v pamäti,
- odovzdávať premenné funkciám cez adresu,
- vytvárať dynamické údaje,
- pracovať s dynamickými poľami,
- vytvárať dátové štruktúry ako zoznamy, stromy alebo grafy.

Dynamická pamäť sa používa vtedy, keď dopredu nevieme, koľko pamäte bude program potrebovať.

---

## 1. Typ smerník

Smerník je premenná, ktorá neobsahuje obyčajnú hodnotu, ale **adresu inej premennej v pamäti**.

Bežná premenná:

```c
int x = 10;
```

Smerník na `int`:

```c
int *p;
```

Premenná `p` môže obsahovať adresu premennej typu `int`.

---

## Deklarácia smerníka

Všeobecný zápis:

```c
typ *nazov_smernika;
```

Príklady:

```c
int *p;
double *d;
char *c;
```

Význam:

| Deklarácia | Význam |
|---|---|
| `int *p` | smerník na celé číslo |
| `double *d` | smerník na reálne číslo typu `double` |
| `char *c` | smerník na znak |

Hviezdička `*` znamená, že premenná je smerník.

---

## Adresa premennej: operátor `&`

Operátor `&` vráti adresu premennej.

```c
int x = 10;
int *p = &x;
```

Tu:

- `x` obsahuje hodnotu `10`,
- `&x` je adresa premennej `x`,
- `p` obsahuje adresu premennej `x`.

---

## Dereferencovanie smerníka: operátor `*`

Operátor `*` pri použití so smerníkom znamená prístup k hodnote, na ktorú smerník ukazuje.

```c
#include <stdio.h>

int main(void)
{
    int x = 10;
    int *p = &x;

    printf("Hodnota x: %d\n", x);
    printf("Adresa x: %p\n", (void *)&x);
    printf("Adresa ulozena v p: %p\n", (void *)p);
    printf("Hodnota cez smernik: %d\n", *p);

    return 0;
}
```

Možný výstup:

```text
Hodnota x: 10
Adresa x: 000000000061FE1C
Adresa ulozena v p: 000000000061FE1C
Hodnota cez smernik: 10
```

Adresa sa môže pri každom spustení líšiť.

---

## Zmena hodnoty cez smerník

Cez smerník môžeme meniť hodnotu premennej, na ktorú ukazuje.

```c
#include <stdio.h>

int main(void)
{
    int x = 10;
    int *p = &x;

    *p = 20;

    printf("x = %d\n", x);

    return 0;
}
```

Výstup:

```text
x = 20
```

Smerník `p` ukazuje na premennú `x`, preto zápis `*p = 20;` zmení hodnotu `x`.

---

## Smerník a `NULL`

Ak smerník nikam neukazuje, mal by obsahovať hodnotu `NULL`.

```c
int *p = NULL;
```

Hodnota `NULL` znamená, že smerník neukazuje na platnú adresu.

Pred použitím smerníka je dobré overiť, či nie je `NULL`.

```c
if (p != NULL)
{
    printf("%d\n", *p);
}
```

Dereferencovanie `NULL` smerníka je chyba.

Nesprávne:

```c
int *p = NULL;
printf("%d\n", *p);
```

Tento kód môže spôsobiť pád programu.

---

## 2. Pojem dynamického údaju

Dynamický údaj je údaj, ktorý vzniká počas behu programu v dynamickej pamäti.

Pri bežných premenných sa pamäť prideľuje automaticky.

```c
int x = 10;
```

Táto premenná existuje napríklad len počas vykonávania bloku alebo funkcie, v ktorej bola deklarovaná.

Dynamický údaj sa vytvára pomocou funkcií ako:

```c
malloc()
calloc()
realloc()
```

a ruší sa pomocou:

```c
free()
```

Tieto funkcie sú v knižnici:

```c
#include <stdlib.h>
```

---

## Statická, automatická a dynamická pamäť

V C sa môžeme stretnúť s viacerými druhmi pamäte.

| Druh pamäte | Kedy vzniká | Kedy zaniká | Príklad |
|---|---|---|---|
| statická pamäť | pri štarte programu | pri ukončení programu | globálne premenné, `static` premenné |
| automatická pamäť | pri vstupe do bloku/funkcie | pri opustení bloku/funkcie | lokálne premenné |
| dynamická pamäť | počas behu programu | po zavolaní `free()` | údaje vytvorené cez `malloc()` |

---

## Príklad automatickej premennej

```c
void function(void)
{
    int x = 10;
}
```

Premenná `x` vznikne pri zavolaní funkcie a zanikne po skončení funkcie.

---

## Príklad dynamického údaju

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *p = malloc(sizeof(int));

    if (p == NULL)
    {
        printf("Nepodarilo sa pridelit pamat.\n");
        return 1;
    }

    *p = 42;

    printf("Hodnota: %d\n", *p);

    free(p);
    p = NULL;

    return 0;
}
```

V tomto príklade:

- `malloc(sizeof(int))` pridelí pamäť pre jedno celé číslo,
- `p` obsahuje adresu tejto pamäte,
- `*p = 42;` uloží hodnotu do dynamickej pamäte,
- `free(p);` pamäť uvoľní,
- `p = NULL;` zabráni používaniu neplatného smerníka.

---

## 3. Prideľovanie dynamickej pamäte

Na prideľovanie dynamickej pamäte sa používa hlavne funkcia `malloc()`.

### `malloc()`

Funkcia `malloc()` pridelí požadovaný počet bajtov.

Všeobecný zápis:

```c
smernik = malloc(pocet_bajtov);
```

Príklad:

```c
int *p = malloc(sizeof(int));
```

Ak sa pamäť nepodarí prideliť, `malloc()` vráti `NULL`.

Preto treba výsledok vždy skontrolovať.

```c
if (p == NULL)
{
    printf("Chyba alokacie pamate.\n");
    return 1;
}
```

---

## Prečo používať `sizeof`

Namiesto pevného čísla bajtov je lepšie používať `sizeof`.

Lepšie:

```c
int *p = malloc(sizeof(int));
```

Ešte lepšie:

```c
int *p = malloc(sizeof(*p));
```

Výhoda zápisu `sizeof(*p)` je, že keď sa zmení typ smerníka, nemusíme meniť veľkosť ručne.

---

## Príklad pridelenia pamäte pre jedno číslo

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *number = malloc(sizeof(*number));

    if (number == NULL)
    {
        printf("Nepodarilo sa pridelit pamat.\n");
        return 1;
    }

    *number = 100;

    printf("Cislo: %d\n", *number);

    free(number);
    number = NULL;

    return 0;
}
```

---

## `calloc()`

Funkcia `calloc()` pridelí pamäť pre viac prvkov a nastaví ju na nulu.

Všeobecný zápis:

```c
smernik = calloc(pocet_prvkov, velkost_prvku);
```

Príklad:

```c
int *array = calloc(5, sizeof(int));
```

Týmto vznikne pole 5 celých čísel, ktoré sú inicializované na `0`.

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *array = calloc(5, sizeof(*array));

    if (array == NULL)
    {
        printf("Nepodarilo sa pridelit pamat.\n");
        return 1;
    }

    for (int i = 0; i < 5; i++)
    {
        printf("%d ", array[i]);
    }

    free(array);
    array = NULL;

    return 0;
}
```

Výstup:

```text
0 0 0 0 0
```

---

## `realloc()`

Funkcia `realloc()` mení veľkosť už pridelenej dynamickej pamäte.

Používa sa napríklad vtedy, keď chceme zväčšiť dynamické pole.

```c
int *newArray = realloc(array, newSize * sizeof(*array));
```

Bezpečný zápis:

```c
int *temp = realloc(array, newSize * sizeof(*array));

if (temp == NULL)
{
    printf("Nepodarilo sa zvacsit pamat.\n");
    free(array);
    return 1;
}

array = temp;
```

Dôvod je, že ak `realloc()` zlyhá, pôvodný blok pamäte ostáva platný. Preto je nebezpečné zapisovať výsledok priamo späť do `array`.

Nevhodné:

```c
array = realloc(array, newSize * sizeof(*array));
```

Ak `realloc()` zlyhá, stratíme adresu pôvodnej pamäte a vznikne diera v pamäti.

---

## Príklad použitia `realloc()`

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *array = malloc(3 * sizeof(*array));

    if (array == NULL)
    {
        printf("Nepodarilo sa pridelit pamat.\n");
        return 1;
    }

    for (int i = 0; i < 3; i++)
    {
        array[i] = i + 1;
    }

    int *temp = realloc(array, 5 * sizeof(*array));

    if (temp == NULL)
    {
        printf("Nepodarilo sa zvacsit pamat.\n");
        free(array);
        return 1;
    }

    array = temp;

    array[3] = 4;
    array[4] = 5;

    for (int i = 0; i < 5; i++)
    {
        printf("%d ", array[i]);
    }

    free(array);
    array = NULL;

    return 0;
}
```

Výstup:

```text
1 2 3 4 5
```

---

## 4. Uvoľňovanie dynamickej pamäte

Dynamickú pamäť treba po použití uvoľniť pomocou funkcie `free()`.

```c
free(p);
```

Po zavolaní `free(p)` sa pamäť vráti systému alebo správcovi pamäte.

Dôležité je, že smerník `p` stále obsahuje pôvodnú adresu, ale táto adresa už nie je platná.

Preto je dobré po `free()` nastaviť smerník na `NULL`.

```c
free(p);
p = NULL;
```

---

## Chyba: zabudnuté `free()`

Ak pamäť neuvoľníme, vznikne tzv. diera v pamäti, často nazývaná aj memory leak.

```c
#include <stdlib.h>

int main(void)
{
    int *p = malloc(sizeof(*p));

    if (p == NULL)
    {
        return 1;
    }

    *p = 10;

    return 0;
}
```

Pamäť bola pridelená, ale nikdy nebola uvoľnená.

Správne:

```c
#include <stdlib.h>

int main(void)
{
    int *p = malloc(sizeof(*p));

    if (p == NULL)
    {
        return 1;
    }

    *p = 10;

    free(p);
    p = NULL;

    return 0;
}
```

---

## Chyba: dvojité uvoľnenie pamäte

Pamäť sa nesmie uvoľniť dvakrát.

Nesprávne:

```c
int *p = malloc(sizeof(*p));

free(p);
free(p);
```

Toto môže spôsobiť pád programu alebo poškodenie pamäte.

Bezpečnejšie:

```c
int *p = malloc(sizeof(*p));

free(p);
p = NULL;

free(p);
```

Volanie `free(NULL);` je bezpečné, neurobí nič.

---

## 5. Diery v pamäti

Diera v pamäti, často označovaná ako **memory leak**, vznikne vtedy, keď program pridelí dynamickú pamäť, ale stratí na ňu odkaz alebo ju neuvoľní.

Príklad:

```c
#include <stdlib.h>

int main(void)
{
    int *p = malloc(sizeof(*p));

    p = NULL;

    return 0;
}
```

Tu sa najprv pridelí pamäť, ale potom sa smerník `p` prepíše hodnotou `NULL`.

Tým sa stratí adresa pridelenej pamäte a program ju už nevie uvoľniť.

Správne:

```c
#include <stdlib.h>

int main(void)
{
    int *p = malloc(sizeof(*p));

    if (p == NULL)
    {
        return 1;
    }

    free(p);
    p = NULL;

    return 0;
}
```

---

## Prečo sú diery v pamäti problém

Diery v pamäti spôsobujú, že program postupne spotrebúva stále viac pamäte.

Pri malom programe to nemusí byť viditeľné, ale pri dlhšie bežiacich programoch môže dôjsť k:

- spomaleniu programu,
- vyčerpaniu pamäte,
- pádu programu,
- nestabilite systému.

---

## Typický príklad memory leaku v cykle

```c
#include <stdlib.h>

int main(void)
{
    for (int i = 0; i < 1000000; i++)
    {
        int *p = malloc(sizeof(*p));

        if (p == NULL)
        {
            return 1;
        }

        *p = i;
    }

    return 0;
}
```

V každej iterácii sa pridelí nová pamäť, ale nikdy sa neuvoľní.

Správne:

```c
#include <stdlib.h>

int main(void)
{
    for (int i = 0; i < 1000000; i++)
    {
        int *p = malloc(sizeof(*p));

        if (p == NULL)
        {
            return 1;
        }

        *p = i;

        free(p);
        p = NULL;
    }

    return 0;
}
```

---

## 6. Vystrčené smerníky

Vystrčený smerník, často označovaný ako **dangling pointer**, je smerník, ktorý ukazuje na pamäť, ktorá už nie je platná.

Najčastejšie vznikne po uvoľnení dynamickej pamäte.

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *p = malloc(sizeof(*p));

    if (p == NULL)
    {
        return 1;
    }

    *p = 10;

    free(p);

    printf("%d\n", *p);

    return 0;
}
```

Toto je chyba, pretože po `free(p)` už smerník `p` ukazuje na uvoľnenú pamäť.

Takýto smerník je vystrčený smerník.

---

## Oprava vystrčeného smerníka

Po uvoľnení pamäte nastavíme smerník na `NULL`.

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int *p = malloc(sizeof(*p));

    if (p == NULL)
    {
        return 1;
    }

    *p = 10;

    free(p);
    p = NULL;

    if (p != NULL)
    {
        printf("%d\n", *p);
    }

    return 0;
}
```

Tým znížime riziko náhodného použitia neplatnej adresy.

---

## Vystrčený smerník na lokálnu premennú

Vystrčený smerník môže vzniknúť aj vtedy, keď funkcia vráti adresu lokálnej premennej.

Nesprávne:

```c
int *getNumber(void)
{
    int x = 10;

    return &x;
}
```

Premenná `x` zanikne po skončení funkcie, takže vrátená adresa už nie je platná.

Správnejšie je použiť dynamickú pamäť:

```c
#include <stdlib.h>

int *getNumber(void)
{
    int *x = malloc(sizeof(*x));

    if (x == NULL)
    {
        return NULL;
    }

    *x = 10;

    return x;
}
```

Použitie:

```c
#include <stdio.h>
#include <stdlib.h>

int *getNumber(void);

int main(void)
{
    int *number = getNumber();

    if (number == NULL)
    {
        return 1;
    }

    printf("%d\n", *number);

    free(number);
    number = NULL;

    return 0;
}
```

---

## 7. Statické pole

Statické pole má veľkosť určenú dopredu pri deklarácii.

```c
int numbers[5];
```

Príklad:

```c
#include <stdio.h>

int main(void)
{
    int numbers[5] = {1, 2, 3, 4, 5};

    for (int i = 0; i < 5; i++)
    {
        printf("%d ", numbers[i]);
    }

    return 0;
}
```

Výstup:

```text
1 2 3 4 5
```

---

## Vlastnosti statického poľa

Statické pole:

- má pevnú veľkosť,
- veľkosť sa nedá počas behu programu meniť,
- pamäť sa spravuje automaticky,
- nie je potrebné volať `free()`.

Príklad:

```c
int array[10];
```

Takéto pole bude mať vždy 10 prvkov.

---

## Nevýhody statického poľa

Nevýhody:

- veľkosť musí byť známa dopredu,
- môže byť zbytočne veľké,
- môže byť príliš malé,
- nedá sa jednoducho zväčšiť alebo zmenšiť počas behu programu.

---

## 8. Dynamické pole

Dynamické pole je pole, ktorého veľkosť sa určí počas behu programu.

Vytvára sa pomocou dynamickej pamäte.

Príklad:

```c
int *array = malloc(n * sizeof(*array));
```

kde `n` je počet prvkov.

---

## Príklad dynamického poľa

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int n;

    printf("Zadaj pocet prvkov: ");
    scanf("%d", &n);

    if (n <= 0)
    {
        printf("Neplatny pocet prvkov.\n");
        return 1;
    }

    int *array = malloc(n * sizeof(*array));

    if (array == NULL)
    {
        printf("Nepodarilo sa pridelit pamat.\n");
        return 1;
    }

    for (int i = 0; i < n; i++)
    {
        array[i] = i + 1;
    }

    for (int i = 0; i < n; i++)
    {
        printf("%d ", array[i]);
    }

    free(array);
    array = NULL;

    return 0;
}
```

Ak používateľ zadá `5`, výstup bude:

```text
1 2 3 4 5
```

---

## Zväčšenie dynamického poľa

Dynamické pole možno zväčšiť pomocou `realloc()`.

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int size = 3;
    int *array = malloc(size * sizeof(*array));

    if (array == NULL)
    {
        return 1;
    }

    for (int i = 0; i < size; i++)
    {
        array[i] = i + 1;
    }

    int newSize = 5;
    int *temp = realloc(array, newSize * sizeof(*array));

    if (temp == NULL)
    {
        free(array);
        return 1;
    }

    array = temp;

    for (int i = size; i < newSize; i++)
    {
        array[i] = i + 1;
    }

    for (int i = 0; i < newSize; i++)
    {
        printf("%d ", array[i]);
    }

    free(array);
    array = NULL;

    return 0;
}
```

Výstup:

```text
1 2 3 4 5
```

---

## Porovnanie statického a dynamického poľa

| Vlastnosť | Statické pole | Dynamické pole |
|---|---|---|
| Veľkosť | pevná | určená počas behu programu |
| Zmena veľkosti | nie | áno, pomocou `realloc()` |
| Správa pamäte | automatická | ručná cez `malloc()` a `free()` |
| Riziko memory leaku | nízke | vyššie |
| Použitie | keď poznáme veľkosť dopredu | keď veľkosť nepoznáme dopredu |

---

## 9. Smerníky a polia

Názov poľa sa často správa ako smerník na prvý prvok poľa.

```c
int numbers[3] = {10, 20, 30};

printf("%d\n", numbers[0]);
printf("%d\n", *numbers);
```

Oba výpisy vypíšu hodnotu `10`.

---

## Aritmetika smerníkov

Smerník možno posúvať po prvkoch poľa.

```c
#include <stdio.h>

int main(void)
{
    int numbers[3] = {10, 20, 30};
    int *p = numbers;

    printf("%d\n", *p);
    printf("%d\n", *(p + 1));
    printf("%d\n", *(p + 2));

    return 0;
}
```

Výstup:

```text
10
20
30
```

Výraz `p + 1` neznamená posun o 1 bajt, ale posun o jeden prvok typu `int`.

---

## 10. Najčastejšie chyby pri smerníkoch

### Použitie neinicializovaného smerníka

Nesprávne:

```c
int *p;
*p = 10;
```

Smerník `p` obsahuje náhodnú adresu.

Správne:

```c
int x;
int *p = &x;

*p = 10;
```

alebo:

```c
int *p = malloc(sizeof(*p));

if (p != NULL)
{
    *p = 10;
    free(p);
    p = NULL;
}
```

---

### Zabudnuté uvoľnenie pamäte

Nesprávne:

```c
int *p = malloc(sizeof(*p));
*p = 10;
```

Správne:

```c
int *p = malloc(sizeof(*p));

if (p != NULL)
{
    *p = 10;
    free(p);
    p = NULL;
}
```

---

### Použitie pamäte po `free()`

Nesprávne:

```c
free(p);
printf("%d\n", *p);
```

Správne:

```c
free(p);
p = NULL;
```

---

### Prekročenie hranice poľa

Nesprávne:

```c
int array[3] = {1, 2, 3};

array[3] = 4;
```

Indexy sú od `0`, preto platné indexy sú:

```text
0, 1, 2
```

Správne:

```c
int array[3] = {1, 2, 3};

array[2] = 4;
```

---

## Súhrnný príklad

Program načíta počet prvkov, vytvorí dynamické pole, načíta čísla, vypočíta súčet a pamäť uvoľní.

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{
    int n;
    int sum = 0;

    printf("Zadaj pocet cisel: ");
    scanf("%d", &n);

    if (n <= 0)
    {
        printf("Neplatny pocet cisel.\n");
        return 1;
    }

    int *numbers = malloc(n * sizeof(*numbers));

    if (numbers == NULL)
    {
        printf("Nepodarilo sa pridelit pamat.\n");
        return 1;
    }

    for (int i = 0; i < n; i++)
    {
        printf("Zadaj cislo %d: ", i + 1);
        scanf("%d", &numbers[i]);
    }

    for (int i = 0; i < n; i++)
    {
        sum += numbers[i];
    }

    printf("Sucet: %d\n", sum);

    free(numbers);
    numbers = NULL;

    return 0;
}
```

Možný beh programu:

```text
Zadaj pocet cisel: 3
Zadaj cislo 1: 10
Zadaj cislo 2: 20
Zadaj cislo 3: 30
Sucet: 60
```

---

## Krátke zhrnutie

| Pojem | Význam |
|---|---|
| smerník | premenná obsahujúca adresu |
| `&` | operátor získania adresy |
| `*` | dereferencovanie smerníka |
| `NULL` | smerník nikam neukazuje |
| dynamický údaj | údaj vytvorený počas behu programu |
| `malloc()` | pridelí dynamickú pamäť |
| `calloc()` | pridelí a vynuluje dynamickú pamäť |
| `realloc()` | zmení veľkosť pridelenej pamäte |
| `free()` | uvoľní dynamickú pamäť |
| diera v pamäti | pridelená, ale neuvoľnená pamäť |
| vystrčený smerník | smerník na neplatnú alebo uvoľnenú pamäť |
| statické pole | pole s pevnou veľkosťou |
| dynamické pole | pole s veľkosťou určenou počas behu programu |

---

## Mini odpoveď vhodná na skúšku

Smerník v jazyku C je premenná, ktorá obsahuje adresu inej premennej alebo dynamicky pridelenej pamäte. Deklaruje sa pomocou hviezdičky, napríklad `int *p`. Operátor `&` slúži na získanie adresy premennej a operátor `*` na prístup k hodnote, na ktorú smerník ukazuje. Dynamický údaj vzniká počas behu programu v dynamickej pamäti. Pamäť sa prideľuje funkciami `malloc()`, `calloc()` alebo `realloc()` z knižnice `stdlib.h` a uvoľňuje sa funkciou `free()`. Ak program pridelí pamäť a neuvoľní ju alebo stratí jej adresu, vzniká diera v pamäti, teda memory leak. Vystrčený smerník je smerník, ktorý ukazuje na pamäť, ktorá už nie je platná, napríklad po zavolaní `free()`. Statické pole má veľkosť určenú pri deklarácii a nedá sa meniť počas behu programu. Dynamické pole sa vytvára pomocou dynamickej pamäte a jeho veľkosť možno určiť alebo meniť počas behu programu.
