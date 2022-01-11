# IT-Schulaufgabe am 14.01.2022 um 07:30 Uhr

Themen: RAID, Backup (Generationenprinzip), Firewall

## 1. RAID

(= redundant array of indipendent disks)  
also ein Zusammenschluss unabhängiger Festplatten, der mittels [Hard- oder Software RAID](#hard--und-software-raid) realisiert wird. RAID Systeme schützen **nicht** vor Festplattenausfall! RAID-Systeme schützen vor Datenverlust bei einem Festplattenausfall (Datensicherheit).

> Für alle RAID-Systeme ist zu beachten:  
> Die Kapazität der einzelnden Festplatten ist immer nur so groß, wie die der Kleinsten Festplatte im Verbund!

### Hard- und Software RAID

Bei einem Hardware-Raid übernimmt der Raid-Controller (auf dem Motherboard), die Organisation der verschiedenen Platten. Bei einem Software-Raid übernimmt jedoch eine Software das Organisieren der Festplatten. Da hier aber keine eigene Hardware für diese Software bereitgestellt wird, geschieht dies auf der CPU.

Motherboard mit RAID Controller  
![RAID Controller](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/hardware-raid.jpg)

| Raid Typ | Vorteile | Nachteile |
| -------- | -------- | --------- |
| Hardware-Raid | schnell, effizient | Kosten, Gefahr des Defekts des Controllers |
| Software-Raid | billig, universell | System (CPU) wird belastet |

### RAID 0

Schnell

![RAID 0](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/raid-0.png)

> Vorsicht! Raid 0 bietet keinerlei Sicherheit, es handelt sich hier um eine reine Performanceoptimierung.

### RAID 1

Sicher

![RAID 1](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/raid-1.png)

### RAID 10

Schenell und Sicher

![RAID 10](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/raid-10.png)

#### Unterschied zu RAID 01

Raid 10: Erst mirroring, dann striping (mirroring "unten")
Raid 01: Erst striping, dann mirroring (striping "unten")  

![RAID 01](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/raid-01.png)

### RAID 5

![RAID 5](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/raid-5.png)

### RAID 6

![RAID 6](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/raid-6.png)

> Einsatz verschiedener Paritäten!

### Vergleich der verschiedenen Raid Systeme

| Raid-Level | Nettokapazität | Minimum an Festplatten |Vorteile | Nachteile |
| ---------- | -------------- | ---------------------- |-------- | --------- |
| Raid 0 | n * Kap | - | Performance, kein Speicherverlust (hohe Kapazität) | großes Risiko vor Datenverlust | Hardwaredefekt kann nicht kompensiert werden |
| Raid 1 | Kap | - | Kein Datenverlust bei Ausfall einer Festplatte, hohe Lesegeschwindigkeit | viel Speicherverlust, hohe Kosten |
| Raid 10 | (n / 2) * Kap | 4 | Performance, Ausfallsicherheit | Netto Kapazität gering, Kosten |
| Raid 5 | (n - 1) * Kap | 3 | Ausfall einer HDD abgefangen, Lesen schneller, schreiben etwas* | Kapazitätsferlust einer Festplatte |
| Raid 6 | (n - 2) * Kap | 4 | Ausfall zweier HDD möglich, Lesen schneller, schreiben etwas* | Kapazitätsferlust zweier Festplatten |

Kap = Kapazität der kleinsten Festplatte im Verbund

\* wegen striping, Raid 5 > Raid 6 weil 2 Paritäten

### Parity Block

Dient zur Wiederherstellung der verlorenen Daten einer, bzw. zweier Festplatten (Raid 5 oder 6). Ist auf alle Festplatten verteilt, da Wiederherstellung schneller, weil zu erst nur die Nutzdaten berechnet werden können, anschließend die fehlenden Paritäten.

### Weitere Begriffe

#### JBOD

(= just a bunch of discs)  
Also ein Zusammenschluss von Festplatten, der keinem klassischen Raid Level entspricht. Z.b. wenn größere Festplatte benötigt als vorhanden?

![JBOD](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/jbod.png)

#### Matrix RAID

Kombination aus Raid 0 und 1, die flexibel einstellbar ist.

#### Hot - Swap

Austausch eines Speichermediums im laufendem Betrieb, bzw. die Möglichkeit dazu.

#### Hot - Spare

Reservefestplatte, die bei Ausfall einer anderen Festplatte automatischaktiv wird. Im gegensatz zum Hot Swap bereits eingebaut.

## 2. Generationenprinzip

Im Generationsprinzip soll

![Generationsprinzip](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/generationsprinzip.png)

> 4 Wochentage + 4 Wochen + 12 Monate = 20 Bänder

## 3. Backups (Wiederholung)

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

## 4. Firewall

Noch nicht unternommen...

## Weblinks

- [Seagate.com](https://www.seagate.com/de/de/manuals/network-storage/business-storage-nas-os/raid-modes/) - RAID Modi
- [codefieber.de](https://www.codefieber.de/it-sicherheit/sicherungsarten-und-backupverfahren-1804) - Sicherungsarten und Backupverfahren
- [chip.de](https://praxistipps.chip.de/software-oder-hardware-raid-wo-liegt-der-unterschied_28803) - Hardware- vs. Software RAID

## Wiederholung Network Layer (nicht SA relevant!)

![Network Layer](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/network-layer.png)
