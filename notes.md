Ablauf

ipv6 history

- wird von IETF (Internet Engineering Task Force) seit 1995 entwickelt
- hieß ursprünglich IP Next Generation
- Beim Design wurden viele Probleme von IPv4 behoben:
	- z.B ist kein NAT mehr notwendig und im Design auch nicht mehr erwünscht(Ende zu Ende wieder möglich)
	- Protokolle wie FTP und SIP sind dadurch einfacher zu benutzen
- IPv4 Addressknappheit ist derzeit das Hauptargument für die Umstellung.
- IPv6 hat aber mehr zu bieten

ipv4 erklären

IP Adressen wurden früher in Klassen eingeteilt

Class A: 8 Bit
Class B: 16 Bit
Class C: 24 Bit
Class D: Multicast
Class E: reserved(Experimental)

Die Klasse bestimmt im wesentlichen wieviele Hosts in diesem Netz vorhanden sind und wo die Adressen anfangen und aufhören.

Das kleinste Netz ist damit Class C mit 256 Hosts. Für die Verteilung eher schlecht. Daher wurde CIDR eingeführt und die Klassen werden heute nicht mehr benutzt.

CIDR - Classless Inter-Domain Routing

192.168.1.0/24 wäre ehemals ein Class C mit dem Subnet 255.255.255.0
Mit CIDR ist auch eine /31 mit 2 Hosts möglich.

Der Netzteil wird ähnlich wie bei Postleitzahlen genutzt. Für den Router ist nur der Netzteil der Adresse interessant, weil er danach die Route bestimmt.

Die ursprüngliche Idee vom einfach Routing durch Strukturierung des Adressraums hat leider nicht funktioniert und die Routingtabellen bei Provider sind trotzdem riesig und unübersichtlich.

Außerdem ist durch die Vergabe von IPv4 Adressen eine große Menge verschwendet worden. Vergabe von /8 an Apple(17.x.x.x), MS, US Mil, etc.
Desweiteren sind die Adressen, die auf 0 oder 255 enden auch nicht benutzbar. (Netz/Broadcast)



den RIRs (Regional Internet Registry) gehen die Adressen aus

RIPE (Europe)
ARIN (North America)
APNIC (Asia - Pacific)
LACNIC (Latin America and Caribbean)
AfriNIC (African)

Das letzte /8 wurde angebrochen. Wer neue IPv4 Adressen haben will, muss auch immer ein IPv6 dazunehmen.

Ripe Pool Picture 




Änderungen unter der Haube

- Adressen sind jetzt 128 Bit lang. Damit sind 2 hoch 128 Adressen möglich.
- Urban Legend: Jedes Atom kann eine IPv6 Adresse haben. > geht nicht, falls man davon aus geht, dass die Erde einen Eisenkern hat. Aber für jedes Sandkorn würde es reichen.



IPv6 Header (320 Bit)

BILD

*Version (4 bits) 01110 = 6

*Traffic Class (8bits) = Traffic Art Bestimmung

*Flow Label (16 Bits) = auf non-zero setzen, um dem Router zu sagen, dass er die Reihenfolge der Paket nicht verändern soll

*Payload Length (16 bits) = Größe des Payloads mit Extension Header

*Next Header (8 bits) = Gibt die Art des nächsten Headers an. Wichtig falls Extension Header benutzt werden.

*Hop Limit (8 bits) = wird um 1 dekrementiert und bei 0 wirft der Router das Paket weg.

TTL (8Bit) ist auch raus. TTL wurde pro Sekunde runterzählt und von jedem Router um eins dekrementiert. Dafür gibt es jetzt das Hoplimit

*Source Address (128 bits) 

*Destination Address (128 bits)


Zusatzinfos werden nicht mehr im Eigentlichen Header untergebracht, sondern bei Bedarf als Extension Header hinter dem Fixed Header eingefügt. Falls ein Router den Ext. Header nicht versteht, wird das Paket gedroppt und ein Parameter Problem message (ICMPv6 type 4, code 1) zurückgeschickt.

Rausgefallen ist die Checksum, weil die jedesmal bei Änderung der TTL vom Router neu erzeugt werden muss und deshalb Performance frist.


