

De log portscan2021.pcap (in binair tcpdump formaat) werd vastgesteld op een Network Intrusion Detection System (NIDS). Vijf verschillende scans werden vastgelegd naar de target. Met behulp van tcpdump/wireshark kan je elke scan manueel analyseren.

Met behulp van p0f, ettercap en snort kan je automatisch analyses laten uitvoeren.  
  
  
  
1. Hoe werd het binaire logbestand portscan2021.pcap aangemaakt?  
	 *door wireshark te laten loggen* 
  
2. Wat is het IP adres van de aanvaller?  
	192.168.56.1
  
3. Wat is het IP adres van de target?  
	  192.168.56.103
  
4. Welke poorten staan open op de target?  
  
	  22,80,53
1. De target werd op minstens 4 manieren gescand. Geef de 4 methodes en leg uit hoe ze werken.  
	*1. xmas*
	*2. reset*
	
  
  
6. Welke scantool werd gebruikt om de target te scannen? Hoe weet je dit?  
  
=======
1. Hoe werd het binaire logbestand portscan2021.pcap aangemaakt?  
  
	  tcpdump
1. Wat is het IP adres van de aanvaller?  
	  192.168.56.1
  
3. Wat is het IP adres van de target?  
  192.168.56.103
  
4. Welke poorten staan open op de target?  
	  22,53,80
  
5. De target werd op minstens 4 manieren gescand. Geef de 4 methodes en leg uit hoe ze werken.  
  reset
  decoy
  xmas 
  ack
  stealth
  
6. Welke scantool werd gebruikt om de target te scannen? Hoe weet je dit?  
  nmap 

  
7. Welke commando's heeft de aanvaller gebruikt? (geef deze zo volledig mogelijk met alle parameters)  
  
  
8. Welk besturingssysteem draait de aanvaller?  
  
  

9. Stel snort in (zorg dat portscans gedetecteerd worden) en laat snort de portscan analyseren.  
