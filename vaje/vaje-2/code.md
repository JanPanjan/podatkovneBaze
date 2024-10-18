Poiščite priimke vseh članov, ki so kadarkoli kaj rezervirali. 

```sql
SELECT distinct(m.surname) 
FROM members m, bookings b
WHERE m.memid = b.memid
```

enakovredno

```sql
SELECT DISTINCT m.surname
FROM members m
NATURAL JOIN bookings b;
```

Poiščite priimke vseh članov, ki so do sedaj naredili več kot 100 rezervacij

```sql
SELECT m.surname, count(*) FROM members m
NATURAL JOIN bookings book
GROUP BY m.surname
HAVING count(*) > 100
```

Poiščite memid vseh članov, ki so rezervirali Squash Court  ali Pool Table 

```sql
SELECT distinct(b.memid)
FROM facilities f, bookings b
WHERE f.facid = b.facid
AND (f.name = 'Squash Court' OR f.name = 'Pool Table')
```

Poiščite memid vseh članov, ki so rezervirali “Squash Court” ali se njihov 
priimek začne s črko R. 

```sql
SELECT distinct(b.memid)
FROM facilities f, bookings b
WHERE f.facid = b.facid AND (f.name = 'Squash Court')
UNION
SELECT m.memid
FROM members m
WHERE surname LIKE 'R%';
```

Poiščite memid vseh članov, ki so do sedaj rezervirali Squash Court, kot tudi 
Pool Table

```sql
SELECT distinct(b.memid)
FROM facilities f, bookings b
WHERE f.facid = b.facid AND (f.name = 'Squash Court')
INTERSECT
SELECT distinct(b.memid)
FROM facilities f, bookings b
WHERE f.facid = b.facid AND (f.name = 'Pool Table')
```

Prejšnjo nalogo (Naloga 5) nadgradite tako, da se vam izpišejo imena in priimki 
članov, ki so do sedaj rezervirali “Squash Court” in “Pool Table”. NAMIG: 
uporabite gnezden SELECT stavek 

```sql
SELECT m.firstname, m.surname
FROM members m
WHERE m.memid IN (

SELECT distinct(b.memid)
FROM facilities f, bookings b
WHERE f.facid = b.facid AND (f.name = 'Squash Court')
INTERSECT
SELECT distinct(b.memid)
FROM facilities f, bookings b
WHERE f.facid = b.facid AND (f.name = 'Pool Table')

);
```

Poiščite priimke vseh članov, ki so se pridružili po članu s priimkom “Owen”

> Razmišljaj o množicah. Tako kot v R-ju, ko dobiš z neko funkcijo
> neke vektorje in jih fukneš v map funkcijo al karkoli.

```sql
SELECT m.surname, m.joindate FROM members m
WHERE m.joindate > (

SELECT m.joindate FROM members m WHERE m.surname LIKE 'Owen'

);
```

> Če bi imeli dva pogoja (Owen in Baker npr.), bi morali pisati 
> '...date > ANY (...', torej ANY bi morali dodat.

Izpišite imena in priimke vseh članov, ki so že rezervirali eno izmed tenis igrišč »Tennis Court 1«, »Tennis Court 2«, …. 

```sql
SELECT distinct(m.surname), m.firstname 
FROM members m, bookings b, facilities f
WHERE b.facid = f.facid
AND m.memid = b.memid
AND f.name LIKE 'Tennis Court%'
```

Izpišite številke objektov, ki imajo najvišje mesečno vzdrževanje

```sql
SELECT facid, monthlymaintenance 
FROM facilities
WHERE monthlymaintenance = (

SELECT max(monthlymaintenance) FROM facilities

)
```

Izpišite številke objektov, ki imajo drugo najvišje mesečno vzdrževanje

```sql
SELECT facid, monthlymaintenance FROM facilities
WHERE monthlymaintenance = (

SELECT monthlymaintenance FROM facilities
WHERE monthlymaintenance < (SELECT max(monthlymaintenance) FROM facilities)
ORDER BY monthlymaintenance DESC
LIMIT 1

)
```

Imena vseh članov, ki so že kdaj rezervirali naenkrat 6 slotov. 

```sql
SELECT firstname FROM members
WHERE memid = ANY (

SELECT distinct(memid) FROM bookings
WHERE slots = 6

)
```

ali