Fun Fact: Mit IPv6 kann man ein Jumbogram versenden. Payload fast 4GB. (Muss im Transport Layer implementiert sein)


Fragmentation

Im Gegensatz zu IPv4 werden Router IPv6 Pakete nicht fragmentieren. Dafür ist der Client zuständig. MTU Path Discovery ist daher Pficht. ICMPv6 blocken bedeutet daher Totalausfall, weil der Client nicht bestimmen kann auf welche Größe er die Paket zuschneiden muss.

MTU Ethernet 1500. DSL 1492 (8 BIT PPPOE)


Adressvergabe auf dem Interface

Mit IPv6 hat ein Interface sofort eine IPv6 Adresse. Auch falls kein Kabel angesteckt ist. 
Dafür ist fe80::/10 reserviert.

Link Local ist bei IPv6 Pflicht!
Link Local gilt nur in Zusammenhang mit einem bestimmten Interface und wird nicht geroutet.

fe80::7ed1:c3ff:fe76:59d0%en1 (%en1 ist das Interface)


Mit IPv6 ist die Unterstützung für mehrere IP-Adressen pro Interface Plficht!





Adressvergabe durch ISP

Die ersten 64 Bit der IPv6 Adressen dienen nur der Strukturieren. ISP vergeben an ihre Kunden meist ein /48. Für kleiner Netze wird inzwischen ein /56 rausgegeben, da man gemerkt hat, dass /48 doch etwas zu viel war.




ipv6 Adressaufbau

2001:6f8:900:90f6:106a:d58d:387c:a74f

- 128 Bit in 8 Blöcken(16 Bit Word) mit : getrennt
- ersten 64 Bit = Prefix
- letzten 64 Bit = Interface Identifier
- es gibt ein paar Sonderfälle für diese Regel



---
##############unordered

128 bit

255:255:255:255:255:255:255:255:255:255:255:255:255:255:255:255

FF:FF:FF:FF:FF:FF:FF:FF:FF:FF:FF:FF:FF:FF:FF:FF

11111111:11111111:11111111:11111111:11111111:11111111:11111111:11111111:11111111:11111111:11111111:11111111:11111111:11111111:11111111:11111111

FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF

:::::::




2a01:4f8:d13:1f41::2

2a01:04f8:0d13:1f41:0000:0000:0000:0002

::ff:78:74:42:22

::ff:78.47.42.22

0a.00.00.01

10.0.0.1


#Verschieden ipv6 Adressarten

##nicht festgelegt
Block ::/128 

##Loopback
Block ::1/128

##Eingebettete IPv4-Adresse
::00:xx:xx:xx:xx/96 z. B. ::00:8.8.8.8 

##IPv4-Adresse auf IPv6 abgebildet
::ff:xx:xx:xx:xx/96 z.B. ::ff.8.8.8.8

##link local
fe80:: - feb::/10

##site local
fec0:: - fef::/10

##multicast
ff::/8

##globaler Unicast
001 (Dual)/3  - Adressen fangen mit 2 oder 3 an und stellen die normalen öffentlichen Adressen dar, die zugewiesen werden.

##ULA Unique local Address

Block: fc00::/7 davon wird zurzeit nur fd00::/8 genutzt

Gegenstück zur den privaten ipv4 Adressen wie z.B. 192.168.1.1. Diese Adressen werden nicht im Internet geroutet und werden z. B. von der Fritzbox den Clients zugewiesen, falls kein ipv6 Provider verfügbar ist. Reverse DNS unter ip6.arpa ist nicht verfügbar und die Adressen sind naturgemäß nicht global eindeutig.

Quellen:
http://en.wikipedia.org/wiki/Unique_local_address
RFC 4193

#Adressen unter Ubuntu

Per default findet man immer eine Loopback Adresse ::1 und eine link local Adresse fe80::/10. Zusätzlich kann eine eine Unicast Adresse vergeben werden. Entweder eine Site Local/ULA oder eine Aggregatable global unicast Adresse. Letztere wird auch im Internet geroutet.

#Router Advertisments und Neighbor Discovery

##Neighbor Discovery

Quelle: RFC 2461





---

fec0:dead:beaf::172.20.1.10


