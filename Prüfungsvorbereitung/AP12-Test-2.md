# AP-Test (DDL/DML) am 26.10.21 um 10:15 Uhr

Die gesamte Kurzarbeit baut wahrscheinlich auf einer **Supermarkt**-Datenbank auf.

## Mögliche Aufgaben

### 1. Datenbank und Tabellen erstellen (DDL)

Datenbank anlegen:

```sql
DROP DATABASE IF EXISTS Lebensmittel; -- best practice
CREATE DATABASE Lebensmittel CHARACTER SET utf8;
```

User anlegen:

```sql
DROP USER IF EXISTS 'lebensmittelTest';
CREATE USER IF NOT EXISTS 'lebensmittelTest' @localhost;
GRANT ALL PRIVILEGES ON Lebensmittel.* TO 'lebensmittelTest' @localhost;
```

Tabellen anlegen:

```sql
USE Lebensmittel; -- nicht vergessen!

CREATE TABLE IF NOT EXISTS Angestellter (
    anr INT AUTO_INCREMENT PRIMARY KEY,
    aname VARCHAR(50) NOT NULL,
    abteilung INT
);

CREATE TABLE IF NOT EXISTS Ware (
    wnr INT AUTO_INCREMENT PRIMARY KEY,
    wname VARCHAR(50) NOT NULL,
    kategorie VARCHAR(50),
    gewicht FLOAT CHECK(gewicht>0),
    preis FLOAT(4,2) UNSIGNED
);

CREATE TABLE IF NOT EXISTS Supermarkt (
    snr INT AUTO_INCREMENT PRIMARY KEY,
    sname VARCHAR(50) NOT NULL,
    ort VARCHAR(50)
);

CREATE TABLE IF NOT EXISTS Aws (
    PRIMARY KEY (anr, wnr, snr),
    anr INT,
    wnr INT,
    snr INT,
    menge INT CHECK(menge>0)
);
```

Fremdschlüssel setzen:

```sql
ALTER TABLE Aws ADD FOREIGN KEY (anr) REFERENCES Angestellter(anr);
ALTER TABLE Aws ADD FOREIGN KEY (wnr) REFERENCES Ware(wnr);
ALTER TABLE Aws ADD FOREIGN KEY (snr) REFERENCES Supermarkt(snr);
```

### 2. Verändern der Datenbankstruktur (DDL)

```sql
-- Tabelle hinzufügen (mit Deaultwert)
ALTER TABLE Angestellter ADD COLUMN 
stunden INT DEFAULT 7;

-- Löschen einer Spalte
ALTER TABLE Angestellter DROP COLUMN
abteilung;

-- Datentyp ändern
ALTER TABLE Ware MODIFY gewicht int;
ALTER TABLE Ware ADD CONSTRAINT checkGewicht CHECK(gewicht>0);
```

### 3. Hinzufügen von Werten (DML)

```sql
INSERT INTO Angestellter (aname)
VALUES ('Huber');

INSERT INTO Ware (wname, kategorie, gewicht, preis)
VALUES ('Apfel', 'Obst', 10, 4.10);

INSERT INTO Supermarkt (sname, ort)
VALUES ('Rewe', 'Kiefersfelden');

INSERT INTO Aws (anr, wnr, snr)
VALUES ( -- dynamische Ermittlung
    (select anr FROM Angestellter WHERE aname = 'Huber'),
    (select wnr FROM Ware WHERE wname = 'Apfel'),
    (select snr FROM Supermarkt WHERE sname = 'Rewe')
);
```

### 4. Änderungen von Werten (DML)

```sql
UPDATE Ware
SET preis = 4.20
WHERE wname='Apfel'; 
```

> Irgendwas darf hier nie `null` sein!

### 5. Tabellen löschen (DDL)

Bei der Löschreihenfolge ist es wichtig, immer die **Fremdschlüsseltabellen** als erstes zu löschen, um die **Datenintegrität** zu jedem Zeitpunkt gewährleisten zu können.

```sql
DROP TABLE Aws; -- FK müssen als erstes gelöscht werden!
DROP TABLE Angestellter;
DROP TABLE Ware;
DROP TABLE Supermarkt;
```

