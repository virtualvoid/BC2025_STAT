# Reťazce a súbory v jazyku C

## Úvod

V jazyku **C** sa pri práci s textom a údajmi často používajú:

- vstup z klávesnice,
- výstup na monitor,
- reťazce,
- súbory,
- knižnice.

Tieto oblasti patria medzi základné časti programovania v C, pretože program zvyčajne potrebuje komunikovať s používateľom, spracovať textové údaje alebo ukladať a načítavať dáta zo súborov.

---

## 1. Vstup z klávesnice a výstup na monitor

Na vstup a výstup sa v jazyku C najčastejšie používa štandardná knižnica:

```c
#include <stdio.h>
```

Táto knižnica obsahuje funkcie ako:

| Funkcia | Význam |
|---|---|
| `printf()` | výstup na obrazovku |
| `scanf()` | vstup z klávesnice |
| `getchar()` | načítanie jedného znaku |
| `putchar()` | výpis jedného znaku |
| `fgets()` | načítanie riadku textu |

---

## Výstup na monitor pomocou `printf()`

Funkcia `printf()` slúži na vypísanie textu alebo hodnoty premennej na obrazovku.

Príklad:

```c
#include <stdio.h>

int main(void)
{
    printf("Ahoj svet!\n");

    return 0;
}
```

Výstup:

```text
Ahoj svet!
```

Znak `\n` znamená prechod na nový riadok.

---

## Formátovaný výstup

Pri výpise hodnôt premenných sa používajú formátovacie znaky.

| Formát | Význam |
|---|---|
| `%d` | celé číslo typu `int` |
| `%u` | nezáporné celé číslo |
| `%f` | reálne číslo |
| `%lf` | reálne číslo typu `double` pri `scanf()` |
| `%c` | znak |
| `%s` | reťazec |

Príklad:

```c
#include <stdio.h>

int main(void)
{
    int age = 20;
    double height = 180.5;
    char grade = 'A';

    printf("Vek: %d\n", age);
    printf("Vyska: %.1f cm\n", height);
    printf("Znamka: %c\n", grade);

    return 0;
}
```

Možný výstup:

```text
Vek: 20
Vyska: 180.5 cm
Znamka: A
```

---

## Vstup z klávesnice pomocou `scanf()`

Funkcia `scanf()` slúži na načítanie údajov z klávesnice.

Príklad:

```c
#include <stdio.h>

int main(void)
{
    int age;

    printf("Zadaj vek: ");
    scanf("%d", &age);

    printf("Tvoj vek je: %d\n", age);

    return 0;
}
```

Dôležité je použiť operátor `&`, ktorý znamená adresu premennej.

```c
scanf("%d", &age);
```

Bez `&` by program nevedel, kam má načítanú hodnotu uložiť.

---

## Načítanie viacerých hodnôt

```c
#include <stdio.h>

int main(void)
{
    int a;
    int b;

    printf("Zadaj dve cele cisla: ");
    scanf("%d %d", &a, &b);

    printf("Sucet je: %d\n", a + b);

    return 0;
}
```

---

## Načítanie znaku

Na načítanie jedného znaku možno použiť `scanf()` alebo `getchar()`.

Príklad so `scanf()`:

```c
#include <stdio.h>

int main(void)
{
    char c;

    printf("Zadaj znak: ");
    scanf(" %c", &c);

    printf("Zadal si znak: %c\n", c);

    return 0;
}
```

Medzera pred `%c` v `scanf(" %c", &c);` zabezpečí, že sa preskočia biele znaky, napríklad nový riadok.

---

## 2. Reťazce v jazyku C

Reťazec je postupnosť znakov.

V C neexistuje samostatný dátový typ `string` ako napríklad v C++ alebo C#. Reťazec sa zapisuje ako pole znakov:

```c
char name[20];
```

Reťazec je ukončený špeciálnym znakom:

```text
\0
```

Tento znak sa nazýva nulový znak alebo ukončovací znak reťazca.

---

## Reťazec ako pole znakov

Príklad:

```c
char text[] = "Ahoj";
```

V pamäti je uložený približne takto:

