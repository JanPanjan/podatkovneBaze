Pridobite vse informacije iz tabele facilities 

```sql
SELECT * FROM facilities;
```

Izpišite seznam vseh objektov (facilities) in stroške za njihove člane.

```sql
SELECT name, membercost FROM facilities;
```

Izpišite seznam vseh objektov, ki članom zaračunajo članarino. 

```sql
SELECT name FROM facilities 
WHERE membercost > 0;
```

Prikažite seznam vseh objektov, ki članom zaračunajo članarino, in 
ta članarina znaša manj kot 1/50 mesečnih stroškov vzdrževanja. 
Izpiše naj se facid, ime objekta, cena članarine in mesečno vzdrževanje objektov.

```sql
SELECT facid, name, membercost, monthlymaintenance 
FROM facilities
WHERE membercost > 0 AND (membercost < monthlymaintenance/50);
WHERE membercost > (1 OR 2) 
```

Izpišite seznam vseh objektov z besedo "Tennis" v njihovem imenu.

> '%' je regex, ki pomeni '*', LIKE je uporabljen za matchanje strings.

```sql
SELECT name FROM facilities 
WHERE name LIKE '%Tennis%'
```

Pridobite podrobnosti o objektih z ID 1 in 5, brez da bi uporabili operator OR. 

> IN je kot LIKE, samo za integerje. Ne izbere od 1 do 5, ampak samo 1 ali 5.

```sql
SELECT * FROM facilities
WHERE facid IN (1,5);
```

Pripravite seznam članov, ki so se pridružili po začetku septembra 2012. 
Izpišite memid, priimek (surname), ime (firstname), in datum pridružitve (joindate) članov. 

> Datumi se obnašajo kot številke in string ids, pač so svoj tip,
> ampak still

```sql
SELECT memid, surname, firstname, joindate 
FROM members
WHERE joindate > '2012-09-01';
```

Pripravite urejen seznam prvih 10-ih priimkov iz tabele members. Seznam ne sme 
vsebovati duplikatov. 

> DISTINCT izbere unikatne vrstice kot input.
> LIMIT je kot head().

```sql
SELECT DISTINCT surname FROM members
ORDER BY surname
LIMIT 10;
```

Vrnite kombiniran seznam vseh priimkov in vseh imen objektov. 

> UNION združi dva stolpca v enega. Pazi, da daš obema enako ime.

```sql
SELECT name AS combined
FROM facilities
UNION
SELECT surname AS combined
FROM members;
```

Pridobite datum prijave vašega zadnjega člana.

```sql
SELECT max(joindate) FROM members;
```

Ugotovite, koliko objektov (vrstic) obstaja v facilities - (vrnite skupno število). 

```
SELECT count(*) FROM facilities;
```

Izpišite število objektov, ki gostom zaračunajo 10 ali več denarnih enot.  

> count kot aggregate funkcija deluje na filtriranih vrsticah. Kljub temu,
> da je WHERE clause v drugi vrstici, se izvede najprej in nato count
> prešteje vrstice, ki so ostale.

```
SELECT count(guestcost) FROM facilities
WHERE guestcost > 10;
```

Izpišite število priporočil, ki jih je vsak član do sedaj dodal. 
Spisek uredite glede na ID članov (Pomoč: recommendedby). 

> čudn ker je tabela v rešitvah s podatki, ki niso v originalni tabeli...

```
SELECT memid, recommendedby FROM members
WHERE recommendedby > 0
ORDER BY memid;
```

Izpišite seznam skupnega števila rezerviranih slotov glede na objekt. 
Naj se prikaže tabela z ID objektov in število rezerviranih slotov. 
Tabela naj bo urejena glede na ID objektov. 

> Spet ni isto, mogoče delam jaz kaj narobe...

```
SELECT bookings.facid, count(bookings.slots) FROM bookings
JOIN facilities ON bookings.facid = facilities.facid
GROUP BY bookings.facid
ORDER BY bookings.facid;
```

Izpišite seznam skupnega števila rezerviranih slotov na objekt v 
mesecu septembru 2012. Prikaže naj se tabela z ID objekti in število
rezerviranih slotov, urejena naj bo glede na število slotov. 

> Spet čuden outcome...

```
SELECT facid, count(slots) AS tot_slots FROM bookings
WHERE starttime >= '2012-09-01' AND starttime <= '2012-09-30'
GROUP BY facid
ORDER BY total_slots;
```

Izpišite seznam skupnega števila rezerviranih slotov na objekt za 
vsak mesec v letu 2012. Prikaže naj se tabela z ID objektov, 
sloti, sortirana pa naj bo glede na ID objektov in mesec.
Namig: Mesec v letu lahko pridobimo s funkcijo ‘extract()’ 

```
SELECT facid, extract(MONTH FROM starttime) AS month, 
    count(slots) AS tot_slots
FROM bookings
GROUP BY facid, month
ORDER BY facid, month
```

Izpišite skupno število članov, ki so opravili vsaj eno rezervacijo.  

> idk...

```
SELECT max(distinct(memid)) FROM bookings
WHERE memid > 0
```

ali 

```
SELECT memid FROM bookings
WHERE memid > 0
ORDER BY memid DESC
LIMIT 1
```

ali

```
SELECT distinct(memid) AS id FROM bookings
ORDER BY id DESC
LIMIT 1
```

Prikažite seznam objektov z več kot 1000 rezerviranimi sloti.  
Prikazana tabela naj vsebuje ID objektov, in količina slotov, 
urejena pa naj bo glede na ID objektov. 

```
SELECT facid, sum(slots) AS total_slots
FROM bookings
GROUP BY facid
HAVING sum(slots) > 1000
ORDER BY facid
```

Izpišite ID objekta z največjim številom rezerviranih slotov.

```
SELECT facid, sum(slots) AS total_slots
FROM bookings
GROUP BY facid
ORDER BY total_slots DESC
LIMIT 1;
```


```language

SELECT b.starttime, b.facid
FROM bookings b
WHERE b.facid IN (0,1)
AND cast(b.starttime AS time) LIKE '2012-09-21%'
```
