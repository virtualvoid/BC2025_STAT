# Programátorom definované údajové typy v jazyku C

## Úvod

V jazyku **C** existujú základné dátové typy ako:

```c
int
char
float
double
```

Okrem nich si programátor môže vytvárať aj vlastné alebo zloženejšie údajové typy.

Medzi dôležité programátorom definované alebo zložené typy patria:

- pole,
- reťazec,
- `typedef`,
- `enum`,
- `struct`,
- `union`,
- operátor `sizeof`.

Tieto nástroje umožňujú lepšie organizovať údaje v programe.

---

## 1. Pole

Pole je údajová štruktúra, ktorá obsahuje viac hodnôt rovnakého typu.

Príklad poľa celých čísel:

```c
int numbers[5];
```

Toto pole má 5 prvkov typu `int`.

Indexy poľa začínajú od `0`.

| Index | Prvok |
|---:|---|
| `0` | prvý prvok |
| `1` | druhý prvok |
| `2` | tretí prvok |
| `3` | štvrtý prvok |
| `4` | piaty prvok |

---

## Deklarácia a inicializácia poľa

Pole možno deklarovať a zároveň naplniť hodnotami.

```c
int numbers[5] = {10, 20, 30, 40, 50};
```

Prístup k prvkom:

```c
numbers[0]   // 10
numbers[1]   // 20
numbers[4]   // 50
```

---

## Príklad práce s poľom

```c
#include <stdio.h>

int main(void)
{
    int numbers[5] = {10, 20, 30, 40, 50};

    for (int i = 0; i < 5; i++)
    {
        printf("%d\n", numbers[i]);
    }

    return 0;
}
```

Výstup:

```text
10
20
30
40
50
```

---

## Zmena hodnoty v poli

```c
#include <stdio.h>

int main(void)
{
    int numbers[3] = {1, 2, 3};

    numbers[1] = 100;

    printf("%d\n", numbers[0]);
    printf("%d\n", numbers[1]);
    printf("%d\n", numbers[2]);

    return 0;
}
```

Výstup:

```text
1
100
3
```

---

## Pole a veľkosť

Veľkosť statického poľa je pevne daná pri jeho vytvorení.

```c
int values[10];
```

Toto pole má vždy 10 prvkov.

Pozor na prekročenie rozsahu poľa:

```c
int numbers[3] = {1, 2, 3};

numbers[3] = 10;
```

Toto je chyba, pretože platné indexy sú iba:

```text
0, 1, 2
```

---

## 2. Reťazec

Reťazec je postupnosť znakov.

V jazyku C neexistuje samostatný typ `string`. Reťazec sa reprezentuje ako pole znakov typu `char`.

```c
char name[20];
```

Reťazec je vždy ukončený špeciálnym nulovým znakom:

```text
\0
```

---

## Reťazec ako pole znakov

```c
char text[] = "Ahoj";
```

V pamäti je uložený približne takto:

| Index | Hodnota |
|---:|---|
| `0` | `A` |
| `1` | `h` |
| `2` | `o` |
| `3` | `j` |
| `4` | `\0` |

Aj keď text `"Ahoj"` má 4 znaky, v pamäti zaberá 5 miest kvôli ukončovaciemu znaku `\0`.

---

## Výpis reťazca

```c
#include <stdio.h>

int main(void)
{
    char name[] = "Juraj";

    printf("Meno: %s\n", name);

    return 0;
}
```

Výstup:

```text
Meno: Juraj
```

Formátovací znak `%s` sa používa na výpis reťazca.

---

## Načítanie reťazca

Jednoduché načítanie pomocou `scanf()`:

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

Nevýhoda je, že `scanf("%s", ...)` načíta iba text po prvú medzeru.

Lepšie načítanie celého riadku:

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

---

## 3. `typedef`

Kľúčové slovo `typedef` slúži na vytvorenie nového názvu pre existujúci typ.

Nevytvára úplne nový typ v zmysle nového spôsobu uloženia údajov, ale vytvára alias, teda iný názov pre typ.

---

## Jednoduchý príklad `typedef`

```c
typedef unsigned int uint;
```

Potom môžeme písať:

```c
uint age = 25;
```

Namiesto:

```c
unsigned int age = 25;
```

---

## Príklad s `typedef`

```c
#include <stdio.h>

typedef unsigned int uint;

int main(void)
{
    uint count = 10;

    printf("Pocet: %u\n", count);

    return 0;
}
```

Výstup:

```text
Pocet: 10
```

---

## `typedef` so štruktúrou

Bez `typedef`:

```c
struct Person
{
    char name[50];
    int age;
};

struct Person p1;
```

S `typedef`:

```c
typedef struct
{
    char name[50];
    int age;
} Person;

Person p1;
```