| Index | Hodnota |
|---:|---|
| 0 | `A` |
| 1 | `h` |
| 2 | `o` |
| 3 | `j` |
| 4 | `\0` |

Aj keď slovo `"Ahoj"` má 4 znaky, pole potrebuje 5 miest, pretože posledné miesto je pre znak `\0`.

---

## Načítanie reťazca pomocou `scanf()`

```c
#include <stdio.h>

int main(void)
{
    char name[30];

    printf("Zadaj meno: ");
    scanf("%29s", name);

    printf("Ahoj, %s!\n", name);

    return 0;
}
```

Pri poli znakov sa pri `scanf()` nepíše `&`, pretože názov poľa už predstavuje adresu začiatku poľa.

Tento zápis:

```c
scanf("%29s", name);
```

načíta maximálne 29 znakov a posledné miesto nechá pre `\0`.

Nevýhoda `scanf("%s", ...)` je, že načíta iba text po prvú medzeru.

Ak používateľ zadá:

```text
Juraj Suchan
```

do premennej sa uloží iba:

```text
Juraj
```

---

## Načítanie celého riadku pomocou `fgets()`

Na načítanie textu aj s medzerami je vhodnejšie použiť `fgets()`.

```c
#include <stdio.h>

int main(void)
{
    char fullName[50];

    printf("Zadaj cele meno: ");
    fgets(fullName, sizeof(fullName), stdin);

    printf("Zadal si: %s", fullName);

    return 0;
}
```

Funkcia `fgets()` načíta celý riadok vrátane medzier.

Pozor: `fgets()` často uloží aj znak nového riadku `\n`, ak sa zmestí do poľa.

---

## Odstránenie znaku nového riadku po `fgets()`

Na odstránenie `\n` možno použiť knižnicu `string.h`.

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char name[50];

    printf("Zadaj meno: ");
    fgets(name, sizeof(name), stdin);

    name[strcspn(name, "\n")] = '\0';

    printf("Ahoj, %s!\n", name);

    return 0;
}
```

Funkcia `strcspn(name, "\n")` nájde pozíciu znaku nového riadku a nahradí ho znakom `\0`.

---

## Práca s reťazcami pomocou `string.h`

Na prácu s reťazcami sa používa knižnica:

```c
#include <string.h>
```

Obsahuje napríklad tieto funkcie:

| Funkcia | Význam |
|---|---|
| `strlen()` | zistí dĺžku reťazca |
| `strcpy()` | skopíruje reťazec |
| `strncpy()` | skopíruje obmedzený počet znakov |
| `strcat()` | spojí reťazce |
| `strcmp()` | porovná reťazce |
| `strchr()` | nájde znak v reťazci |
| `strstr()` | nájde podreťazec |

---

## Dĺžka reťazca: `strlen()`

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char text[] = "Programovanie";

    printf("Dlzka retazca je: %zu\n", strlen(text));

    return 0;
}
```

Výstup:

```text
Dlzka retazca je: 13
```

Funkcia `strlen()` nepočíta ukončovací znak `\0`.

---

## Kopírovanie reťazca: `strcpy()`

Reťazce sa v C nedajú priraďovať obyčajným `=` po deklarácii.

Nesprávne:

```c
char name[20];
name = "Juraj";
```

Správne:

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char name[20];

    strcpy(name, "Juraj");

    printf("%s\n", name);

    return 0;
}
```

---

## Bezpečnejšie kopírovanie: `strncpy()`

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char name[10];

    strncpy(name, "Alexander", sizeof(name) - 1);
    name[sizeof(name) - 1] = '\0';

    printf("%s\n", name);

    return 0;
}
```

Tento príklad skopíruje iba toľko znakov, aby sa neprekročila veľkosť poľa.

---

## Spájanie reťazcov: `strcat()`

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char text[50] = "Ahoj ";
    char name[] = "Juraj";

    strcat(text, name);

    printf("%s\n", text);

    return 0;
}
```

Výstup:

```text
Ahoj Juraj
```

Pri `strcat()` musí mať cieľové pole dostatočnú veľkosť.

---

## Porovnávanie reťazcov: `strcmp()`

Reťazce sa neporovnávajú pomocou `==`.

Nesprávne:

```c
if (password == "admin")
{
    printf("OK\n");
}
```

Správne:

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char password[20];

    printf("Zadaj heslo: ");
    scanf("%19s", password);

    if (strcmp(password, "admin") == 0)
    {
        printf("Spravne heslo\n");
    }
    else
    {
        printf("Nespravne heslo\n");
    }

    return 0;
}
```

