# Binárne a logické operátory v jazyku C

## Úvod

V jazyku **C** treba rozlišovať medzi:

- **logickými operátormi**,
- **bitovými operátormi**.

Na prvý pohľad vyzerajú podobne, ale robia rozdielne veci.

Napríklad:

```c
&&
&
```

nie je to isté.

A tiež:

```c
||
|
```

nie je to isté.

Logické operátory pracujú s pravdivostnou hodnotou výrazu.

Bitové operátory pracujú priamo s jednotlivými bitmi čísla.

---

## 1. Pravdivostné hodnoty v C

V jazyku C platí:

| Hodnota | Význam |
|---:|---|
| `0` | nepravda |
| nenulová hodnota | pravda |

Príklad:

```c
#include <stdio.h>

int main(void)
{
    int a = 0;
    int b = 5;

    printf("%d\n", a != 0);
    printf("%d\n", b != 0);

    return 0;
}
```

Výstup:

```text
0
1
```

Hodnota `0` je nepravda, hodnota `5` je pravda.

---

## 2. Logické operátory

Logické operátory sa používajú najmä v podmienkach.

| Operátor | Názov | Význam |
|---|---|---|
| `&&` | logické AND | pravda, ak sú oba výrazy pravdivé |
| `||` | logické OR | pravda, ak je aspoň jeden výraz pravdivý |
| `!` | logické NOT | negácia pravdivostnej hodnoty |

Výsledkom logickej operácie je hodnota:

```text
0 alebo 1
```

---

## Logické AND: `&&`

Logické AND je pravdivé iba vtedy, keď sú pravdivé obe podmienky.

| A | B | A `&&` B |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

Príklad:

```c
#include <stdio.h>

int main(void)
{
    int age = 20;
    int hasTicket = 1;

    if (age >= 18 && hasTicket)
    {
        printf("Vstup povoleny.\n");
    }
    else
    {
        printf("Vstup zamietnuty.\n");
    }

    return 0;
}
```

Výstup:

```text
Vstup povoleny.
```

Podmienka je pravdivá, pretože používateľ má aspoň 18 rokov a zároveň má lístok.

---

## Logické OR: `||`

Logické OR je pravdivé vtedy, keď je pravdivá aspoň jedna podmienka.

| A | B | A `||` B |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

Príklad:

```c
#include <stdio.h>

int main(void)
{
    int isAdmin = 0;
    int isOwner = 1;

    if (isAdmin || isOwner)
    {
        printf("Pristup povoleny.\n");
    }
    else
    {
        printf("Pristup zamietnuty.\n");
    }

    return 0;
}
```

Výstup:

```text
Pristup povoleny.
```

Stačí, že používateľ je vlastník.

---

## Logické NOT: `!`

Logické NOT otočí pravdivostnú hodnotu.

| A | `!A` |
|---:|---:|
| 0 | 1 |
| 1 | 0 |

Príklad:

```c
#include <stdio.h>

int main(void)
{
    int isLoggedIn = 0;

    if (!isLoggedIn)
    {
        printf("Pouzivatel nie je prihlaseny.\n");
    }

    return 0;
}
```

Výstup:

```text
Pouzivatel nie je prihlaseny.
```

---

## Skrátené vyhodnocovanie logických operátorov

Logické operátory `&&` a `||` používajú tzv. **short-circuit evaluation**, teda skrátené vyhodnocovanie.

Pri `&&` platí:

Ak je prvá podmienka nepravdivá, druhá sa už nemusí vyhodnocovať.

```c
if (p != NULL && *p > 0)
{
    printf("Platna hodnota.\n");
}
```

Toto je bezpečné, pretože `*p > 0` sa vyhodnotí len vtedy, keď `p != NULL`.

Pri `||` platí:

Ak je prvá podmienka pravdivá, druhá sa už nemusí vyhodnocovať.

```c
if (isAdmin || hasSpecialPermission())
{
    printf("Pristup povoleny.\n");
}
```

Ak je `isAdmin` pravda, funkcia `hasSpecialPermission()` sa už nemusí volať.

---

