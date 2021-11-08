# SQL-Einstieg 2

## Abarbeitungsreihenfolge

Zu beachten ist nun zusätzlich die Reihenfolge der Abarbeitung von SQL-Anweisungen:

1. FROM
2. WHERE
3. GROUP BY
4. HAVING
5. SELECT
6. ORDER BY
7. LIMIT

> Ein häufiges Fehler in SQL ist der Versuch, eine `WHERE`-Klausel zum Filtern von Aggregationen zu verwenden, was gegen die SQL-Ausführungsregeln verstößt.  
> Dies liegt daran, dass bei der Auswertung der `WHERE`-Klausel die `GROUP BY`-Klausel noch ausgeführt werden muss und die aggregierten Werte unbekannt sind. Daher schlägt die folgende Abfrage fehl:

```sql
-- ACHTUNG FEHLER!
SELECT country, SUM(area) 
FROM countries 
WHERE SUM(area) > 1000 
GROUP BY 1;
```

## Aggregatsfunktionen

Aggregatfunktionen führen Berechnungen für verschiedene Werte durch und geben einen einzelnen Wert zurück.Aggregatfunktionen werden häufig mit der `GROUP BY`-Klausel der `SELECT`-Anweisung verwendet.

> Alle Aggregatfunktionen, außer `COUNT()`, ignorieren NULL-Werte.

### SUM()

Gibt die Summe aller Werte im Ausdruck zurück. `SUM` kann nur bei numerischen Spalten verwendet werden.

```sql
SELECT SUM(Gehalt) 
FROM Mitarbeiter;
```

### COUNT()

Diese Funktion gibt die Anzahl der in einer Gruppe gefundenen Elemente zurück.

```sql
SELECT COUNT(M_ID) 
FROM Mitarbeiter;
```

#### Duplikatelimination bei COUNT()

```sql
-- Zählt VERSCHIEDENE Mitarbeiternamen
COUNT(DISTINCT Name);

-- Zählt alle Namenseinträge (außer NULL)
COUNT(ALL Name);
COUNT(Name);

-- Zählt alle Datensätze (incl. Zeilen mit NULL-Einträgen oder komplette NULL-Zeilen)
COUNT(*);
```

### MIN()

Gibt den kleinsten Wert im Ausdruck zurück.

```sql
SELECT MIN(Gehalt) 
FROM Mitarbeiter;
```

### MAX()

Gibt den größten Wert im Ausdruck zurück.

```sql
SELECT MAX(Gehalt) 
FROM Mitarbeiter;
```

### AVG()

Diese Funktion gibt den Mittelwert der Werte in einer Gruppe zurück.

```sql
SELECT AVG(Gehalt) 
FROM Mitarbeiter;
```

## Weitere Klauseln

### GROUP BY

Die `GROUP BY`-Klausel gruppiert Zeilen mit den gleichen Werten in Zusammenfassungszeilen.  
Sie wird häufig mit [Aggregatfunktionen](#aggregatsfunktionen)  verwendet, um die Ergebnismenge nach einer oder mehreren Spalten zu gruppieren.

```sql
SELECT COUNT(M_ID), Abteilung 
FROM Mitarbeiter 
GROUP BY Abteilung;
```

> Achtung!  
> Durch ein `GROUP BY` wird das `SELECT` eingeschränkt.

### HAVING

Gibt eine Suchbedingung für eine Gruppe oder ein Aggregat an. `HAVING` kann nur mit der `SELECT`-Anweisung verwendet werden. `HAVING` wird in der Regel mit einer `GROUP BY`-Klausel verwendet.

```sql
SELECT Abteilung, COUNT(M_ID) 
FROM Mitarbeiter 
GROUP BY Abteilung 
HAVING COUNT(M_ID) > 3;
```

> Da  `WHERE` für Bedingungen nach der Gruppierung aufgrund der [Abarbeitungsreihenfolge](#abarbeitungsreihenfolge) nicht verwendet werden kann, wird das neue Schlüsselwort `HAVING` benötigt.