Funkcia `strcmp()` vráti:

| Výsledok | Význam |
|---:|---|
| `0` | reťazce sú rovnaké |
| menej ako `0` | prvý reťazec je lexikograficky menší |
| viac ako `0` | prvý reťazec je lexikograficky väčší |

---

## 3. Súbory v jazyku C

Súbory umožňujú programu ukladať údaje natrvalo alebo ich načítavať zo súboru.

Na prácu so súbormi sa používa knižnica:

```c
#include <stdio.h>
```

Základný typ pre súbor je:

```c
FILE *
```

---

## Základný postup pri práci so súborom

Pri práci so súborom sa zvyčajne používajú tieto kroky:

1. otvorenie súboru,
2. kontrola, či sa súbor podarilo otvoriť,
3. čítanie alebo zápis,
4. zatvorenie súboru.

---

## Otvorenie súboru: `fopen()`

```c
FILE *file = fopen("data.txt", "r");
```

Funkcia `fopen()` má dva hlavné parametre:

```c
fopen("nazov_suboru", "rezim");
```

---

## Režimy otvorenia súboru

| Režim | Význam |
|---|---|
| `"r"` | otvorí súbor na čítanie |
| `"w"` | otvorí súbor na zápis, pôvodný obsah vymaže |
| `"a"` | otvorí súbor na pridávanie na koniec |
| `"r+"` | otvorí súbor na čítanie aj zápis |
| `"w+"` | otvorí súbor na čítanie aj zápis, pôvodný obsah vymaže |
| `"a+"` | otvorí súbor na čítanie aj pridávanie |
| `"rb"` | čítanie binárneho súboru |
| `"wb"` | zápis binárneho súboru |

---

## Kontrola otvorenia súboru

Po otvorení súboru treba skontrolovať, či sa otvorenie podarilo.

```c
FILE *file = fopen("data.txt", "r");

if (file == NULL)
{
    printf("Subor sa nepodarilo otvorit.\n");
    return 1;
}
```

Ak `fopen()` zlyhá, vráti hodnotu `NULL`.

---

## Zatvorenie súboru: `fclose()`

Každý otvorený súbor treba po skončení práce zatvoriť.

```c
fclose(file);
```

Zatvorením súboru sa uvoľnia systémové prostriedky a pri zápise sa zabezpečí uloženie dát.

---

## Zápis do textového súboru

Na zápis do súboru sa používa napríklad funkcia `fprintf()`.

```c
#include <stdio.h>

int main(void)
{
    FILE *file = fopen("data.txt", "w");

    if (file == NULL)
    {
        printf("Subor sa nepodarilo otvorit.\n");
        return 1;
    }

    fprintf(file, "Ahoj zo suboru!\n");
    fprintf(file, "Cislo: %d\n", 123);

    fclose(file);

    return 0;
}
```

Po spustení programu vznikne súbor `data.txt` s obsahom:

```text
Ahoj zo suboru!
Cislo: 123
```

---

## Čítanie zo súboru po riadkoch

Na čítanie celého riadku sa používa `fgets()`.

```c
#include <stdio.h>

int main(void)
{
    FILE *file = fopen("data.txt", "r");
    char line[100];

    if (file == NULL)
    {
        printf("Subor sa nepodarilo otvorit.\n");
        return 1;
    }

    while (fgets(line, sizeof(line), file) != NULL)
    {
        printf("%s", line);
    }

    fclose(file);

    return 0;
}
```

Program vypíše obsah súboru `data.txt` na obrazovku.

---

## Čítanie formátovaných údajov zo súboru

Ak sú v súbore údaje v známom formáte, možno použiť `fscanf()`.

Predstavme si súbor `numbers.txt`:

```text
10 20
```

Program:

