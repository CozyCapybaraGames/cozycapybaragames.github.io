+++
title = 'readonly / final'
draft = false
type = 'posts'
+++

{{< haxecs

`
// Általános alak
readonly Típus név = érték;

// Type Inference
// Típus automatikus felismerése
readonly név = érték;
`

`
// Általános alak
final név: Típus = érték;

// Type Inference
// Típus automatikus felismerése
final név = érték;
`

>}}

A *readonly/final* mezők ugyanazt a szerepet töltik be, mint a változók, viszont **létrehozás után nem lehet nekik új értéket adni a `=` művelettel**. Ennek megkísérlése fordítási hibához vezet.

Véleményem szerint ezek igen fontosak a logikai hibák megelőzésében és a megértés segítésében, mivel egyből el tudjuk választani egymástól egy kódrészlet "mozgó" és "nem mozgó" részeit.

> Fontos megjegyezni, hogy a `readonly` / `final` nem azt jelenti, hogy az érték *változtathatatlan*, csak azt, hogy a változó értékét nem lehet később lecserélni.
>
> Ez elsőre meglepetés lehet - ha például egy `readonly` változóban tárolunk egy listát, attól még a listából lehet törölni és elemeket hozzáadni - **csak a változó** ***"védett",*** **az érték nem!**

A szintaktikai különbségen kívül ugyanúgy működnek mindkét nyelvben, viszont (jelenlegi tudomásom szerint) **a `readonly` C#-ban csak osztály / rekord / struktúra mezőkön lehet használni**, míg Haxe-ben a `final` mindenhol elérhető, ahol sima változókat is létre lehet hozni.

Ennek tudtommal semmi technikai oka nincsen, csak a C# fejlesztői csapat továbbra is személyes programozási stílus preferenciák alapján fejlesztik a feature-öket, még akkor is ha [legalább 2015 óta könyörögnek a programozók valamiért](https://github.com/dotnet/roslyn/issues/115).