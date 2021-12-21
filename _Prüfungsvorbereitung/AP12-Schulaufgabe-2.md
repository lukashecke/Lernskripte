# AP-Schulaufgabe (SQL-Abfragen) am 21.12.21 um 13:15 Uhr

Die Schulaufgabe baut größtenteils auf einer **Elektromarkt**-Datenbank wie folgt auf:

![Alt text](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/datenbankmodell-elektromarkt.png)

## Mögliche Aufgaben

### Unter welcher Filial-Telefonnummer kann man Frau Annika Burstedt erreichen

```sql
SELECT f.Telefon 
FROM Filiale f RIGHT JOIN mitarbeiter m ON m.Filiale = f.Nr 
WHERE m.Name = 'Burstedt' AND m.Vorname='Annika';
```

### Welche Mitarbeiter arbeiten in einer Münchner-Filiale

```sql
SELECT m.* 
FROM mitarbeiter m INNER JOIN filiale f ON f.Nr = m.Filiale 
WHERE f.Ort LIKE '%München%';
```

### Welcher Grohßändler beliefert die Filiale in Augsburg

```sql
SELECT g.* 
FROM Filiale f
INNER JOIN beliefert b 
ON f.Nr = b.Filiale 
INNER join grosshaendler g 
ON g.Nr = b.Haendler 
WHERE f.Ort = 'Augsburg';
```

### Gesucht sind Ansprechpartner und Telefonnummer des Großhändlers aus der Branche Haushalt, der die Filiale München-Süd beliefert

```sql
SELECT g.Ansprechpartner, g.Tel 
FROM Grosshaendler g 
JOIN Beliefert b 
ON g.Nr = b.Haendler
JOIN Filiale f 
ON f.Nr = b.Filiale
WHERE g.Branche = 'Haushalt' AND f.Ort = 'München-Süd';
```

### Ermitteln Sie alle Informationen derjenigen Filialen, die von keinem Großhändler beliefert werden

```sql
SELECT f.* 
FROM Filiale f 
LEFT JOIN beliefert b 
ON f.Nr = b.Filiale 
WHERE b.Haendler IS NULL; -- b.Filiale würde auch gehen
```

### Geben Sie Name und Vorname des direkten Vorgesetzten von Mitarbeiter Lars Becker an

```sql
SELECT v.Name, v.Vorname 
FROM mitarbeiter m 
LEFT JOIN mitarbeiter v
ON m.Vorgesetzter = v.Nr 
WHERE m.Vorname='Lars' AND m.Name='Becker';
```

### Erstellen Sie eine Liste aller Mitarbeiter (Name, Vorname) mit einer neuen Spalte Direkter Vorgesetzter in der Vor- und Nachname des Vorgesetzten von jedem Mitarbeiter steht. Besitzt ein Mitarbeiter keinen Vorgesetzten, soll der Eintrag leer sein

```sql
SELECT m.Name, m.Vorname, IFNULL(CONCAT(v.Vorname, ' ', v.Name), '') AS 'Direkter Vorgesetzter' -- null mit '' ersetzen
FROM mitarbeiter m 
LEFT JOIN mitarbeiter v
ON m.Vorgesetzter = v.Nr;
```

> Diese Aufgabe wird so nicht drankommen, da die verwendeten Funktionen nicht in unserem 'SQL-Befehlssatz' vorhanden sind.

## Joins auf Papier

![Alt text](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/joins1.png)

![Alt text](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/joins2.png)

## Group By und Having

`HAVING` gibt eine Suchbedingung für eine Gruppe oder ein Aggregat an. `HAVING` kann nur mit der `SELECT`-Anweisung verwendet werden. `HAVING` wird in der Regel mit einer `GROUP BY`-Klausel verwendet.

```sql
SELECT Abteilung, COUNT(M_ID) 
FROM Mitarbeiter 
GROUP BY Abteilung 
HAVING COUNT(M_ID) > 3;
```

> Da  `WHERE` für Bedingungen nach der Gruppierung aufgrund der Abarbeitungsreihenfolge nicht verwendet werden kann, wird das neue Schlüsselwort `HAVING` benötigt.

## Count und Distinct

Duplikatelimination bei COUNT()

```sql
-- Zählt VERSCHIEDENE Mitarbeiternamen
COUNT(DISTINCT Name);

-- Zählt alle Namenseinträge (außer NULL)
COUNT(ALL Name);
COUNT(Name);

-- Zählt alle Datensätze (incl. Zeilen mit NULL-Einträgen oder komplette NULL-Zeilen)
COUNT(*);
```

## Subselects

Hier einfach mal ein paar Beispiele:

```sql
-- Listen sie alle Spieler auf, deren Rückennummer der Anzahl an österreichischen Spieler in der Mannschaft
-- Mainz 05 Profis Herren entspricht!
SELECT * 
FROM spieler s 
WHERE s.Ruecken_Nr = (
  SELECT COUNT(s.S_ID) 
 FROM spieler s 
  INNER JOIN nationalitaet n ON s.NAT=n.NAT 
  INNER JOIN mannschaft m ON s.M_ID=m.M_ID 
  WHERE n.Bezeichnung LIKE '%Österreich%' 
  AND m.MannschName LIKE '%Mainz 05 Profis Herren%'
);

-- Listen Sie alle Spieler auf, deren Alter der Länge eines Minispielfeldes entspricht oder größer ist!
SELECT * 
FROM spieler s 
WHERE TIMESTAMPDIFF(YEAR, s.GebDat, CURTIME()) >= (
  SELECT laenge 
  FROM groesse_spielfeld g 
  WHERE g.beschreibung LIKE '%Minispielfeld%'
);

-- Kombination der oberen Aufgaben:
-- Listen Sie alle Spieler auf, deren Rückennummer der Anzahl an brasilianischen Spieler in der Mannschaft
-- VfL Wolfsburg Profis Herren entspricht und deren Alter der Länge eines Minispielfeldes entspricht oder
-- größer ist!
SELECT * 
FROM spieler s 
WHERE s.Ruecken_Nr = (
  SELECT COUNT(s.S_ID) 
  FROM spieler s 
  INNER JOIN nationalitaet n ON s.NAT=n.NAT 
  INNER JOIN mannschaft m ON s.M_ID=m.M_ID 
  WHERE n.Bezeichnung LIKE '%Brasilien%' 
  AND m.MannschName LIKE '%Wolfsburg%'
)
AND 
TIMESTAMPDIFF(YEAR, s.GebDat, CURTIME()) >= (
  SELECT laenge 
  FROM groesse_spielfeld g 
  WHERE g.beschreibung LIKE '%Minispielfeld%'
);
```

## Stolpersteine

- `MIN` und `MAX` funktionieren nur in `SELECT` und `HAVING` (Workaround: Subselect)
- Das gilt übrigens für alle Aggregatsfunktionen!
- Eigene definierte Funktionen können jedoch schon außerhalb verwendet werden z.B. in der Where klausel

## Wichtige Statements

### Alle, die kein

```sql
-- Die Namen aller Lieferanten, die kein Projekt in Berlin beliefern.
SELECT l.lname 
FROM l 
WHERE l.lnr NOT IN (
  SELECT ltp.lnr 
  FROM ltp 
  INNER JOIN p ON p.pnr = ltp.pnr 
  WHERE p.ort = 'Berlin'
);
```

### Ausschließlich

```sql
-- Die Namen aller Lieferanten, die ausschließlich Projekte in Berlin
-- beliefern.
SELECT DISTINCT lname 
FROM l  
NATURAL JOIN ltp 
WHERE lnr NOT IN (
  SELECT lnr 
  FROM ltp 
  NATURAL JOIN p 
  WHERE ort != 'Berlin'
);
```

### Alle, die alle

```sql
-- Die Namen aller Lieferanten, die alle Projekte in Berlin beliefern.
-- Die Lieferanten dürfen dabei, neben allen Projekten in Berlin, noch weitere Projekte
-- beliefern.
SELECT lname 
FROM p 
NATURAL JOIN ltp 
NATURAL JOIN l 
WHERE ort = 'Berlin' 
GROUP BY lnr, lname 
HAVING COUNT(DISTINCT pnr) = (
  SELECT COUNT(pnr) 
  FROM p 
  WHERE ort = 'Berlin'
);
```

### Alter ausrechnen

Meine lieblingsfunktion ist `TIMESTAMPDIFF(YEAR, m.GebDat, CURRENT_DATE())` und wird wie folgt verwendet:

```sql
SELECT m.Name, m.Vorname, TIMESTAMPDIFF(YEAR, s.GebDat, CURRENT_DATE()) AS 'Alter'
FROM mitarbeiter m
```

### Jüngster Mitarbeiter (ohne zusätzliche Funktion)

```sql
SELECT m.name, TIMESTAMPDIFF(YEAR, m.geburtsdatum, CURRENT_DATE()) AS 'Alter' 
FROM mitarbeiter m
-- Hier wollen wir keine Funktion verwenden!
WHERE m.geburtsdatum = (
    SELECT MAX(m2.geburtsdatum) FROM mitarbeiter m2 
);
```

## Gebräuchliche Abfragen

### Höchste Artikelnummer

```sql
SELECT MAX(AID) 
FROM ARTIKEL;
```

### Billigster Artikel (nur einer)

```sql
SELECT AID, L_ID, Preis
FROM ARTIKEL
ORDER BY Preis ASC
LIMIT 1;
```

| AID | L_ID | Preis |
| --- | ---- | ----- |
| 10 | F | 1.25 |

### Billigster Artikel (einer oder mehrere)

```sql
SELECT AID, L_ID, Preis
FROM ARTIKEL
WHERE Preis = (
    SELECT MIN(Preis)
    FROM ARTIKEL
);
```

| AID | L_ID | Preis |
| --- | ---- | ----- |
| 7 | D | 1.25 |
| 10 | F | 1.25 |

### Höchster Preis je Lieferant

```sql
SELECT L_ID, MAX(Preis)
FROM ARTIKEL
GROUP BY L_ID
ORDER BY MAX(Preis) DESC;
```

| L_ID | Preis |
| ---- | ----- |
| D | 21.45 |
| A | 10.50 |
| B | 2.99 |
| E | 2.45 |
| C | 1.99 |
| F | 1.25 |

### Niedrigster Preis je Artikel/Lieferant

```sql
SELECT AID, L_ID, Preis
FROM ARTIKEL a
WHERE Preis = (
    SELECT MIN(a2.preis)
    FROM ARTIKEL a2
    WHERE a.AID = a2.AID
);
```

| AID | L_ID | Preis |
| --- | ---- | ----- |
| 1 | A | 2.45 |
| 2 | B | 2.99 |
| 3 | A | 10.50 |
| 4 | C | 1.35 |
| 5 | C | 1.99 |
| 6 | D | 2.45 |
| 6 | A | 2.45 |
| 7 | D | 1.25 |
| 8 | D | 21.45 |
| 9 | E | 2.45 |
| 10 | F | 1.25 |
