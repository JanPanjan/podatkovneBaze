# Osnovni ukazi

## Select

`SELECT` izbere stolpce, spremenljivke, domene, idk.

    SELECT <Kaj>

`<Kaj>` je lahko [regular expression] ali ime stolpcev, ločenih z vejico.
Da izberemo vse, pišemo `*`.

- Distinct: Izbere unikatne vrednsoti v stolpcu (`unique`).
- max: Izbere max vrednost v stolpcu.

Primeri:

    SELECT DISTINCT stolpec
    SELECT max(stolpec)

## Count

Prešteje vrstice stolpca

    SELECT stolpec, COUNT(*)

## Union

Naredi unijo podanih stolpcev.

    SELECT stolpec1 FROM table1
    UNION
    SELECT stolpec2
    FROM table2

## Where

*Izberi* tole, *kjer* je tole.

    SELECT stolpec
    WHERE stolpec > 0
    
Kot [filter] v R.ju. Logični operatorji so generični (`<, >, >=, <=, ...`).
Enakost dobiš s `IS`, neenakost z `IS NOT`.

`AND` veriži pogoje:

    WHERE stolpec > 0 AND ...

Lahko izvajaš aritmetične operacije na podatkih:

    WHERE stolpec > 0 AND (stolpec < drugiStolpec/50.0)

## Like

Basically [grep].

    WHERE stolpec LIKE 'expression'

Expression lahko pišeš kot `%expression` (vse pred njim), ali `expression%`
(vse za njim) ali `%expression%` (vse pred ali za njim).

## In

Kot `%in%` v R-ju.

    WHERE stolpec IN (1,5)

To pomeni, da so vrednosti iz stolpca med 1 in 5.

## Order by

Dobesedno enako kot v R-ju. Orderaš po stolpcu.

    ORDER BY stolpec DESC

Ordera po stolpcu v descending.