## 3. Bitové operátory

Bitové operátory pracujú s jednotlivými bitmi čísla.

| Operátor | Názov | Význam |
|---|---|---|
| `&` | bitové AND | bit je 1, ak sú oba bity 1 |
| `|` | bitové OR | bit je 1, ak je aspoň jeden bit 1 |
| `^` | bitové XOR | bit je 1, ak sú bity rozdielne |
| `~` | bitové NOT | otočí všetky bity |
| `<<` | posun doľava | posunie bity doľava |
| `>>` | posun doprava | posunie bity doprava |

---

## Binárny zápis

Pri bitových operáciách je vhodné predstaviť si číslo v dvojkovej sústave.

Napríklad:

```text
5  = 00000101
3  = 00000011
```

---

## Bitové AND: `&`

Bitové AND porovnáva bity dvoch čísel.

Výsledný bit je `1` iba vtedy, keď sú oba príslušné bity `1`.

| A | B | A `&` B |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 0 |
| 1 | 0 | 0 |
| 1 | 1 | 1 |

Príklad:

```text
5      = 00000101
3      = 00000011
5 & 3  = 00000001
```

Výsledok je `1`.

Program:

```c
#include <stdio.h>

int main(void)
{
    int a = 5;
    int b = 3;

    printf("%d\n", a & b);

    return 0;
}
```

Výstup:

```text
1
```

---

## Bitové OR: `|`

Bitové OR nastaví výsledný bit na `1`, ak je aspoň jeden z bitov `1`.

| A | B | A `|` B |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 1 |

Príklad:

```text
5      = 00000101
3      = 00000011
5 | 3  = 00000111
```

Výsledok je `7`.

Program:

```c
#include <stdio.h>

int main(void)
{
    int a = 5;
    int b = 3;

    printf("%d\n", a | b);

    return 0;
}
```

Výstup:

```text
7
```

---

## Bitové XOR: `^`

Bitové XOR nastaví výsledný bit na `1`, ak sú bity rozdielne.

| A | B | A `^` B |
|---:|---:|---:|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

Príklad:

```text
5      = 00000101
3      = 00000011
5 ^ 3  = 00000110
```

Výsledok je `6`.

Program:

```c
#include <stdio.h>

int main(void)
{
    int a = 5;
    int b = 3;

    printf("%d\n", a ^ b);

    return 0;
}
```

Výstup:

```text
6
```

---

## Bitové NOT: `~`

Bitové NOT otočí všetky bity čísla.

```text
0 sa zmení na 1
1 sa zmení na 0
```

Príklad na 8 bitoch:

```text
5      = 00000101
~5     = 11111010
```

V C však výsledok závisí od veľkosti typu a reprezentácie záporných čísel.

Program:

```c
#include <stdio.h>

int main(void)
{
    int a = 5;

    printf("%d\n", ~a);

    return 0;
}
```

Typický výstup:

```text
-6
```

Pretože pri dvojkovom doplnku platí:

```text
~x = -x - 1
```

Teda:

```text
~5 = -6
```

---

## Rozdiel medzi logickým NOT a bitovým NOT

| Operátor | Typ | Význam |
|---|---|---|
| `!` | logický NOT | pracuje s pravdivosťou výrazu |
| `~` | bitový NOT | otočí všetky bity čísla |

Príklad:

```c
#include <stdio.h>

int main(void)
{
    int x = 5;

    printf("!x = %d\n", !x);
    printf("~x = %d\n", ~x);

    return 0;
}
```

Typický výstup:

```text
!x = 0
~x = -6
```

`!5` je `0`, pretože `5` je pravdivá hodnota.

`~5` otočí všetky bity čísla.

---

## 4. Rozdiel medzi logickými a bitovými operátormi

### `&&` verzus `&`

```c
#include <stdio.h>

int main(void)
{
    int a = 5;
    int b = 3;

    printf("a && b = %d\n", a && b);
    printf("a & b = %d\n", a & b);

    return 0;
}
```

Výstup:

```text
a && b = 1
a & b = 1
```

Tu náhodou vyzerá výsledok rovnako, ale význam je odlišný:

