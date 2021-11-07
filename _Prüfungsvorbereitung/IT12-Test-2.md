# IT-Test am 08.11.21 um 13:15 Uhr

## 1. Verschlüsselung

### 1.1. Symmetrische Verschlüsselung

Verschlüsselungsverfahren, bei denen jeweils der selbe Schlüssel für Ver- und Entschlüsselung verwendet wird (z.B. AES mit 182 - 256 Bit).

#### Vorteile

- Sehr sicher nach Schlüsselübermittlung

- schnelle Berechenbarkeit

- kurze Schlüssel

#### Nachteile

- Schlüsselverteilungsproblem  
Wie kann man einen sicheren Schlüssel über einen unsicheren Kanal (Internet) übertragen?

- Anzahl benötigter Schlüssel  
Die Anzahl der Schlüssel wächst mit jedem weiteren Kommunikationsteilnehmer beträchtlich an. So werden bei n=5 Teilnehmern schon **n(n-1)/2** = 10 Schlüssel benötigt.

### 1.2. Asymmetrische Verschlüsselung

Verschlüsselungsverfahren, bei denen sich die Schlüssel für Ver- und Entschlüsselung unterscheiden (z.B. RSA mit 1024 - 4096 Bit).  
Meist wird der öffentliche Schlüssel zum Verschlüsseln, der geheime Schlüssel zum Entschlüsseln verwendet.

#### Vorteile

- Öffentlicher und privater Schlüssel unabhängig voneinander  
Privater kann nicht berechnet werden, wenn öffentlicher im Besitz.

- Schlüsselverteilungsproblem gelöst  
Der Transport des öffentlichen Schlüssels über einen  unsicheren Kanel (z.B. Internet) ist unkritisch.

#### Nachteile

- Sehr rechen- und Zeitintensiv  
1.000 bis 10.000 mal langsamer als symmetrisches Verfahren.

- Lange Schlüssel benötigt (min. 1024 Bit)  
Angreifer versuchen, aus dem Public Key den Private Key zu berechnen.

### 1.3. Hybride Verfahren

Kombinieren die symmetrische und die asymmetrische Verschlüsselung und vermeiden jeweils deren Nachteile. Die zu übertragende Nachricht wird symmetrisch verschlüsselt. Der dafür nötige Schlüssel wird vorher asymmetrisch verschlüsselt übertragen (z.B. SSH, HTTPS)

### Schlüsselraum

Schlüsselraum ist ein Begriff aus der Kryptographie und bezeichnet die Menge aller für ein Verschlüsselungsverfahren möglichen Schlüssel.

> Berechnung:  
2^n Schlüssel bei n Bits

### Zeitberechnung

Schlüsselraum: 2^64
Berechnungsrate: 10^11 Schlüssel pro Sekunde

Lösung: (2^64) / (10^11) = 5,8 Jahre;

> Formel:  
> Benötigte Zeit = Schlüsselraum / Berechnungsrate

## 2. Digitale Signatur

![Alt text](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/digitale-signatur.png)

## 3. Backups

= Die Sicherung aller Daten auf ein **externes** Medium.

### 3.1. Full Backup

Sicherung aller Daten auf ein externes Medium.

### 3.2. Inkrementelles Backup

Zunächst Full-Bakup.  
Anschließend werden nur noch **die Änderungen zum Letzten Backup** gesichert.

### 3.3 Differentielles Backup

Zunächst Full-Bakup.  
Anschließend werden nur noch **die Änderungen zum Letzten Full-Backup** gesichert.

![Alt text](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/backups.png)

### Zeitberechnung

Datenvolumen: 6,2 GiB  
Datenübertragungsrate: 3 MB/s

Lösung: (6,2 \* 1024^3 B) / (3 \* 10^6 B/s) = 2220s;

> Formel:  
> Benötigte Zeit = Datenvolumen / Datenübertragungsrate

### Verfügbarkeit

> Formel:  
> Verfügbarkeit = (Gesamtzeit - Gesamtausfallzeit) / (Gesamtzeit) \* 100%
