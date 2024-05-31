+++
title = 'Haxe mint nyelv'
date = 2024-05-23T16:28:25+02:00
draft = true
type = 'posts'
layout = 'posts'
+++

> Ez a poszt bemutatja a Haxe pár érdekesebb funkcióját, előnyeit valamint a hibáit is. Előzetes tapasztalat más programozási nyelvben vagy haladóbb C# tudás ajánlott.
>
> Ha szeretnél egy rövidebb, egy-az-egyhez összehasonlítást Haxe és C# kód között: [Haxe és C#](/haxe/cs)

Először is, nézzünk egy kis ízelítőt. Természetesen nem baj, ha valami nem érthető - egyesével végig megyünk majd mindenen.

```haxe
typedef Player = { name: String, move: Move }

enum Move { Rock; Paper; Scissors; }

enum Result { 
  Winner(player:Player); 
  Draw; 
}

class Game {
    static function main() {
        var playerA = { name: "Simon", move: Paper }
        var playerB = { name: "Nicolas", move: Rock }

        var result = switch [playerA.move, playerB.move] {
            case [Rock, Scissors] | 
                [Paper, Rock] |
                [Scissors, Paper]: Winner(playerA);
                    
            case [Rock, Paper] |
                [Paper, Scissors] |
                [Scissors, Rock]: Winner(playerB);
                    
            case _: Draw;
        }

        trace('result: $result');
    }
}
```

Látod, van pár jól ismerős elem C#-ból: `class`, `static`, `function`, `switch` - esetleg még az `enum` és a `var` is mond valamit. A kód többi része viszont elég idegennek tűnhet ha csak C#-al foglalkoztál eddig, így fentről lefele megnézünk pár nyelvi elemet a példából (és párat csak úgy nyersen).

## Anonim Struktúrák

Haxe-ben **osztály írás nélkül** is lehet objektumokat csinálni, pont mint egy szkriptnyelvben:

```haxe
var player = {
    name: 'Jóska Pista',
    hp: 100,
    weaponNames: ['Sword', 'Gun']
}
```

Mivel a Haxe típusos nyelv, ezért ez az érték a következő típust fogja kapni:

```haxe
{ name: String, hp: Int, weaponNames: Array<String> }
```

Ezt persze kiírhatjuk kézzel, ha szeretnénk mondjuk egy függvényt ami egy ilyen "player"-t kap paraméterül vagy térít vissza, de kihasználhatjuk a következő tárgyalt nyelvi elemet, ami a...

## Typedef

Akinek volt dolga *C*-vel vagy *C++*-al annak ez már ismerős. Egy typedef - vagy szebb nevén *"típus álnév"* - feladata, hogy más nevet adjon egy típusnak, például:

```haxe
typedef Szam = Int;
```

Mostmár minden `Int` típus annotációt lecserélhetünk `Szam`-ra és fordítva - tehát a **`Szam` és az `Int` ugyanaz a típus, csak más néven**.

Ezzel egyrészt extra szemantikai értelmet adhatunk a típusainknak, például:

```haxe
typedef PixelsPerSecond = Float;

function move(speed: PixelsPerSecond) {
    // ...
}
```

Természetesen erre a célra kihasználhatnánk az argumentum nevét is, a la:

```haxe
function move(speedInPixelsPerSecond: Float) {
    // ...
}
```

...ami néhány esetben jobb, néhány esetben pedig rosszabb. *Mint minden eszköz, a typedef-ek is moderációban a leghasznosabbak.*

---

A második use-case amire hirtelen gondolni tudok, az a hosszabb típusnevek lerövidítésre.

Tegyük fel, szeretnénk az előző példából négy "player" struktúrát tárolni változóban:

```haxe
var player1: { name: String, hp: Int, weaponNames: Array<String> };
var player2: { name: String, hp: Int, weaponNames: Array<String> };
var player3: { name: String, hp: Int, weaponNames: Array<String> };
var player4: { name: String, hp: Int, weaponNames: Array<String> };
```

*...yeah.* Elég kellemetlen. Ehelyett csinálhatnánk egy *typdef*-et rá:

```haxe
typedef Player = { name: String, hp: Int, weaponNames: Array<String> };

var player1: Player = [];
var player2: Player = [];
var player3: Player = [];
var player4: Player = [];
```

Mondhatjuk úgyis, hogy a **típus álnevek olyanok, mint a konstansok, csak nem értékekre hanem típusokra**.

## Enumok és Algebrai Adattípusok

Ilyesztőnek hangozhat, de valójában nagyon egyszerű.

Egy `enum` *(az 'enumeráció' rövidítése - jelentése 'komplett listázás')* összetartozó nevek csoportja:

```haxe
enum Napszak {
    Reggel;
    Este;
}
```

Ez azért hasznos, mert így felhasználhatjuk a `Napszak` típust, aminek **az egyetlen lehetséges értékei a `Reggel` és az `Este` - azaz választási lehetőséget modellez**:

```haxe
var mostaniNapszak: Napszak = Reggel;
var kesobbiNapszak = Este;
```

ilyen változákat gyakran ellenőrzünk például elágazással:

```haxe
if (mostaniNapszak == Reggel) {
    // ...
}
```

... és itt már talán láthatjuk, miért lenne sokkal törékenyebb mondjuk egy sima `String`-et használni ilyesmire:

```haxe
var mostaniNapszak = "Reggel";
var kesobbiNapszak = "Este";

if (mostaniNapszak == "reggel") { // Nagybetű-kisbetű különbség!!!
}

if (kesobbiNapszak == "Esteű") {  // Typo!!!
}
```

Az ilyen hibákat sajnos nem tudja elfogni nekünk a számítógép előre, hiszen minden valid `String` és összehasonlítás - az enumok viszont igen!

---

C#-al ellentétben a Haxe enumok egyes variánsai tartalmazhatnak értékeket is, például:

```haxe
enum Item {
    Key;
    Sword(name: String, attack: Int);
    Shield(defense: Int);
}
```

Így mind a `Key`, a `Sword` és a `Shield` is egy `Item`, viszont pl. csak a `Sword`-nak van `attack` mezője. Más OOP nyelvekben ezt sokkal kínosabb lenne biztonságosan implementálni - valszeg kéne egy rakás interface vagy csak runtime ellenőrzésekkel lehetne garantálni, hogy ne érje el senki egy `Key`-nek az `attack` mezőjét.