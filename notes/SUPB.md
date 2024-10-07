# Sistem za upravljanje s podatkovnimi bazami

*Database management system* ali DMBS ali *sistem za upravljanje s podatkovnimi
bazami* ali SUPB je programska oprema ustvarjena tako, da pomaga upravljati
velike količine podatkov oz. podatkovne baze.

Svoje okolje modelira po realnem svetu iz katerega črpa podatke.

> Šolski sistem bi imel npr. podatke o *entitetah* (npr. študenti ali
> neki predmeti) ter podatke o *razmerjih* med enetitetami (npr. v kateri
> predmet je vpisan kateri študent).

Namesto, da uporabljamo programčke za delo s shranjenimi podatki, uporabljamo
podatkovne baze zaradi naslednjih razlogov:

1. **Database design** - kako lahko uporabnik ustvari podatke v bazi, tako da
bodo primerjlive z modelom iz katerega črpa te podatke? (npr. kako bi
ustvaril podatkovno bazo za univerziteto)

2. **Data analysis** - kako lahko uporabnik odgovori na vprašanja o podatkovni
bazi, tako da si pomaga s *queries* na podatkih?

3. **Concurrency and robustness** - kako lahko DBMS dopušča mnogo uporabnikom,
da istočasno dostopajo do podatkov ter kako zavaruje podatke pred sistemskimi
izpadi?

4. **Efficiency and scalability** - kako lahko DBMS hrani velike količine
podatkov, tako da nima problemov z učinkovitostjo?

## Nivoji abstrakcije

DBMS hrani podatke v obliki bitov brez večje logike. Uporabniki so tisti,
ki ustvarjajo modele in razmerja med entitetami v bazi. Tako se na podlagi
več nivojev abstrakcije hranijo in uporabljajo podatki v DBMS.

    ------------  ------------  ------------
    | pogled 1 |  | pogled 2 |  | pogled 3 |
    ------------  ------------  ------------
         |              |             |
         ------------------------------
                        |
                        V
              ----------------------
              | konceptualna shema |
              ----------------------
                        |
                        V
                -----------------
                | fizična shema |
                -----------------
                        |
                        V
                    ---------
                    ---------
                    ---------
                    ---------

**Pogledi** opisujejo kako uporabnik vidi podatke. **Konceptualna shema**
definira logično strukturo baze. **Fizična shema** opisuje uporabljene
datoteke in indekse.

### Primer podatkovne baze univerze

1. **Konceptualna shema**:
    - študenti (ime. starost, povprečje ocen...)
    - predmeti (ime, točke)
    - vpis (ocena)
2. **Fizična shema**:
    - relacije shranjene v neurejenih datotekah
    - indeks je definiran na prvem stolpcu relacije *študenti*
3. **Zunanja shema (pogled)**:
    - Predmet_info (vpisani na predmet)

