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

Besonders für große Festplatten-Arrays, da bei vielen Festplatten auch die wahrscheinlichkeit des Ausfalls steigt.

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
Also ein Zusammenschluss von Festplatten, der keinem klassischen Raid Level entspricht.  
Beispielsweise für große Datenbanken, Fileserver, etc. für die einzelnde Festplatten nicht ausreichen.

![JBOD](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/jbod.png)

#### Hot - Swap

Austausch eines Speichermediums im laufendem Betrieb, bzw. die Möglichkeit dazu.

#### Hot - Spare

Reservefestplatte, die bei Ausfall einer anderen Festplatte automatischaktiv wird. Im gegensatz zum Hot Swap bereits eingebaut.

## 2. Generationenprinzip

Das Generationsprinzip (auch Großvater-Vater-Sohn-Prinzip) stellt sicher, dass immer mehrere Sicherungen in verschiedenen zeitlichen Abstufungen (Großvater, Vater, Sohn) vorhanden sind.  
In diesem Beispiel, soll auf jede Woche und jeden Monat zurückgespielt werden können. Die Tagesbänder werden alle 7 Tage überschrieben und am Freitag wird immer das Wochenbackup eingespielt.

![Generationsprinzip](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/generationsprinzip.png)

> 4 Wochentage ([inkrementell](#wiederholung-backups)) +  
> 4 Wochen ([full](#wiederholung-backups)) +  
> 12 Monate ([full](#wiederholung-backups))  
> = 20 Bänder

## 3. Firewall

Hard- oder Software, die anhand von Regeln Netzwerk-Verkehr erlaubt oder verbietet.

> Schütz interne Netze nur vor Angriffen aus einem externen Netz, nicht von innen!

### Kriterien für Firewall-Regeln

Firewall-Regeln können auf Folgendem basieren:

- IP Adressen ([OSI 3](#wiederholung-osi-modell))
- Ports ([OSI 4](#wiederholung-osi-modell))
- Domain names
- Protokolle
- Programme ([OSI 7](#wiederholung-osi-modell))
- Schlüsselworter ([OSI 7](#wiederholung-osi-modell))

> Paket-Filter nennt sich die Art von Firewall, die die ein- und ausgehenden Datenpakete auf der Netzwerk- und Transportebene kontrolliert. ([OSI-Schichten 3 und 4](#wiederholung-osi-modell))

Die Firewall Regeln werden der Reihe nach abgearbeitet:

| Regel | Bedingung | Aktion |
| ----- | --------- | ------ |
| Regel 1 | ... | erlauben |
| Regel 2 | ... | verbieten |
| Regel 3 | ... | erlauben |
| ... | ... | ... |
| Regel n | ... | erlauben/ verbieten |

Mögliche Aktionen (Linux):

- Accept - lässt das Paket passieren
- Drop - lehnt das Paket ohne Rückmeldung ab
- Reject - lehnt das Paket ab und schickt dem Absender per ICMP die Rückmeldung *destination unreachable*
- Log - Paket im Logfile des Systems protokollieren

> Die Regelprüfung findet nur bis zur 1. erfülten Bedingung statt. Diese Aktion wird dann ausgeführt und die Regelprüfung beendet.  
> Das heißt die erste passende Regel, die auf das Paket zutrifft wird angewendet.

### Black- vs. Whitelist

#### Blacklist

| Regel | Bedingung | Aktion |
| ----- | --------- | ------ |
| Regel 1 | ... | verbieten |
| Regel 2 | ... | verbieten |
| Regel 3 | ... | verbieten |
| ... | ... | ... |
| Letzte Regel | ... | alles (andere) erlauben |

#### Whitelist

| Regel | Bedingung | Aktion |
| ----- | --------- | ------ |
| Regel 1 | ... | erlauben |
| Regel 2 | ... | erlauben |
| Regel 3 | ... | erlauben |
| ... | ... | ... |
| Letzte Regel | ... | alles (andere) verbieten |

### SPI-Firewall ([OSI 4](#wiederholung-osi-modell))

(= stateful \[packet\] inspection firewall)  
Pakete werden nur reingelassen, wenn jemand von innen eine Verbindung aufgebaut hat.  
Dies funktioniert nicht über den [TCP-Handshake](#wiederholung-tcp-handshake), sondern über Listen. Die IP-Adresse mit der kommuniziert werden soll, wird sich gemerkt, Daten davon durchgelassen und im Anschluss wird diese wider "blockiert".

### Application-Layer Firewall ([OSI 7](#wiederholung-osi-modell))

(o.a. Proxy-Level-Firewall)  
Weil diese Firewall bis Schicht 7 arbeitet, kann der Dateninhalt gelesen werden.  
Durch dieses **Content-Filtering** können zum Beispiel alle Mails mit bestimmten Spam-Inhalt blockiert werden.

> Achtung!  
> Proxy-Layer-Firewall ist nicht das selbe wie ein Proxy.  
> Eine Proxy-Level-Firewall ist auch kein Virensacanner: Virenscanner suchen nach bestimmten Mustern, welche laufend aktualisiert werden müssen.

## Weblinks

- [Seagate.com](https://www.seagate.com/de/de/manuals/network-storage/business-storage-nas-os/raid-modes/) - RAID Modi
- [codefieber.de](https://www.codefieber.de/it-sicherheit/sicherungsarten-und-backupverfahren-1804) - Sicherungsarten und Backupverfahren
- [chip.de](https://praxistipps.chip.de/software-oder-hardware-raid-wo-liegt-der-unterschied_28803) - Hardware- vs. Software RAID
- [youtube.com](https://www.youtube.com/watch?v=kDEX1HXybrU) - Was ist eine Firewall?
- [Wikipedia.org](https://de.wikipedia.org/wiki/OSI-Modell) - OSI-Modell

## Wiederholung Backups

= Die Sicherung aller Daten auf ein **externes** Medium.

### Full Backup

Sicherung aller Daten auf ein externes Medium.

### Inkrementelles Backup

Zunächst Full-Bakup.  
Anschließend werden nur noch **die Änderungen zum Letzten Backup** gesichert.

### Differentielles Backup

Zunächst Full-Bakup.  
Anschließend werden nur noch **die Änderungen zum Letzten Full-Backup** gesichert.

![Alt text](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/backups.png)

## Wiederholung OSI-Modell

![Network Layer](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/osi-modell.png)

## Wiederholung TCP-Handshake

![TCP-Handshake](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/tcp-handshake.png)
