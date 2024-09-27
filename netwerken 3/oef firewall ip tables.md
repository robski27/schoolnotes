


**OPGAVE**

1. Zorg bij beide virtuele machines minstens voor een netwerkkaart van het type "Host Only" en vboxnet0 (daarbuiten mag je ook nog een "NAT" netwerkkaart hebben, zodat je bijvoorbeeld nog op internet kan. Stel bij de Advanced settings de netwerkkaart in met Promiscuous op Allow All, zodat je kan sniffen. 

2. Netwerkverbinding testen:  
a) Ping van de machine A naar machine B.  
Om te kijken of het sniffen werkt start je op machine B het programma tcpdump.  
Stel nu je ping in zodat je 100 pings per seconde doet. Met welk commando kan dit?
ip -i 0.01 192.168.66.3

b) Doe een nslookup van bv www.kdg.be  
Je moet een antwoord van een DNS server krijgen met het IP adres van www.kdg.be
185.135.13.159

 Iptables zit in de kernel gecompileerd. Standaard komt de iptables log dus terecht in de systeemlog /var/log/syslog en/of in de kernel log /var/log/kern.log

3. Maak het script iptables.sh aan. Hiermee kan je alle firewallregels met één script instellen.

**Start met dit script** **(dat moet je als root uitvoeren)****:**

**iptables -F INPUT  
iptables -F OUTPUT  
****iptables -Z****
iptables -P INPUT DROP  
iptables -P OUTPUT DROP  
iptables -A OUTPUT -p tcp --destination-port** **80** **-j ACCEPT  
iptables -A INPUT -p tcp --source-port** **80** **--dport 1025:65535 -j ACCEPT  
iptables -L -n** **-v**

4. Welke iptables commando's aan het begin van het script maken alle chains leeg?
**iptables -F INPUT  
iptables -F OUTPUT**


6. Welke iptables commando's aan het begin van je script stellen alle default policies in op DROP?
	***iptables -P INPUT DROP  
	iptables -P OUTPUT DROP

7. We gaan nu de UDP aanvraag voor de nslookup loggen met iptables. Zoek zelf op met welke iptables regel je dit kan doen. gebruik een eigen log-prefix 'UDP IPTABLES' . Kijk na of je nslookup gelogd wordt.
	***iptables -A OUTPUT  -j LOG --log-prefix "UDP IPTABLES"***

9. Test op de Host Only interface een ip v6 ping uit tussen de machines (bv ping -6 -I enp0s8 fe80......)  
Schrijf regels die wel een v6 ping blokkeren maar niet een v4 ping
/
  
8. Installeer het tooltje nemesis. Opgelet deze gebruikt ook het pakket libnet1 (zie referentie) Zoek op met welk commando je een DNS aanvraag kan doen. Doe opnieuw een DNS aanvraag. (man nemesis-dns geeft een manual page)
	
  
9. Maak een iptables regel waarmee je een limiet legt op het aantal icmp pakketten per seconde. Stel maximum 1 per seconde in met een burst van 5. Log dit met prefix 'ICMP IPTABLES'. Test nu uit of je ping van 100 per seconde gedetecteerd wordt.
```bash
iptables -A INPUT -p icmp -m limit --limit 1/second --limit-burst 5 -j ACCEPT
```


10. Schrijf een loopscriptje in shell dat met nemesis-icmp pings doet (deze pings wachten niet op een reply en zullen dus zo snel mogelijk uitgevoerd worden). 
	sudo nemesis icmp -D 192.168.66.3 -qE -c 0; echo $?

11. Maak met nemesis nu een DNS request met als SRC en DST adres je eigen adres. Komt er iets speciaal in de log?

  
12. Schrijf iptables regels voor udp die enkel udp verkeer toelaat naar binnen, dat je zelf hebt aangevraagd. Bij uitbreiding zorg je er ook voor dat deze regel niet geldt voor een lokale interface.

13. Voer het commando iptables -n -L INPUT uit. Wat krijg je te zien?

	de chains

15. Stel een zo strikt mogelijke ACL in op je Linux waarmee je zorgt dat je Linux kan pingen naar een andere computer, maar niemand naar je Linux.

16. Installeer en start de apache webserversudo apt-get install apache2/etc/init.d/apache2 start(Kijk even na in een browser of er op jou IP adres een website draait)Zorg er nu, via iptables voor dat mensen die op poort 8080 verbinden ook de website te zien krijgen van poort 80.
```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 8080 -j REDIRECT --to-port 80
```
18. Installeer een ftp server (bv vsftpd) Installeer en start wireshark.  
a) Test uit of je een passieve ftp sessie kan starten naar je server (als gui kan je filezilla installeren).Bekijk in wireshark de initiele handshake tussen de client en de FTP server.Noteer volgende informatie van alle pakketten en de richting:SRC IP/SRC PORT/DST IP/DST PORT  
b) Test uit of je actieve ftp kan starten naar de serverBekijk in wireshark de initiele handshake tussen de client en de FTP server. Noteer volgende informatie van alle pakketten en de richting:SRC IP/SRC PORT/DST IP/DST PORT. Schrijf zo strikt mogelijke regels 

17. Installeer en start de ssh server openssh-server

Start de apache webserver op poort 8080 in plaats van de standaard poort 80.  
Test uit of je vanaf de client kan surfen naar 8080  
Start de openssh server. Test uit of je met je client kan verbinden naar poort 22 (ssh)