```sql
SELECT distinct(m.firstname) FROM members m
WHERE EXISTS (

SELECT b.memid FROM bookings b
WHERE b.slots = 6
AND m.memid = b.memid

)
```

> EXISTS preveri pogoj v oklepaju in vrne vrstice iz m, za
> katere pogoj drži.

Izpišite Imena in priimke obiskovalcev, ki so večkrat rezervirali Badminton 
court od vsake osebe z imenom »Timothy«  

```sql
SELECT m.firstname, m.surname, count(b.starttime) 
FROM members m, bookings b, facilities f 
WHERE b.memid = m.memid
AND b.facid = f.facid
AND f.name = 'Badminton Court'
GROUP BY m.firstname, m.surname
HAVING count(b.starttime) > 
( -- timothy count
SELECT count(b2.starttime)
    FROM bookings b2
    JOIN members m2 ON b2.memid = m2.memid
    JOIN facilities f2 ON b2.facid = f2.facid
    WHERE m2.firstname = 'Timothy'
    AND f2.name = 'Badminton Court'
    GROUP BY m2.firstname
)
```

Tokrat uporabite INNER JOIN ali pa na sledeč način: “R1.id = R2.id”. (Izpišite 
Imena in priimke obiskovalcev, ki so večkrat rezervirali Badminton court od 
vsake osebe z imenom »Timothy«). 

```sql
SELECT m.firstname, m.surname, count(b.starttime) 
FROM members m
INNER JOIN bookings b ON b.memid = m.memid
INNER JOIN facilities f ON b.facid = f.facid
WHERE f.name = 'Badminton Court'
GROUP BY m.firstname, m.surname
HAVING count(b.starttime) > 
( -- timothy count
SELECT count(b2.starttime)
    FROM bookings b2
    JOIN members m2 ON b2.memid = m2.memid
    JOIN facilities f2 ON b2.facid = f2.facid
    WHERE m2.firstname = 'Timothy'
    AND f2.name = 'Badminton Court'
    GROUP BY m2.firstname
)
```

Izpišite imena objektov, ki zaračunajo najvišjo ceno gostom. (14)

```sql
SELECT name, guestcost FROM facilities
ORDER BY guestcost DESC
LIMIT 2;
```

Izpišite seznam začetnih časov (“starttime”) vseh rezervacij, ki jih je storil član 
'David Farrell'? (Uporabite INNER JOIN) (15)

1. brez inner join

```sql
SELECT b.starttime
FROM bookings b, members m
WHERE b.memid = m.memid
AND m.firstname LIKE 'David' 
AND m.surname LIKE 'Farrell'
```

1. z inner join

```sql
SELECT b.starttime
FROM bookings b
INNER JOIN members m ON b.memid = m.memid
WHERE m.firstname LIKE 'David' 
AND m.surname LIKE 'Farrell'
```

Izpišite seznam začetnih časov (“starttime”) rezervacij narejenih za objekta z ID 
0 in 1, in sicer za dne '2012-09-21'? Vrnite seznam začetnih časov (starttime in 
imena objektov). Seznam naj bo urejen po “starttime”. 

```sql
SELECT b.starttime, f.name
FROM bookings b
NATURAL JOIN facilities f
WHERE b.facid IN (0,1)
AND cast(b.starttime AS date) = '2012-09-21'
```

Izpišite seznam vseh članov, vključno s člani, ki so jih priporočili (Če jih ni 
priporočil nihče, vrnite NULL). Seznam naj bo urejen po priimku in imenu.

```sql
SELECT m1.firstname, m1.surname, m2.firstname, m2.surname
FROM members m1
JOIN (SELECT * FROM members) m2
ON m1.recommendedby = m2.memid
ORDER BY m1.firstname
```

 Izpišite seznam vseh članov na način, da se prikažejo priimki in imena v
sledečem formatu 'Surname, Firstname'. (Funkcija CONCAT())

```sql
SELECT CONCAT(surname, ', ', firstname)
FROM members
```

Izpišite začetnice vseh članov (predpostavljamo, da ime vsak le en priimek in le
eno ime).

```sql
SELECT CONCAT(LEFT(surname, 1), '.', LEFT(firstname, 1))
FROM members
```

> Useful link: [sql builtin server funkcije](https://www.w3schools.com/sql/func_sqlserver_left.asp) 