```c
#include <stdio.h>

int main(void)
{
    FILE *file = fopen("numbers.txt", "r");
    int a;
    int b;

    if (file == NULL)
    {
        printf("Subor sa nepodarilo otvorit.\n");
        return 1;
    }

    fscanf(file, "%d %d", &a, &b);

    printf("Sucet: %d\n", a + b);

    fclose(file);

    return 0;
}
```

Výstup:

```text
Sucet: 30
```

---

## Pridávanie na koniec súboru

Ak nechceme prepísať pôvodný obsah súboru, použijeme režim `"a"`.

```c
#include <stdio.h>

int main(void)
{
    FILE *file = fopen("log.txt", "a");

    if (file == NULL)
    {
        printf("Subor sa nepodarilo otvorit.\n");
        return 1;
    }

    fprintf(file, "Nova udalost v programe.\n");

    fclose(file);

    return 0;
}
```

Každé spustenie programu pridá nový riadok na koniec súboru.

---

## Binárne súbory

Textové súbory ukladajú údaje čitateľné pre človeka. Binárne súbory ukladajú údaje v binárnej forme.

Na binárne súbory sa používajú režimy ako:

```c
"rb"
"wb"
```

Na zápis a čítanie sa používajú funkcie:

| Funkcia | Význam |
|---|---|
| `fwrite()` | zápis binárnych dát |
| `fread()` | čítanie binárnych dát |

Jednoduchý príklad zápisu čísla do binárneho súboru:

```c
#include <stdio.h>

int main(void)
{
    FILE *file = fopen("number.bin", "wb");
    int number = 12345;

    if (file == NULL)
    {
        printf("Subor sa nepodarilo otvorit.\n");
        return 1;
    }

    fwrite(&number, sizeof(int), 1, file);

    fclose(file);

    return 0;
}
```

Čítanie čísla z binárneho súboru:

```c
#include <stdio.h>

int main(void)
{
    FILE *file = fopen("number.bin", "rb");
    int number;

    if (file == NULL)
    {
        printf("Subor sa nepodarilo otvorit.\n");
        return 1;
    }

    fread(&number, sizeof(int), 1, file);

    printf("Nacitane cislo: %d\n", number);

    fclose(file);

    return 0;
}
```

---

## 4. Práca s knižnicami v jazyku C

Knižnica je súbor funkcií, typov a konštánt, ktoré môžeme použiť vo svojom programe.

V C sa knižnice pripájajú pomocou direktívy:

```c
#include
```

Napríklad:

```c
#include <stdio.h>
#include <string.h>
#include <math.h>
```

---

## Štandardné knižnice

| Knižnica | Použitie |
|---|---|
| `stdio.h` | vstup, výstup, súbory |
| `stdlib.h` | všeobecné funkcie, pamäť, konverzie |
| `string.h` | práca s reťazcami |
| `math.h` | matematické funkcie |
| `ctype.h` | práca so znakmi |
| `time.h` | práca s časom |

---

## Príklad použitia `math.h`

```c
#include <stdio.h>
#include <math.h>

int main(void)
{
    double x = 16.0;

    printf("Odmocnina: %.2f\n", sqrt(x));
    printf("Mocnina: %.2f\n", pow(2.0, 3.0));

    return 0;
}
```

Pri kompilácii v niektorých prostrediach môže byť potrebné pridať prepínač `-lm`:

```bash
gcc main.c -o main -lm
```

---

## Príklad použitia `ctype.h`

Knižnica `ctype.h` obsahuje funkcie na prácu so znakmi.

```c
#include <stdio.h>
#include <ctype.h>

int main(void)
{
    char c = 'a';

    printf("Velke pismeno: %c\n", toupper(c));

    if (isalpha(c))
    {
        printf("Znak je pismeno.\n");
    }

    return 0;
}
```

---

## Vlastná knižnica

V C si môžeme vytvoriť aj vlastnú knižnicu pomocou hlavičkového súboru `.h` a zdrojového súboru `.c`.

Príklad štruktúry:

```text
main.c
calculator.h
calculator.c
```

---

## Hlavičkový súbor `calculator.h`

```c
#ifndef CALCULATOR_H
#define CALCULATOR_H

int add(int a, int b);
int multiply(int a, int b);

#endif
```

