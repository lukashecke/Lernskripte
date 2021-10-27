# SQL-Joins

## 1) Cross-Join

Der Cross Join (auch als Kartesisches Produkt oder Kreuzprodukt bezeichnet) verbindet jede Zeile der ersten Tabelle mit jeder Zeile der Zweiten Tabelle. Die Ergebnistabelle eines Cross Joins kann sehr groß werden und ist häufig nutzlos. 

```sql
-- implizit
SELECT * 
FROM Mitarbeiter, Abteilung;

-- explizit
SELECT * 
FROM Mitarbeiter 
CROSS JOIN Abteilung;
```

## 2) (Inner) Join

Der Inner Join verbindet Datensätze aus zwei Tabellen, welche in beiden Tabellen dieselben Werte enthalten. Die Spalten die in beiden Tabellen verglichen werden sollen, müssen explizit angegeben werden.

```sql
SELECT m.Name, a.Name 
FROM Mitarbeiter m 
INNER JOIN Abteilung a 
ON m.ANr = a.ANr;
```

> Dies ist auch das "normale" Join. Das `INNER` Keyword ist hier optional.

## 3) Natural Join

Der Natural Join verknüpft die beiden Tabellen über die gleichheit der Felder, in Spalten mit gleichem Namen. Spalten mit gleichem Namen werden im Ergebnis nur einmal angezeigt. Haben die Tabellen keine Spalten mit gleichem Namen, wird der Natural Join automatisch zum Cross Join.

```sql
-- automatisches Mapping
SELECT m.Name, a.Name 
FROM Mitarbeiter m 
NATURAL JOIN Abteilung a;
```

## 3) Right Join

Der Right Join (auch Right Outer Join genannt) erstellt eine so genannte rechte Inklusionsverknüpfung. Diese schließt alle Datensätze aus der zweiten (rechten) Tabelle ein, auch wenn keine entsprechenden Werte für die Datensätze in der ersten (linken) Tabelle existieren. Die zu vergleichenden Spalten müssen explizit Angegeben werden.

```sql
SELECT *
FROM Mitarbeiter m 
RIGHT JOIN Abteilung a 
ON m.ANr = a.ANr;
```

## 3) Left Join

Der Left Join (auch Left Outer Join genannt) erstellt eine so genannte linke Inklusionsverknüpfung. Diese schließt alle Datensätze aus der ersten (linken) Tabelle ein, auch wenn keine entsprechenden Werte für die Datensätze in der zweiten (rechten) Tabelle existieren. Die zu vergleichenden Spalten müssen explizit Angegeben werden. 

```sql
SELECT *
FROM Abteilung a 
LEFT JOIN Mitarbeiter m 
ON m.ANr = a.ANr;
```

## 4) Self Join

Der Self Join dient dazu, um in bestimmten Situationen einen Verbund innerhalb einer einzigen Tabelle bilden zu können. Dazu muss man der Tabelle zwei verschiedene Aliasnamen geben. Self Joins können mit allen Joins durchgeführt werden. Ein Beispiel für den Gebrauch eines Self Joins ist zum Beispiel ein Vergleich innerhalb einer Tabelle. 

```sql
SELECT m1.Name, m2.Name
FROM mitarbeiter m1 
LEFT JOIN mitarbeiter m2 
ON m1.VorgesetzterId = m2.Id;
```

## 5) Full Join

Der Full Join (auch Full Outer Join genannt) ist eine Kombination von Left Join und Right Join. Die zu vergleichenden Spalten müssen explizit Angegeben werden. 

```sql
SELECT * 
FROM Mitarbeiter m 
FULL JOIN Abteilung a 
ON m.ANr = a.ANr;
```
