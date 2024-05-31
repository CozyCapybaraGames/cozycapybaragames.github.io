+++
title = 'Miért váltanunk nyelvet?'
date = 2024-05-30T07:17:30+02:00
draft = false
type = 'posts'
layout = 'posts'
+++

> Ha nem érdekel a rizsa, ugorj az [Összefoglalva](#összefoglalva) részhez!

Bár programozási nyelv fanatikus *(én)* szempontjából a Haxe rendkívül magával ragadó nyelvecske, nem az volt a cél, hogy mindenkire egy új nyelvet erőltessek ok nélkül. Amikor elkezdtük ezt az egészet, a következő fejlesztési környezetet szerettem volna megteremteni mindenkinek:

1. *Könnyen tanulható legyen*
1. *Egyszerű legyen a telepítés / setup folyamat*
1. *Gyorsan lehessen benne kódolni*
1. *Legyen egy játékfejlesztési izébizé hozzá, ami...*
    1. *egyértelmű*
    1. *jó hozzá a doksi*
    1. *nem okoz semmilyen problémát Git-el, hogy tudjunk együtt dolgozni*
    1. *nem kell egy NASA szuperkompjúter, hogy megnyiss egy projektet*
1. *Működjön web-en (opcionális)*

és ezekre pedig a tökéletes nyelv: ......*(dobpergés)*......

a *Lua* és a *LOVE2D*. Komolyan. Legalábbis anno így gondoltam, de ma már tudom, hogy

1. *Nem volt könnyen tanulható*<br>
mert egyáltalán nem hasonlított eddig semmire, amit iskolában vagy egyetemen láthattál. Bár a Lua tényleg *"egyszerű"*, a pusztán egyszerűnél sokkal célravezetőbb az *ismerős*.
1. *Nem volt egyszerű setupolni*<br>
mert sokan szenvedtetek a helyes `PATH` konfigurációval minden oprendszeren és sajnos mire rájöttem a megoldásra (készíteni egy starter projektet, amiben benne van a LOVE2D és egy futtatási script hozzá) addigra már valószínűleg páran feladtatátok a LOVE2D-t - lehet én is feladtam volna, az első benyomások fontosak!
1. *Nem lehetett benne olyan gyorsan kódolni*<br>
mert a gyenge tooling miatt szinte mindent fejből kellett volna vágni. Hiába volt (véleményem szerint) jó dokumentáció a LOVE2D-hez, nem sokan vibe-oltatok a keretrendszerrel eléggé, és még a szinte táblázatszerű oldalon is nehéz volt megtalálni, hogy hogyan érjetek el egy-egy funkciót vagy hogy a sok közül melyiket válasszátok.
1. *A játékfejlesztési izébizé hozzá...*
    1. *Talán túl egyértelmű volt*<br>
    és túl sok kódot kellett írni ahhoz, hogy bármi *egyedi* játékmechanika megvalósuljon
1. Mint kiderült, a web build nagyon is **NEM** opcionális<br>
ugyanis sokkal kevesebben hajlandóak letölteni egy random *.exe* fájlt a netről - még akkor is ha ott van mellette a forráskód is és az exe könnyen visszafordítható. Sajnos a Lua iránti szerelmem elvakított, a web build igen is létfontosságú a legtöbb jam-hez.

Ezek mellé még hozzáadhatnánk azt is, hogy valószínűen sokatoknak egyszerűen túl *"szabad"* volt a nyelv - nem volt könnyű elkezdeni egy feature-t, amit pedig valaki mégis befejezett az valószínűleg nagyban különbözött egy másik ember munkájától: van aki inkább Rust-féle kódot írt, volt aki valami OOP-szerűre vágyott s volt, aki puszta imperatív C stílusban oldott volna meg mindent.

Egyszóval:
### Kellene egy új megoldás, ami ismerősebb, jobban fogja a kezünket és ami szolidabb struktúrát ad a projektjeinknek.

A *"szolidabb struktúra"* rész a legérdekesebb - mivel kicsit eltérünk a puszta imperatív

```lua
local player = createPlayer()

player.x = player.x + player.dx
player.y = player.y + player.dy

love.graphics.draw(player.spr, player.x, player.y)
```

... receptféle kódtól, és megtanuljuk kihasználni majd egy **Entity Component System**, azaz **ECS** előnyeit is *(erről itt kezdhetsz majd el tanulgatni: [ECS](/ecs))*

Ehhez nyilván nem volt feltétlen szükséges nyelvet váltani - *Lua-hoz és LOVE2D-hez is vannak ECS keretrendszerek* - de valamiért egyik sem tetszett nekem *(sok zavaros feature, sok memorizációt igényel, sok Lua-specifikus kódot igényel<sup><a href="#fn1">\*</a></sup>, vagy minden együtt)*, és egy "szétszórtabb", szegmentáltabb rendszerezési formánál jó lenne, ha legalább a proginyelv segítene picit eligazodni ahelyett, hogy minden futtatáskor derüljön ki.

Ebből az ambícióból született meg a [Peach](/ecs/peach), a saját ECS keretrendszerünk, ami **végig fogni fogja a kezed** hibákkal és figyelmeztetésekkel egyenesen a VSCode ablakodban! Remélem, hogy mindenkinek tetszik majd és hogy frissítőnek fogja találni az efféle programozást, ahol a hibák tényleg segítenek és nem csak mégjobban összezavarnak *(khm [C++ template hibák](https://www.reddit.com/r/programminghorror/comments/mm8put/me_fully_embracing_c_templates_but_making_a/) khm)*.

Ahhoz, hogy a *Peach* létrejöhessen, szükségem volt egy nyelvre, amivel lehet forráskódot vizsgálni, azt futás közben átírni és fordítási hibákat dobálni, ha valami ténylegesen, strukturálisan helytelen vagy rossz ötlet lenne. Hirtelen csak két (nem-teljesen-funkcionális) nyelv jut eszembe, ami lehetőséget nyújt efféle metaprogramozáshoz: a *Haxe* és a *Rust* - ami sok-sok szempontból bonyolultabb lett volna egy game jamelős és tanítós forgatókönyvhöz (sorry Dávid és egyéb meg-nem-nevezett Rustacean kollega).

## Összefoglalva

Mit jelent majd nekünk a Haxe:
1. *Web build-et*
1. *Ismerősebb, C#-hoz jobban hasonlító nyelvet*
1. *Könnyebb setup-ot*
1. *Egy [ECS](/ecs) rendszert...*
    1. ami végig segít majd minket elkezdeni a kódolást és ellenőrizni azt fordításkor is és futáskor is
    1. amivel gyorsabban fogunk tudni prototípusolni<sup><a href="#fn2">\*\*</a></sup>
    1. amivel könnyebb és viccesebb lesz a debugolás<sup><a href="#fn2">\*\*</a></sup>
    1. ami könnyebb és egyértelműbb munkamegoztáshoz fog vezetni **minden skill levelen**

Miben rosszabb a Haxe mint a *Lua/LOVE2D*:
1. *Bonyolultabb game framework és pocsék doksi*<br>
Hiányos, elavult, nulla leírás és bonyolult is. A *Kha* nevű framework maga nagyon quality és powerful, de nem kezdőknek való dolog, ezért készül egy **saját mini-framework a Kha + a [Peach](/ecs/peach) köré, a [Kapiche](/haxe/kapiche) (working title)**.
1. *Néha lehal/lassú az LSP és nincs kód kiegészítés amíg újra nem indítod a VSCode-ot*<br>
Ezzel sajna nem lehet mit tenni, nem tart még ott a Haxe, hogy Go szintű toolingja legyen.

Miben jobb, mint a *C#*:
1. *Kevesebb setup*
1. *Kevesebb 'fuck you linux user' from Micro$oft (tudom, személyes probléma de AKKORIS)*
1. *Egyszerűbb nyelv - kevesebb alátét kód, nincs in-out paraméter trükközés, stb.*
1. *Makrók*<br>
(makrókat nem fogunk írni, de nekem a framework kreáláshoz nagyon hasznosak és nagyrészt emiatt lehet ilyen segítőkész a [Peach](/ecs/peach))
1. (fogjuk rá) *Jobban értek hozzá - többet tudok segíteni!*<br>
Nyilván ez nem a C# hibája, de a tény az tény.

Miben jobb, mint egy *Unity/UNREAL/Godot/stb*:
1. *Totál kontroll*<br>
Triviális lesz, ha valamiért kell egy globális változó vagy ki akarjuk kerülni az ECS keretrendszert, stb.
1. *Kevesebb tanulnivaló*<br>
(maga kevesebb komponens is lesz - idővel berántjuk a frameworkbe, ami több játékon át hasznos volt)
1. *Gyorsabb setup*
1. *Nem kell NASA PC*
1. *Nem C#* :)

---

> <sup id="fn1">\*</sup>  Ez egyébként nem rossz dolog, de mivel senkinek nem jött be a Lua, merem feltételezni, hogy a nyelvi elemek többsége sem ragadott magával senkit.
>
> <sup id="fn2">\*\*</sup>  Ezek még csak tervezett feature-ökre utalnak a cikk írásának idején, névszerint egy vizuális, real-time debuggerre (maybe időutazással) és egy a Peach-et körülvevő mini-frameworkre.