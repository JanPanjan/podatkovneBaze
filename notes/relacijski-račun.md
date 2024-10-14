# Relacijski račun

Obstajata dve obliki - **n-terični relacijski račun (TRR)** in **domenski
relacijski račun (DRR)**.

Izrazi poizvedb (t.i. *formule*) vsebujejo spremenljivke, konstante, 
primerjalne operacije, logične operacije in kvantifikatorje. 
Odgovor na vprašanje dobimo tako, da prostim spremenljivkam priredimo konstante, tako da je vrednost 
formule TRUE.

- TRR: spremenljivke so omenejene na n-terice
- DRR: spremenljivke so omenejene na domene atributov (ne bomo delali)

## N-terični relacijski račun (TRR)

Poizvedba ima obliko $\{T; p(T)\}$, kjer je $T$ prosta spremenljivka.
Rezultat vsebuje vse n-terice $T$, za katere je $p(T)==TRUE$.

Izraz TRR je rekurzivno definiran na osnovi enostavnih atomarnih
izrazov, ki se lahko gradijo v bolj kompleksne izraze z logičnimi
operacijami.

> atomarni izrazi so v tem primeru referenciranje n-teric v relacijah
> ali primerjanje atributov.

Nevezane spremenljivke so **proste spremenljivke**.
