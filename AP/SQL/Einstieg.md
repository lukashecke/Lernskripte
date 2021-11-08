# SQL-Einstieg

SQL ist eine Datenbanksprache zur Definition von Datenstrukturen in relationalen Datenbanken sowie zum Bearbeiten (Einfügen, Verändern, Löschen) und Abfragen von darauf basierenden Datenbeständen. 

## Syntax

Select...  
From...  
Where...  
Order By...  
Limit...  

## Beispielabfrage

Eine beispielhafte Abfrage könnte wie folgt aussehen:

```sql
SELECT Name  
FROM Mitarbeiter  
ORDER BY Name  
LIMIT 5;
```

Die ersten 5 Mitarbeiternamen aufsteigend sortiert.

## Erläuterung

| Befehl | Funktion |
| ------ | -------- |
| SELECT | Welche Spalten? -> Projektion |
| FROM | Welche Tabelle(n)? |
| WHERE | Bedingung(en) mit AND/ OR verknüpfen -> Selektion |
| ORDER BY | Sortieren nach Werten (ASC: aufsteigend, DESC: absteigend) |
| LIMIT | Begrenzt die Anzahl der Ergebniszeilen (nach möglicher Sortierung) |
