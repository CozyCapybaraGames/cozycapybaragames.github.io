+++
title = 'Függvények / Metódusok `(function)`'
draft = false
type = 'posts'
+++

{{< haxecs

`
// Általános alak
ReturnTípus Nev(ArgTípus1 ArgNév1, ..., ArgTípusN ArgNévN) {
    // Függvénytest
}

// Példa
int Add(int a, int b) {
    return a + b;
}
`

`
// Általános alak
function Nev(ArgNév1: ArgTípus1, ..., ArgNévN: ArgTípusN): ReturnTípus {
    // Függvénytest
}

// Példa
function add(a: Int, b: Int): Int {
    return a + b;
}

// Minimális példa:
// ReturnTípus automatikus felismerése
// Argumentum típusok felismerése használatból (nem ajánlott)
function add(a, b) {
    return a + b;
}

// Típus kiírattatása a konzolra:
$type(add);  // (a: Int, b: Int) -> Int

`

>}}

A függvények szintén azonosan működnek a két nyelvben, csak szintaktikailag mások picit. Mindkét nyelv követi a változókból megszokott sorrendet a függvény- és argumentum deklarációkhoz: C#-ban `Típus név`, Haxe-ben `név: Típus`.

A Haxe-béli függvények érdekessége, hogy a visszatérítési érték típusa és még az argumentumok típusai is elhagyhatók, és megkérhetjük a nyelvet, hogy ismerje fel a típusokat magától. Ez elég *kétélű kard*:
- egyrészt lehet, hogy rosszul ismeri fel a nyelv a típusokat (argumentumoknál kifejezetten igaz)
- másrészt pedig lehet, hogy nehezebben olvasható lesz tőle a kód

A type inference használatának mértéke mindenkinek saját preferenciája, ám amikor csapatban dolgozunk *érdemes megállapodni valami szabályon*. Személy szerint én majdnem mindig lehagyom a visszatérítési értékeket, mivel ezeket 99%-ban helyesen ismeri fel a nyelv, és a pár másodpercig a függvény nevére biggyesztett kurzor bárkinek megmutatja a típust, az argumentumok típusait viszont mindig kiírom, hogy minél gyorsabban tudjon helyes segítséget nyújtani a nyelv.