Vďaka `typedef` už nemusíme písať `struct Person`, ale iba `Person`.

---

## 4. `enum`

`enum`, teda výpočtový typ, slúži na vytvorenie množiny pomenovaných celočíselných konštánt.

Používa sa vtedy, keď premenná môže nadobúdať jednu z niekoľkých vopred známych hodnôt.

---

## Jednoduchý príklad `enum`

```c
enum Day
{
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY
};
```

Hodnoty enumu sú v skutočnosti celé čísla.

Ak neurčíme hodnoty ručne:

| Položka | Hodnota |
|---|---:|
| `MONDAY` | `0` |
| `TUESDAY` | `1` |
| `WEDNESDAY` | `2` |
| `THURSDAY` | `3` |
| `FRIDAY` | `4` |
| `SATURDAY` | `5` |
| `SUNDAY` | `6` |

---

## Použitie `enum`

```c
#include <stdio.h>

enum Day
{
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY
};

int main(void)
{
    enum Day today = WEDNESDAY;

    if (today == WEDNESDAY)
    {
        printf("Dnes je streda.\n");
    }

    return 0;
}
```

Výstup:

```text
Dnes je streda.
```

---

## Vlastné hodnoty v `enum`

Hodnoty možno nastaviť aj ručne.

```c
enum Status
{
    STATUS_OK = 200,
    STATUS_NOT_FOUND = 404,
    STATUS_ERROR = 500
};
```

Príklad:

```c
#include <stdio.h>

enum Status
{
    STATUS_OK = 200,
    STATUS_NOT_FOUND = 404,
    STATUS_ERROR = 500
};

int main(void)
{
    enum Status status = STATUS_OK;

    printf("Status: %d\n", status);

    return 0;
}
```

Výstup:

```text
Status: 200
```

---

## `enum` s `typedef`

Často sa kombinuje `enum` a `typedef`.

```c
typedef enum
{
    RED,
    GREEN,
    BLUE
} Color;
```

Potom môžeme písať:

```c
Color color = GREEN;
```

---

## 5. `struct`

`struct`, teda štruktúra, umožňuje spojiť viac údajov rôznych typov do jedného celku.

Používa sa napríklad na reprezentáciu osoby, knihy, auta alebo študenta.

---

## Deklarácia štruktúry

```c
struct Person
{
    char name[50];
    int age;
    double height;
};
```

Táto štruktúra obsahuje:

- `name` ako reťazec,
- `age` ako celé číslo,
- `height` ako reálne číslo.

---

## Použitie štruktúry

```c
#include <stdio.h>
#include <string.h>

struct Person
{
    char name[50];
    int age;
    double height;
};

int main(void)
{
    struct Person person;

    strcpy(person.name, "Juraj");
    person.age = 25;
    person.height = 180.5;

    printf("Meno: %s\n", person.name);
    printf("Vek: %d\n", person.age);
    printf("Vyska: %.1f\n", person.height);

    return 0;
}
```

Výstup:

```text
Meno: Juraj
Vek: 25
Vyska: 180.5
```

---

## Inicializácia štruktúry

```c
struct Person person = {"Juraj", 25, 180.5};
```

Alebo prehľadnejšie pomocou pomenovaných členov:

```c
struct Person person = {
    .name = "Juraj",
    .age = 25,
    .height = 180.5
};
```

---

## `typedef struct`

V praxi sa často používa `typedef struct`, aby sme nemuseli stále písať slovo `struct`.

```c
typedef struct
{
    char name[50];
    int age;
    double height;
} Person;
```

Použitie:

```c
Person person = {
    .name = "Juraj",
    .age = 25,
    .height = 180.5
};
```

---

## Pole štruktúr

Môžeme vytvoriť aj pole štruktúr.

```c
#include <stdio.h>

typedef struct
{
    char name[50];
    int age;
} Student;

int main(void)
{
    Student students[3] = {
        {"Anna", 20},
        {"Peter", 21},
        {"Juraj", 22}
    };

    for (int i = 0; i < 3; i++)
    {
        printf("%s ma %d rokov.\n", students[i].name, students[i].age);
    }

    return 0;
}
```

Výstup:

```text
Anna ma 20 rokov.
Peter ma 21 rokov.
Juraj ma 22 rokov.
```

---

## Smerník na štruktúru

Ak máme smerník na štruktúru, k členom pristupujeme pomocou operátora `->`.

```c
#include <stdio.h>

typedef struct
{
    char name[50];
    int age;
} Person;

int main(void)
{
    Person person = {"Juraj", 25};
    Person *p = &person;

    printf("Meno: %s\n", p->name);
    printf("Vek: %d\n", p->age);

    return 0;
}
```