- `a && b` zisťuje, či sú obe hodnoty pravdivé,
- `a & b` robí bitový súčin.

Lepší príklad:

```c
#include <stdio.h>

int main(void)
{
    int a = 2;
    int b = 4;

    printf("a && b = %d\n", a && b);
    printf("a & b = %d\n", a & b);

    return 0;
}
```

Výstup:

```text
a && b = 1
a & b = 0
```

Vysvetlenie:

```text
2      = 00000010
4      = 00000100
2 & 4  = 00000000
```

Ale `2 && 4` je pravda, pretože obe hodnoty sú nenulové.

---

### `||` verzus `|`

```c
#include <stdio.h>

int main(void)
{
    int a = 2;
    int b = 4;

    printf("a || b = %d\n", a || b);
    printf("a | b = %d\n", a | b);

    return 0;
}
```

Výstup:

```text
a || b = 1
a | b = 6
```

Vysvetlenie:

```text
2      = 00000010
4      = 00000100
2 | 4  = 00000110
```

Binárne `00000110` je desiatkovo `6`.

---

### `!` verzus `~`

```c
#include <stdio.h>

int main(void)
{
    int x = 0;
    int y = 5;

    printf("!x = %d\n", !x);
    printf("!y = %d\n", !y);
    printf("~x = %d\n", ~x);
    printf("~y = %d\n", ~y);

    return 0;
}
```

Typický výstup:

```text
!x = 1
!y = 0
~x = -1
~y = -6
```

---

## 5. Bitové posuvy

Bitové posuvy posúvajú bity čísla doľava alebo doprava.

| Operátor | Význam |
|---|---|
| `<<` | posun bitov doľava |
| `>>` | posun bitov doprava |

---

## Posun doľava: `<<`

Posun doľava posunie bity o zadaný počet miest doľava.

Príklad:

```text
5       = 00000101
5 << 1  = 00001010
```

Výsledok je `10`.

Program:

```c
#include <stdio.h>

int main(void)
{
    int x = 5;

    printf("%d\n", x << 1);

    return 0;
}
```

Výstup:

```text
10
```

Pri kladných celých číslach platí, že posun doľava o `n` bitov často zodpovedá násobeniu číslom `2^n`.

```text
x << 1  približne x * 2
x << 2  približne x * 4
x << 3  približne x * 8
```

Príklad:

```c
#include <stdio.h>

int main(void)
{
    int x = 3;

    printf("%d\n", x << 1);
    printf("%d\n", x << 2);
    printf("%d\n", x << 3);

    return 0;
}
```

Výstup:

```text
6
12
24
```

---

## Posun doprava: `>>`

Posun doprava posunie bity o zadaný počet miest doprava.

Príklad:

```text
20      = 00010100
20 >> 1 = 00001010
```

Výsledok je `10`.

Program:

```c
#include <stdio.h>

int main(void)
{
    int x = 20;

    printf("%d\n", x >> 1);

    return 0;
}
```

Výstup:

```text
10
```

Pri nezáporných celých číslach posun doprava o `n` bitov zodpovedá celočíselnému deleniu číslom `2^n`.

```text
x >> 1  približne x / 2
x >> 2  približne x / 4
x >> 3  približne x / 8
```

Príklad:

```c
#include <stdio.h>

int main(void)
{
    int x = 32;

    printf("%d\n", x >> 1);
    printf("%d\n", x >> 2);
    printf("%d\n", x >> 3);

    return 0;
}
```

Výstup:

```text
16
8
4
```

---

## Pozor na posuvy

Pri bitových posuvoch si treba dávať pozor:

- neposúvať o záporný počet bitov,
- neposúvať o počet bitov väčší alebo rovný veľkosti typu,
- pri signed typoch môže byť správanie problémovejšie,
- pri bitových operáciách je často vhodnejšie používať `unsigned` typy.

Lepšie:

```c
unsigned int x = 1u;
unsigned int mask = x << 3;
```

---

## 6. Bitové masky

