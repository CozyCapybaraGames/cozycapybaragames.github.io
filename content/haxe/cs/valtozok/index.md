+++
title = 'Változók `(var)`'
draft = false
type = 'posts'
+++

{{< haxecs

`
// Általános alak
Típus név = érték;

// Pl.:
int health = 100;

// Type Inference
// Típus automatikus felismerése
var név = "Tudja, hogy ez String";

// Új érték megadása
health = 50;
`

`
// Általános alak
var név: Típus = érték;

// Pl.:
var health: Int = 100;

// Type Inference
// Típus automatikus felismerése
var név = "Tudja, hogy ez String";

// Új érték megadása
health = 50;
`

>}}

A változók szinte azonos módon működnek mindkét nyelvben, csak szintaktikai különbség van a létrehozásban: **`Típus név = érték;`** **VS** **`var név: Típus = érték;`**;

A legtöbb esetben felesleges típust megadni változóknak, ugyanis a számító ki tudja találni magától. Ebben az esetben a két nyelvben ugyanazt kell írni: **`var név = érték;`**