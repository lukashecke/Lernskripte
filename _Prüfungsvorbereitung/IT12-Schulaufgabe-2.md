# IT-Schulaufgabe am 04.04.2022 um 09:00 Uhr

Themen: Firewall, Malware, VPN

## 1. Firewall

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

### Demilitarisierte Zone (DMZ)

Ein Bereich zwischen WAN und LAN in dem Komponenten liegen, welche von außen zugreifbar sein sollen, z.B. Web- und Mailserver.

## 2. Malware

Als Schadprogramm, Schadsoftware oder zunehmend als Malware, bezeichnet man Computerprogramme, die entwickelt wurden, um unerwünschte und gegebenenfalls schädliche Funktionen auszuführen.

### Gängige Malware Typen

siehe Skript!

### Besonders wichtig

#### Ransomware

Ransomware (auch Kryptotrojaner), sind Schadprogramme, mit deren Hilfe ein Eindringling den Zugriff des Computerinhabers auf Daten, deren Nutzung oder auf das ganze Computersystem verhindern kann. Dabei werden private Daten auf dem fremden Computer verschlüsselt oder der Zugriff auf sie verhindert, um für die Entschlüsselung oder Freigabe ein Lösegeld zu fordern.

#### Wurm vs. Virus

Würmer können sich selbstständig und unabhängig über ein Netz verbreiten, wohingegen ein Virus einen Wirten benötigt (z.B. eine exe-Datei) und manuell ausgeführt werden muss.

#### Botnet

Ein Botnet ist eine Gruppe automatisierter Schadprogramme, sogenannter Bots. Diese laufen auf vernetzten Rechnern, deren Netzwerkanbindung sowie lokale Ressourcen und Daten ihnen, ohne Einverständnis des Eigentümers, zur Verfügung stehen.

## 3. VPN

(= virtual private network)

## Tunneln

Mit Tunneln wird das nochmalige Einkapseln der Daten in ein weiteres Protokoll bezeichnet. Dies kann au verschiedenen Schichten passieren, z.B. OSI 5-7 (HTTPs, SSH, OpenVPN) oder OSI 3 (IPsec).

### 3 Arten

#### Site-to-Site

Zusammenschluss mehrerer LANs über ein entsprechendes VPN-Gateway.

#### End-to-End

Direkte Verbindung zwischen mehreren Arbeitsrechnern.

#### End-to-Site

Verbindung zwischen externem Endgerät und einem VPN-Gateway des Firmen-LANs.

### IPsec (und deren 2 Protokolle)

Ein erweitertes IP-Protokoll um Authentisierung, Integritätsprüfung und Verschlüsselung.

#### AH-Protokoll (IP Authentication Header)

Ermöglicht die Prüfung von Integrität und Authentizität von Daten

> Bei diesem Protokoll findet keine Datenverschlüsselung statt.

#### ESP Protokoll (IP Encapsulated Security Payload)

Verschlüsselt die Daten (Vertraulichkeit)

### Modi

#### Transportmodus (Nur bei End-to-end)

Verbindet zwei Rechner direkt. Nutzlast (Payload) wird verschlüsselt.

#### Tunnelmodus

Das ursprüngliche Paket wird gekapselt und die Sicherheitsdienste von IPsec auf das gesamte Paket angewandt.

> Zusätzlicher (äußerer IP-Header) benötigt!

## Weblinks

- [youtube.com](https://www.youtube.com/watch?v=kDEX1HXybrU) - Was ist eine Firewall?
- [Wikipedia.org](https://de.wikipedia.org/wiki/OSI-Modell) - OSI-Modell

## Wiederholung OSI-Modell

![OSI-Modell](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/osi-model.png)

## Wiederholung TCP-Handshake

![TCP-Handshake](https://github.com/lukashecke/Lernskripte/blob/master/_Assets/tcp-handshake.png)