Bitová maska je hodnota, ktorá má nastavené určité bity a používa sa na testovanie, nastavovanie, rušenie alebo prepínanie bitov.

Napríklad:

```text
00001000
```

je maska pre bit číslo `3`.

Bity zvyčajne číslujeme od nuly sprava:

```text
bit:  7 6 5 4 3 2 1 0
data: 0 0 0 0 1 0 0 0
```

---

## Vytvorenie masky

Masku pre bit `n` vytvoríme takto:

```c
1u << n
```

Príklad:

```c
unsigned int mask = 1u << 3;
```

Výsledok:

```text
00001000
```

---

## Testovanie bitu

Na testovanie, či je bit nastavený, použijeme bitové AND.

```c
if (flags & mask)
{
    printf("Bit je nastaveny.\n");
}
```

Príklad:

```c
#include <stdio.h>

int main(void)
{
    unsigned int flags = 0b00001000;
    unsigned int mask = 1u << 3;

    if (flags & mask)
    {
        printf("Bit 3 je nastaveny.\n");
    }

    return 0;
}
```

Výstup:

```text
Bit 3 je nastaveny.
```

Poznámka: zápis `0b00001000` podporujú mnohé moderné kompilátory, ale v staršom štandarde C nemusí byť prenositeľný. Prenositeľnejší zápis je napríklad:

```c
unsigned int flags = 8u;
```

---

## Nastavenie bitu

Na nastavenie bitu použijeme bitové OR.

```c
flags = flags | mask;
```

Skrátene:

```c
flags |= mask;
```

Príklad:

```c
#include <stdio.h>

int main(void)
{
    unsigned int flags = 0u;

    flags |= 1u << 2;

    printf("%u\n", flags);

    return 0;
}
```

Výstup:

```text
4
```

Bit číslo `2` zodpovedá hodnote:

```text
2^2 = 4
```

---

## Vynulovanie bitu

Na vynulovanie bitu použijeme AND s negovanou maskou.

```c
flags = flags & ~mask;
```

Skrátene:

```c
flags &= ~mask;
```

Príklad:

```c
#include <stdio.h>

int main(void)
{
    unsigned int flags = 7u;

    flags &= ~(1u << 1);

    printf("%u\n", flags);

    return 0;
}
```

Vysvetlenie:

```text
7 = 00000111
```

Vynulujeme bit číslo `1`.

Výsledok:

```text
5 = 00000101
```

Výstup:

```text
5
```

---

## Prepnutie bitu

Prepnutie bitu znamená:

- ak bol bit `0`, zmení sa na `1`,
- ak bol bit `1`, zmení sa na `0`.

Používa sa XOR.

```c
flags ^= mask;
```

Príklad:

```c
#include <stdio.h>

int main(void)
{
    unsigned int flags = 0u;

    flags ^= 1u << 0;
    printf("%u\n", flags);

    flags ^= 1u << 0;
    printf("%u\n", flags);

    return 0;
}
```

Výstup:

```text
1
0
```

---

## 7. Bitové makrá

Bitové makrá sú pomocné makrá, ktoré zjednodušujú prácu s bitmi.

Používajú sa najmä pri príznakoch, nastaveniach, registroch alebo nízkoúrovňovom programovaní.

---

## Makro pre vytvorenie bitu

```c
#define BIT(n) (1u << (n))
```

Použitie:

```c
unsigned int mask = BIT(3);
```

To znamená:

```c
unsigned int mask = 1u << 3;
```

---

## Makrá na nastavenie, zrušenie, prepnutie a test bitu

```c
#define BIT(n)          (1u << (n))
#define SET_BIT(x, n)   ((x) |= BIT(n))
#define CLEAR_BIT(x, n) ((x) &= ~BIT(n))
#define TOGGLE_BIT(x, n) ((x) ^= BIT(n))
#define CHECK_BIT(x, n) (((x) & BIT(n)) != 0u)
```

---

## Príklad použitia bitových makier

