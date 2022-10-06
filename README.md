# Poznámky k bonusové pracovní skupince 23. 6. (funkce a struktura projektu)

Ahoj,
omlouvám se, že to tak trvalo, ale konečně jsem se dostal k sepsání poznámek.

## Vzorová řešení úloh z kodim.cz ([ZP2: 4 Funkce](https://kodim.cz/czechitas/progr2-python/zaklady-programovani-2/funkce))

Řešení, která se nebojím označit za vzorová, mi poslala jedna z vás, přeposílám je záměrně zcela beze změn:

```py
# 1 Napiš funkci mult, která bude mít dva číselné parametry. Funkce oba parametry vynásobí a vrátí výsledek.

def mult(a, b):
    return a * b​

# 2 Napiš funkci total_price, která vypočte cenu noci v hotelu. Funkce bude mít dva
# parametry - persons a breakfast. Cena za noc za osobu je 850 Kč a cena za snídani za
# osobu je 125 Kč. Funkce vrátí výslednou cenu. Parametr breakfast je nepovinný a výchozí
# hodnota je False.
#
# Funkci vyzkoušej se zadáním dvou i jedné hodnoty, např. total_price(3), total_price(2, True).

def total_price(persons, breakfast=False):
    if breakfast == True:
        return (850 + 125) * persons
    return 850 * persons

print(total_price(3))
print(total_price(2, True))​

# 3 Napiš funkci month_of_birth, která bude mít jeden parametr - rodné číslo. Funkce ze
# zadaného rodného čísla určí měsíc narození, které vrátí jako výstup. Nezapomeň, že pro
# ženy je k měsíci připočtena hodnota 50.
#
# Příklad: Pro hodnotu 9207054439 vrátí 7. Pro hodnotu 9555125899 vrátí 5.

def month_of_birth(rodne_cislo):
    return int(str(rodne_cislo)[2:4]) % 50

print(month_of_birth(9207054439))


# 4 Napiš funkci, která bude jednoduchou simulací rulety. Budeme uvažovat pouze možnost
# sázení na řady. Uživatel si může vybrat jednu ze tří řad:
#     první řada (hodnoty 1, 4, 7 atd.),
#     druhá řada (hodnoty 2, 5, 8 atd.),
#     třetí řada (hodnoty 3, 6, 9 atd.).
#
#     Zeptej se uživatele, na kterou ze tří řad sází a na výši sázky.
#
#     Vytvoř funkci roulette, která bude mít parametry číslo řady a výši sázky. Pomocí
# funkce randint z modulu random vygeneruj náhodné číslo v rozsahu od 0 (včetně) do 36.
# Vyhodnoť, do které řady číslo náleží. Nezapomeň, že 0 nepatří do žádné z řad a pokud
# padne, uživatel vždy prohrává. Funkce roulette vrací hodnotu výhry. Pokud uživatel
# vsadil na správnou řadu, vyhrává dvojnásobek sázky, v opačném případě nevyhrává nic
# jeho sázka propadá.

from random import randint

def vrati_radu(cislo):
    return list(range(cislo, 37, 3))

rada = int(input("Na kterou řadu sázíte? 1/2/3 "))
sazka = int(input("Jak vysoká je Vaše sázka? "))

def roulette(cislo_rady, vyse_sazky):
    nahodne_cislo = randint(0, 36)
    print(f"Náhodné číslo je {nahodne_cislo}")
    if nahodne_cislo in vrati_radu(rada):
        return sazka * 2
    return 0

print(f"Vaše výhra je {roulette(rada, sazka)}")
```

Vzhledem k tomu, co jsme si říkali během lekce o struktuře programu, si možná říkáte, že ta poslední úloha je trochu nepřehledná a chaotická. Pokusil jsem se ji tedy trochu učesat. A co že se mi na tom zaslaném řešení trochu nezdálo?
- V řešení se míchají definice funkcí a jejich volání (import, definice, funkční kód, definice, spuštění programu).
- Celý program se spouští voláním funkce roulette **zevnitř** f-stringu, který současně vypisuje výsledek.