Hlavičkový súbor obsahuje deklarácie funkcií.

Makrá:

```c
#ifndef CALCULATOR_H
#define CALCULATOR_H
#endif
```

slúžia ako ochrana proti viacnásobnému vloženiu súboru.

---

## Zdrojový súbor `calculator.c`

```c
#include "calculator.h"

int add(int a, int b)
{
    return a + b;
}

int multiply(int a, int b)
{
    return a * b;
}
```

Zdrojový súbor obsahuje implementáciu funkcií.

---

## Použitie vlastnej knižnice v `main.c`

```c
#include <stdio.h>
#include "calculator.h"

int main(void)
{
    int result = add(5, 3);

    printf("Vysledok: %d\n", result);

    return 0;
}
```

---

## Kompilácia viacerých `.c` súborov

Ak má program viac zdrojových súborov, skompilujú sa spolu:

```bash
gcc main.c calculator.c -o program
```

Spustenie:

```bash
./program
```

Na Windows môže byť spustenie napríklad:

```bash
program.exe
```

---

## Súhrnný príklad

Tento príklad načíta meno používateľa, uloží ho do súboru a potom súbor vypíše.

```c
#include <stdio.h>
#include <string.h>

int main(void)
{
    char name[50];
    FILE *file;

    printf("Zadaj meno: ");
    fgets(name, sizeof(name), stdin);

    name[strcspn(name, "\n")] = '\0';

    file = fopen("user.txt", "w");

    if (file == NULL)
    {
        printf("Subor sa nepodarilo otvorit.\n");
        return 1;
    }

    fprintf(file, "Meno pouzivatela: %s\n", name);

    fclose(file);

    file = fopen("user.txt", "r");

    if (file == NULL)
    {
        printf("Subor sa nepodarilo otvorit.\n");
        return 1;
    }

    char line[100];

    while (fgets(line, sizeof(line), file) != NULL)
    {
        printf("%s", line);
    }

    fclose(file);

    return 0;
}
```

Možný výstup:

```text
Zadaj meno: Juraj
Meno pouzivatela: Juraj
```

---

## Krátke zhrnutie

| Pojem | Význam |
|---|---|
| `printf()` | výpis na obrazovku |
| `scanf()` | načítanie vstupu z klávesnice |
| `fgets()` | načítanie celého riadku textu |
| `char text[50]` | pole znakov, teda reťazec |
| `\0` | ukončovací znak reťazca |
| `strlen()` | dĺžka reťazca |
| `strcpy()` | kopírovanie reťazca |
| `strcat()` | spájanie reťazcov |
| `strcmp()` | porovnávanie reťazcov |
| `FILE *` | ukazovateľ na súbor |
| `fopen()` | otvorenie súboru |
| `fprintf()` | zápis do súboru |
| `fscanf()` | čítanie formátovaných údajov zo súboru |
| `fgets()` | čítanie riadku zo súboru |
| `fclose()` | zatvorenie súboru |
| `#include` | pripojenie knižnice |

---

## Mini odpoveď vhodná na skúšku

V jazyku C sa na vstup z klávesnice a výstup na monitor používa najmä knižnica `stdio.h`, ktorá obsahuje funkcie ako `printf()` a `scanf()`. Funkcia `printf()` slúži na formátovaný výstup a `scanf()` na načítanie údajov od používateľa. Reťazce sa v C reprezentujú ako polia znakov ukončené nulovým znakom `\0`. Na prácu s reťazcami sa používa knižnica `string.h`, ktorá obsahuje funkcie ako `strlen()`, `strcpy()`, `strcat()` a `strcmp()`. So súbormi sa pracuje pomocou typu `FILE *` a funkcií `fopen()`, `fprintf()`, `fscanf()`, `fgets()` a `fclose()`. Súbor možno otvoriť napríklad na čítanie, zápis alebo pridávanie na koniec. Knižnice sa do programu pripájajú direktívou `#include`. Okrem štandardných knižníc si programátor môže vytvoriť aj vlastné knižnice pomocou hlavičkových `.h` súborov a zdrojových `.c` súborov.
