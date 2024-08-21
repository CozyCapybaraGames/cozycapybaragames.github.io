+++
title = 'Konstansok `(static inline final)`'
draft = false
type = 'posts'
+++

{{< haxecs

`
// Általános alak
const Típus név = konst-kifej;

// Type Inference
// Típus automatikus felismerése
// Nincs C#-ban :(
`

`
// Általános alak
static inline final név: Típus = konst-kifej;

// Type Inference
// Típus automatikus felismerése
static inline final név = konst-kifej;
`

>}}

A konstansok kicsit extrásabb verziói a [`readonly`/`final` változóknak](/haxe/cs/readonly_final), ugyanis itt **nem csak a változó értéke nem változhat, de maga az érték sem!**. Ez azt jelenti, hogy egy `const` változó csak bizonyos *"konstans kifejezések"* értékét tudja tárolni, amikről a nyelv már fordításkor tudja, hogy mit fog jelképezni - így magabiztosabban használhatja őket arra, hogy optimalizálja a kódunkat.

Gyakori példák erre a primitív szószerinti értékek:

```cs
const double Pi = 3.14;
const int ScreenDesignWidth = 1920;
const string Name = "Valaki";
```

Haxe-ben ugyanezt az ötletet egy csomó másik kulcsszóval fejezzük ki, de a működése azonos a C#-éval. *Ezekről mind szó fog esni később, egyenlőre elég, ha megjegyzed vagy felkeresed ezt az oldalt*.

> Ha esetleg hallottad már ezeket a kulcsszavakat más nyelvekből, rá tudsz jönni, miért ezek jelzik Haxe-ben a konstansokat:
>
> - `static`: Nincs szüksége minden objektumnak egy saját másolatra, mert mindig ugyanazt jelenti
> - `inline`: A konstans neve helyett a konkrét érték kerül beillesztésre a produkált kódban
> - `final`: A konstansok értékét nem lehet lecserélni értékadással.