Zápis:

```c
p->age
```

je skrátený zápis pre:

```c
(*p).age
```

---

## 6. `union`

`union` je podobná štruktúre, ale všetky jej členy zdieľajú tú istú pamäť.

To znamená, že v jednom okamihu má zmysel používať iba jeden člen unionu.

---

## Rozdiel medzi `struct` a `union`

Pri `struct` má každý člen vlastné miesto v pamäti.

```c
struct Example
{
    int i;
    float f;
    char c;
};
```

Pri `union` členy zdieľajú rovnaké miesto.

```c
union Value
{
    int i;
    float f;
    char c;
};
```

---

## Príklad `union`

```c
#include <stdio.h>

union Value
{
    int i;
    float f;
    char c;
};

int main(void)
{
    union Value value;

    value.i = 100;
    printf("i = %d\n", value.i);

    value.f = 3.14f;
    printf("f = %.2f\n", value.f);

    value.c = 'A';
    printf("c = %c\n", value.c);

    return 0;
}
```

Výstup môže byť:

```text
i = 100
f = 3.14
c = A
```

Pozor: po zapísaní do `value.f` už pôvodná hodnota `value.i` nie je spoľahlivo použiteľná, pretože oba členy zdieľajú rovnakú pamäť.

---

## Ukážka zdieľania pamäte v `union`

```c
#include <stdio.h>

union Value
{
    int i;
    float f;
};

int main(void)
{
    union Value value;

    value.i = 100;
    value.f = 3.14f;

    printf("i = %d\n", value.i);
    printf("f = %.2f\n", value.f);

    return 0;
}
```

Výstup pre `i` môže byť nezmyselný, pretože posledná zapísaná hodnota bola `f`.

---

## Kedy sa používa `union`

`union` sa používa najmä vtedy, keď chceme šetriť pamäť a vieme, že v danom okamihu potrebujeme uložiť iba jednu z viacerých možných hodnôt.

Napríklad hodnota môže byť buď celé číslo, alebo reálne číslo, alebo znak.

Často sa používa spolu s `enum`, ktorý určuje, aký typ hodnoty je práve uložený.

---

## `union` spolu s `enum`

```c
#include <stdio.h>

typedef enum
{
    TYPE_INT,
    TYPE_FLOAT,
    TYPE_CHAR
} ValueType;

typedef union
{
    int i;
    float f;
    char c;
} ValueData;

typedef struct
{
    ValueType type;
    ValueData data;
} Value;

int main(void)
{
    Value value;

    value.type = TYPE_FLOAT;
    value.data.f = 3.14f;

    if (value.type == TYPE_FLOAT)
    {
        printf("Float: %.2f\n", value.data.f);
    }

    return 0;
}
```

Tu:

- `enum ValueType` hovorí, aký typ hodnoty je uložený,
- `union ValueData` drží samotnú hodnotu,
- `struct Value` spája typ aj hodnotu.

---

## 7. Operátor `sizeof`

Operátor `sizeof` slúži na zistenie veľkosti typu alebo premennej v bajtoch.

Príklady:

```c
sizeof(int)
sizeof(double)
sizeof(char)
```

---

## Príklad použitia `sizeof`

```c
#include <stdio.h>

int main(void)
{
    printf("sizeof(char) = %zu\n", sizeof(char));
    printf("sizeof(int) = %zu\n", sizeof(int));
    printf("sizeof(double) = %zu\n", sizeof(double));

    return 0;
}
```

Možný výstup:

```text
sizeof(char) = 1
sizeof(int) = 4
sizeof(double) = 8
```

Presné hodnoty sa môžu líšiť podľa platformy a kompilátora.

Formát `%zu` sa používa pre hodnotu typu `size_t`, ktorú vracia `sizeof`.

---

## `sizeof` a pole

Pri statickom poli môžeme pomocou `sizeof` zistiť počet prvkov.

```c
#include <stdio.h>

int main(void)
{
    int numbers[] = {10, 20, 30, 40, 50};

    size_t count = sizeof(numbers) / sizeof(numbers[0]);

    printf("Pocet prvkov: %zu\n", count);

    return 0;
}
```

Výstup:

```text
Pocet prvkov: 5
```

Výpočet:

```c
sizeof(numbers) / sizeof(numbers[0])
```

znamená:

```text
celková veľkosť poľa / veľkosť jedného prvku
```

---

## Pozor na `sizeof` pri smerníku

Ak pole odovzdáme do funkcie, zmení sa na smerník na prvý prvok.

