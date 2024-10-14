# Relacijska algebra

Povpraševalni jeziki (ali PJ) omogočajo urejanje podatkov in poizvedovanje
po podatkih v podatkovni bazi. Relacijski model podpira enostavne 
povpraševalne jezike z veliko izrazno močjo:

- formalne osnove v logiki
- omogoča optimizacijo poizvedb

PJ ni programski jezik! Ni računsko kompleten, ni namenjen za kompleksne
izračune, podpira enostaven in učinkovit dostop do velikih zbirk.

---

Dva formalna (matematična) jezika tvorita osnovo za *realne* povpraševalne
jezike in njihovo implementacijo.

- **Relacijska algebra** : proceduralen jezik, uporaben za predstavitev
plana izvajanja poizvedb.
- **Relacijski račun** : omogoča uporabnikom opisati kaj želijo in ne toliko
kako to izračunati - deklarativni jezik (select, filter ...)

S proceduralnimi jeziki opravljamo operacije na objektih (npr. na tabelah),
output je spet tabela (data frames...).

> povpraševalni jezik je npr. SQL

## Osnove poizvedb

Poizvedba je izvršena nad instancami relacij, rezultat je instanca neke
relacije (glej gor).

- Sheme vhodnih relacij so fiksne
- Shema rezultata je fiksna

Notacija je osnovana na poziciji/imenih atributov. Obe vrsti notacij se uporablja
v SQL.

- pozicija : primernejša za programe
- imena : primernejša za uporabnike (berljivost)

### Primer podatkovne baze

- Mornarji (**mid:int**, mime:string, ocena:int, star:int)
- Ladje (**lid:int**, lime:string, barva:string)
- Rezervacije (**mid:int**, **lid:string**, datum:date)

Bold vrednosti so primerni ključi. Povezuje data frames skupaj. Z njimi
uredimo/organiziramo podatkovno bazo.

### Primeri relacij

Npr. relaciji mornarji ni rezervacije za naše primere. Uporabljamo
notacijo osnovano na poziciji in imenih. Imena atributov v vmesnih in
končnem rezultatu se podedujejo od vhodnih relacij.

| mid | lid | dan |
| --------------- | --------------- | --------------- |
| Item1.1 | Item2.1 | Item3.1 |
| Item1.2 | Item2.2 | Item3.2 |

naredi tabele!

## Relacijske operacije

Osnovne relacijske operacije:

- **selekcija** ($\delta$) : izbere podmn. n-teric iz relacije (vrstice)
- **projekcija** ($\pi$) : izbere določene stolpce relacije (stolpci)
- **produkt** (×) : omogoča kombiniranje relacij
- **razlika** (-) : n-terice iz prve in ne druge
- **unija** (U) : n-terice obeh relacij

Dodatne operacije (niso nujne a so koristne):

- **presek** 
- **stik** 
- **deljenje** 
- **preimenovanje** 

### Projekcija

Izbere atribute, ki so v listi projekcije iz relacije. Realni sistemi ne
odstranijo duplikatov, če uporabnik tega ne zahteva.

> ker je operacija draga...

**Primer:** $\pi_{mime,ocena}(S2)$

| mime   | ocena    |
|--------------- | --------------- |
| volk   | 9   |
| kranjc   | 8   |
| jauk   | 5   |
| petelin   | 10   |

**Primer:** $\pi_{star}(S2)$

| star |
| ------------- |
| 35.0 |
| 55.5 |

### Selekcija

Izbere vrstice, ki zadoščajo pogoju selekcije. Ni duplikatov v rezultatu.

**Primer:** $\delta_{ocena>8}(S2)$

| mid | mime | ocena | star |
| --- | ---- | ----- | ---- |
| 28 | volk | 9 | 35.0 |
| 58 | petelin | 10 | 35.0 |

### Produkt

Kartezični produkt : vsaka vrstica iz $S1$ se poveže z vsako vrstio $R1$.
Shema rezultata ima po en atribut za vsak atribut elacij S1 in R1 (imena od
operandov).

**Primer** : ...

### Stik (join)

#### Stik s pogojem ali theta stik

$R \infty_{c} S = \delta_{c} (R \times S)$ in $S1 \infty_{S1.mid<R1.mid} R1$

Enako kot kartezijski produkt, ampak manj n-teric.

#### Equi stik

Pogoj stika uporablja samo pogoj enačaj. **Naravni stik** je equi-stik po 
vseh skupnih atributih.

**Primer** : $...$

### Deljenje

Ni osnovna operacija, a je uporabna za izražanje vprašanj (npr. poišči
mornarje, ki so rezervirali **vse** ladje - nevem kaj to pomeni).

Naj bo to $A$:

| sno | pno | sno | pno |
| --------------- | --------------- | --------------- | --------------- |
| s1 | p1 | s1 | p2 |
| s1 | p3 | s1 | p4 |
| s2 | p1 | s2 | p2 |
| s3 | p2 | s4 | p4 |
| s4 | p4 |    |    |

Naj bodo to $B1(pno1), B2(pno2), B3(pno3)$:

| pno1 | pno1 | pno3 |
| --- | --- | --- |
| p1  | p2  | p1  |
|     | p4  | p2  |
|     |     | p4  |

Zdaj uporabimo operacijo deljenja in dobimo $A/B1(sno1), A/B2(sno2), 
A/B3(sno3$:

| sno1 | sno2 | sno3 |
| ---| --- | --- |
| s1 | s1 | s1 |
| s2 | s4 |    |
| s4 |    |    |
| s4 |    |    |

### Unija, presek, razlika

Vse operacije so binarne. Vhodni relaciji morati biti *unija-kompatibilni* (
enako število atributov, enaki tipi izbranih polj).

**Primer:** $S1 \ U \ S2$
...