#### DROP vs. DELETE

DROP (DDL) löscht gesamte Tabellen, während mit DELETE (DML) nur Datensätze gelöscht werden. Außerdem kann nur DELETE an eine Bedingung geknüpft werden.

```sql
-- Tabelle wird gelöscht
DROP TABLE Ware;

-- Inhalte werden gelöscht
DELETE FROM Ware 
WHERE kategorie='nicht_mehr_im_Sortiment';
```

### Sonstiges 

Was noch so übrig war an Aufgaben...

```sql
-- Gesucht sind alle Informationen von Sonja Kaufmann und Michael Wolff.
SELECT * 
FROM mitarbeiter 
WHERE name ='Lorenz' AND vorname = 'Sophia' OR name ='Wolff' AND vorname = 'Michael';

-- Erstellen Sie eine alphabetisch sortierte Liste der Mitarbeiter (Name und Vorname).
-- Dabei soll nach nach dem Nachnamen absteigend sortiert werden. Bei gleichen Nachnamen
-- soll aufsteigend nach dem Vornamen sortiert werden.
SELECT name, vorname 
FROM mitarbeiter 
ORDER BY name DESC, vorname ASC;
```

## Theorie und weitere Befehle

### DDL und DML

DLL und DML sind zwei sehr wichtige Bestandteile von SQL. Es gibt aber noch andere, wie z.B. DCL, DQL und TCL.

| Kürzel | Begriff | Funktion |
| ------ | ------- | -------- |
| DDL | Data Definition Language | Modellierung der Datenbank |
| DML | Data Manipulation Language | Manipulation der Daten |

### Erläuterung der Standartbefehle

| Befehl | Funktion |
| ------ | -------- |
| SELECT | Welche Spalten? -> Projektion |
| FROM | Welche Tabelle(n)? |
| WHERE | Bedingung(en) mit AND/ OR verknüpfen -> Selektion |
| ORDER BY | Sortieren nach Werten (ASC: aufsteigend, DESC: absteigend) |
| LIMIT | Begrenzt die Anzahl der Ergebniszeilen (nach möglicher Sortierung) |

Eine beispielhafte Abfrage könnte also wie folgt aussehen: 

```sql
SELECT Name  
FROM Mitarbeiter  
ORDER BY Name  
LIMIT 5;
```

Die ersten 5 Mitarbeiternamen werden aufsteigend sortiert ausgegeben.

### Arbeiten mit Datumswerten

Daten müssen in Anführungszeichen geschrieben werden und in einem sortierbaren Datumsformat vorliegen (i.d.R. **YYYY-MM-DD**).

```sql
-- Filtern nach Jahr
SELECT id, name, vorname 
FROM mitarbeiter 
WHERE YEAR(eingestellt) = '2006';

-- Einschränken zwischen zwei Daten
SELECT id, name, vorname 
FROM mitarbeiter 
WHERE eingestellt BETWEEN '2001-01-01' AND '2001-03-31';
```

### Like

Wird verwendet um quasi Substrings zu finden:

```sql
-- Findet Rotwein
SELECT * 
FROM produkt 
WHERE bez LIKE "%Wein";

-- Findet Weinschorle
SELECT * 
FROM produkt 
WHERE bez LIKE "Wein%";

-- Findet Rotwein und Weinschorle
SELECT * 
FROM produkt 
WHERE bez LIKE "%Wein%";
```

Das `%`-Zeichen funktioniert hier wie der `*` in der regulären Suche und in Regex.

### Concat und Aliasse

Dieser Befehl:

```sql
SELECT CONCAT(strasse, hausnr) AS 'Adresse'
FROM Mitarbeiter;
```

macht aus dem hier:

| Strasse | Hausnummer | ... |
| ------- | ---------- | --- |
| Musterstraße | 42 | ... |
| Georg-Bräuchle-Ring | 1 | ... |

das hier:

| Adresse | ... |
| --------| --- |
| Musterstraße 42 | ... |
| Georg-Bräuchle-Ring 1 | ... |