Jak je to lepší?

- Nejdřív provést všechny importy.
- Potom definovat konstanty (do kterých můžete v jednodušších skriptech uložit konfiguraci programu).
- Potom sepsat všechny definice tříd (o těch si něco povíme příště) a funkcí.
- Potom teprve napsat kód, který se skutečně vykonává a ty dříve definované třídy a funkce využívá.
- Pro větší přehlednost můžete hlavní tok programu definovat v nějaké funkci, říkejme jí třeba **main**. Tuto funkci potom spustíme jediným voláním na úplném konci programu.
- Je lepší, když se nejdřív zabýváte logikou programu a nikoli výpisy. Funkce by měly dostávat vstupy a vracet výstupy v datových typech nutných k jejich funkci. Například funkce `sum(1, 2, 3)` by měla vracet jako `int`, takže třeba `6`, nikoli jako `string` v podobě `Výsledek součtu je 6`. Formátování a omáčku pro uživatele je lepší vyřešit někde odděleně, protože ten výsledek nemusíte chtít pouze vypsat, ale třeba odeslat na server, uložit do souboru apod.

Takže, přeformátovaná úloha č. 4:

```py
from random import randint


def vrati_radu(cislo: int) -> list[int]:
    # 1 -> [1, 4, 7, .., 34]
    # 2 -> [2, 5, 8, .., 35]
    # 3 -> [3, 6, 9, .., 36]
    return list(range(cislo, 37, 3))


def roulette(rada: int, sazka: int, jmeno: str = "") -> int:
    nahodne_cislo = randint(0, 36)
    print(f"Náhodné číslo je {nahodne_cislo}")

    if nahodne_cislo in vrati_radu(rada):
        return sazka * 2
    return 0


rada = int(input("Na kterou řadu sázíte? 1/2/3 "))
sazka = int(input("Jak vysoká je Vaše sázka? "))
vyhra = roulette(rada, sazka)

print(f"Vaše výhra je {vyhra}")
```

A ještě řešení jednoduché úlohy, kterou jsem vám zadával předem, opět přímo od Czechity:

```py
def je_liche(x):
    return x % 2 == 1
```

## Další zadání aneb miluju Python a chci se bavit i o prázdninách 😍

### Úlohy z [Advent of Code 2021](https://adventofcode.com/2021/):

Advent of Code je každoroční celosvětová soutěž v rychlostním programování pro programátory, kteří mají před Vánoci příliš mnoho volného času. 😁 Tyto úlohy neslouží k nácviku funkcí ani struktury programu, k jejich řešení ale budete muset zapojit všechny mozkové závity.

#### Úloha 1A

Data v souboru [1_input](test) zahrnují data o sestupu ponorky na mořské dno. Spočítejte, kolik klesajících kroků ponorka provedla (hloubka v jednom řádku je vyšší než hloubka v předchozím řádku).

#### Úloha 1B

Využijte stejná data jako v předchozím příkladu. Protože údaj ze snímače hloubky je někdy trochu nepřesný, pokusíme se tyto nepřesnosti „vyžehlit“. Při posuzování toho, zda ponorka klesá, nebo stoupá, využijte „okna“ 3 po sobě jdoucích hodnot. Příklad:
```
[5 7 8] 6 5 6 👉 20
5 [7 8 6] 5 6 👉 21 👉 +1
5 7 [8 6 5] 6 👉 19 👉 0
5 7 8 [6 5 6] 👉 17 👉 0
```
Součet prvního okna je 20, součet druhého okna je 21, ponorka klesla, přičítám jedničku. U všech ostatních oken se hloubka nezvyšuje, výsledkem analýzy je tedy číslo 1.

### Úloha od Soustruha na funkce a strukturování kódu

Doplním! 🤞