```c
#include <stdio.h>

#define BIT(n)           (1u << (n))
#define SET_BIT(x, n)    ((x) |= BIT(n))
#define CLEAR_BIT(x, n)  ((x) &= ~BIT(n))
#define TOGGLE_BIT(x, n) ((x) ^= BIT(n))
#define CHECK_BIT(x, n)  (((x) & BIT(n)) != 0u)

int main(void)
{
    unsigned int flags = 0u;

    SET_BIT(flags, 2);

    if (CHECK_BIT(flags, 2))
    {
        printf("Bit 2 je nastaveny.\n");
    }

    TOGGLE_BIT(flags, 2);

    if (!CHECK_BIT(flags, 2))
    {
        printf("Bit 2 uz nie je nastaveny.\n");
    }

    SET_BIT(flags, 1);
    CLEAR_BIT(flags, 1);

    printf("flags = %u\n", flags);

    return 0;
}
```

Výstup:

```text
Bit 2 je nastaveny.
Bit 2 uz nie je nastaveny.
flags = 0
```

---

## Prečo sa v makrách používajú zátvorky

V makrách treba používať zátvorky, aby sa predišlo chybám s prioritou operátorov.

Lepšie:

```c
#define BIT(n) (1u << (n))
```

Horšie:

```c
#define BIT(n) 1u << n
```

Pri komplikovanejších výrazoch by bez zátvoriek mohol vzniknúť nesprávny výsledok.

---

## 8. Príznaky pomocou bitov

Bitové operácie sa často používajú na ukladanie viacerých logických nastavení do jedného čísla.

Príklad:

```c
#define PERMISSION_READ  BIT(0)
#define PERMISSION_WRITE BIT(1)
#define PERMISSION_EXEC  BIT(2)
```

Každý bit reprezentuje jedno povolenie.

---

## Príklad s príznakmi

```c
#include <stdio.h>

#define BIT(n)            (1u << (n))
#define PERMISSION_READ   BIT(0)
#define PERMISSION_WRITE  BIT(1)
#define PERMISSION_EXEC   BIT(2)

int main(void)
{
    unsigned int permissions = 0u;

    permissions |= PERMISSION_READ;
    permissions |= PERMISSION_WRITE;

    if (permissions & PERMISSION_READ)
    {
        printf("Pouzivatel moze citat.\n");
    }

    if (permissions & PERMISSION_WRITE)
    {
        printf("Pouzivatel moze zapisovat.\n");
    }

    if (!(permissions & PERMISSION_EXEC))
    {
        printf("Pouzivatel nemoze spustat.\n");
    }

    return 0;
}
```

Výstup:

```text
Pouzivatel moze citat.
Pouzivatel moze zapisovat.
Pouzivatel nemoze spustat.
```

---

## 9. Priority operátorov

Niektoré bitové a logické operátory majú rozdielnu prioritu.

Zjednodušené poradie od vyššej priority po nižšiu:

| Priorita | Operátory |
|---:|---|
| 1 | `!`, `~` |
| 2 | `*`, `/`, `%` |
| 3 | `+`, `-` |
| 4 | `<<`, `>>` |
| 5 | `<`, `<=`, `>`, `>=` |
| 6 | `==`, `!=` |
| 7 | `&` |
| 8 | `^` |
| 9 | `|` |
| 10 | `&&` |
| 11 | `||` |
| 12 | `=`, `+=`, `-=`, `&=`, `|=`, `^=`, `<<=`, `>>=` |

Pri bitových výrazoch je rozumné používať zátvorky.

Príklad:

```c
if ((flags & BIT(2)) != 0u)
{
    printf("Bit je nastaveny.\n");
}
```

Takýto zápis je čitateľnejší a bezpečnejší.

---

## 10. Najčastejšie chyby

### Zámena `&&` a `&`

Nesprávne pri podmienke:

```c
if (a > 0 & b > 0)
{
    printf("OK\n");
}
```

Správne:

```c
if (a > 0 && b > 0)
{
    printf("OK\n");
}
```

Použitie `&` môže fungovať náhodou, ale nejde o logické AND a nemá skrátené vyhodnocovanie.

---

### Zámena `||` a `|`

Nesprávne pri podmienke:

```c
if (isAdmin | isOwner)
{
    printf("Pristup povoleny.\n");
}
```