```c
#include <stdio.h>

void printSize(int numbers[])
{
    printf("sizeof(numbers) vo funkcii: %zu\n", sizeof(numbers));
}

int main(void)
{
    int numbers[5] = {1, 2, 3, 4, 5};

    printf("sizeof(numbers) v main: %zu\n", sizeof(numbers));

    printSize(numbers);

    return 0;
}
```

Vo funkcii `printSize` už `numbers` nie je celé pole, ale smerník.

Preto počet prvkov poľa treba do funkcie posielať zvlášť.

Správnejší zápis:

```c
#include <stdio.h>

void printArray(int numbers[], size_t count)
{
    for (size_t i = 0; i < count; i++)
    {
        printf("%d ", numbers[i]);
    }
}

int main(void)
{
    int numbers[5] = {1, 2, 3, 4, 5};
    size_t count = sizeof(numbers) / sizeof(numbers[0]);

    printArray(numbers, count);

    return 0;
}
```

---

## `sizeof` a `struct`

Veľkosť štruktúry nemusí byť jednoduchým súčtom veľkostí jej členov.

Dôvodom je zarovnanie pamäte, teda padding.

```c
#include <stdio.h>

typedef struct
{
    char c;
    int i;
} Example;

int main(void)
{
    printf("Velkost struktury: %zu\n", sizeof(Example));

    return 0;
}
```

Aj keď `char` má zvyčajne 1 bajt a `int` 4 bajty, veľkosť štruktúry môže byť napríklad 8 bajtov kvôli zarovnaniu.

---

## `sizeof` a `union`

Veľkosť unionu je daná veľkosťou jeho najväčšieho člena, prípadne ešte zarovnaním pamäte.

```c
#include <stdio.h>

union Value
{
    char c;
    int i;
    double d;
};

int main(void)
{
    printf("Velkost unionu: %zu\n", sizeof(union Value));

    return 0;
}
```

Ak je najväčší člen `double`, union bude mať aspoň veľkosť typu `double`.

---

## Súhrnný príklad

Tento príklad používa `typedef`, `enum`, `struct`, `union` aj `sizeof`.

```c
#include <stdio.h>
#include <string.h>

typedef enum
{
    STUDENT_ACTIVE,
    STUDENT_INACTIVE
} StudentStatus;

typedef union
{
    int year;
    double average;
} StudentInfo;

typedef struct
{
    char name[50];
    int age;
    StudentStatus status;
    StudentInfo info;
} Student;

int main(void)
{
    Student student;

    strcpy(student.name, "Juraj");
    student.age = 22;
    student.status = STUDENT_ACTIVE;
    student.info.average = 1.5;

    printf("Meno: %s\n", student.name);
    printf("Vek: %d\n", student.age);

    if (student.status == STUDENT_ACTIVE)
    {
        printf("Status: aktivny\n");
    }

    printf("Priemer: %.2f\n", student.info.average);

    printf("Velkost Student: %zu bajtov\n", sizeof(Student));

    return 0;
}
```

Možný výstup:

```text
Meno: Juraj
Vek: 22
Status: aktivny
Priemer: 1.50
Velkost Student: 72 bajtov
```

Veľkosť štruktúry sa môže líšiť podľa platformy a kompilátora.

---

## Krátke zhrnutie

| Pojem | Význam |
|---|---|
| pole | viac hodnôt rovnakého typu uložených za sebou |
| index poľa | poradové číslo prvku, začína od `0` |
| reťazec | pole znakov ukončené znakom `\0` |
| `typedef` | vytvorenie aliasu pre existujúci typ |
| `enum` | množina pomenovaných celočíselných konštánt |
| `struct` | zložený typ s viacerými členmi |
| `union` | zložený typ, ktorého členy zdieľajú tú istú pamäť |
| `sizeof` | operátor na zistenie veľkosti typu alebo premennej |
| `%zu` | formátovací znak pre hodnotu typu `size_t` |

---

## Mini odpoveď vhodná na skúšku

Programátorom definované údajové typy v jazyku C umožňujú vytvárať zložitejšie štruktúry údajov. Pole obsahuje viac prvkov rovnakého typu a k jeho prvkom sa pristupuje pomocou indexov od nuly. Reťazec je pole znakov typu `char`, ktoré je ukončené nulovým znakom `\0`. Pomocou `typedef` možno vytvoriť nový názov pre existujúci typ. Typ `enum` slúži na definovanie množiny pomenovaných celočíselných konštánt. Štruktúra `struct` umožňuje spojiť viac údajov rôznych typov do jedného celku. `union` je podobný ako `struct`, ale všetky jeho členy zdieľajú rovnaké miesto v pamäti, preto má zmysel používať vždy iba jeden člen naraz. Operátor `sizeof` vracia veľkosť typu alebo premennej v bajtoch a často sa používa pri práci s poľami, štruktúrami a dynamickou pamäťou.