Správne:

```c
if (isAdmin || isOwner)
{
    printf("Pristup povoleny.\n");
}
```

---

### Zámena `!` a `~`

```c
!x
```

znamená logickú negáciu.

```c
~x
```

znamená bitovú negáciu.

Nie sú zameniteľné.

---

### Posun o príliš veľa bitov

Nesprávne:

```c
unsigned int x = 1u;
unsigned int y = x << 32;
```

Ak má `unsigned int` 32 bitov, posun o 32 je problémový.

Lepšie je kontrolovať rozsah:

```c
if (n < sizeof(unsigned int) * 8)
{
    unsigned int mask = 1u << n;
}
```

---

## Súhrnný príklad

```c
#include <stdio.h>

#define BIT(n)           (1u << (n))
#define SET_BIT(x, n)    ((x) |= BIT(n))
#define CLEAR_BIT(x, n)  ((x) &= ~BIT(n))
#define TOGGLE_BIT(x, n) ((x) ^= BIT(n))
#define CHECK_BIT(x, n)  (((x) & BIT(n)) != 0u)

int main(void)
{
    unsigned int flags = 0u;
    int age = 20;
    int hasTicket = 1;

    if (age >= 18 && hasTicket)
    {
        printf("Logicke AND: vstup povoleny.\n");
    }

    SET_BIT(flags, 0);
    SET_BIT(flags, 2);

    if (CHECK_BIT(flags, 0))
    {
        printf("Bit 0 je nastaveny.\n");
    }

    if (CHECK_BIT(flags, 2))
    {
        printf("Bit 2 je nastaveny.\n");
    }

    CLEAR_BIT(flags, 0);
    TOGGLE_BIT(flags, 2);

    printf("flags = %u\n", flags);

    printf("4 << 1 = %u\n", 4u << 1);
    printf("8 >> 1 = %u\n", 8u >> 1);

    return 0;
}
```

Možný výstup:

```text
Logicke AND: vstup povoleny.
Bit 0 je nastaveny.
Bit 2 je nastaveny.
flags = 0
4 << 1 = 8
8 >> 1 = 4
```

---

## Krátke zhrnutie

| Pojem | Význam |
|---|---|
| `&&` | logické AND |
| `||` | logické OR |
| `!` | logické NOT |
| `&` | bitové AND |
| `|` | bitové OR |
| `^` | bitové XOR |
| `~` | bitové NOT |
| `<<` | bitový posun doľava |
| `>>` | bitový posun doprava |
| `1u << n` | maska pre bit číslo `n` |
| `flags |= mask` | nastavenie bitu |
| `flags &= ~mask` | vynulovanie bitu |
| `flags ^= mask` | prepnutie bitu |
| `flags & mask` | testovanie bitu |
| `BIT(n)` | typické makro na vytvorenie bitovej masky |

---

## Mini odpoveď vhodná na skúšku

V jazyku C treba rozlišovať logické a bitové operátory. Logické operátory `&&`, `||` a `!` pracujú s pravdivostnou hodnotou výrazov, pričom `0` znamená nepravdu a nenulová hodnota pravdu. Výsledkom logických operácií je zvyčajne `0` alebo `1`. Bitové operátory `&`, `|`, `^` a `~` pracujú priamo s jednotlivými bitmi čísla. Bitové AND nastaví bit na `1` iba vtedy, keď sú oba príslušné bity `1`, bitové OR nastaví bit na `1`, ak je aspoň jeden bit `1`, XOR nastaví bit na `1`, ak sú bity rozdielne, a bitové NOT otočí všetky bity. Operátory `<<` a `>>` slúžia na posun bitov doľava a doprava. Posun doľava pri nezáporných číslach zodpovedá násobeniu mocninou dvojky a posun doprava celočíselnému deleniu mocninou dvojky. Bitové makrá, napríklad `BIT(n)`, `SET_BIT`, `CLEAR_BIT`, `TOGGLE_BIT` a `CHECK_BIT`, sa používajú na jednoduchšiu prácu s bitovými príznakmi.
