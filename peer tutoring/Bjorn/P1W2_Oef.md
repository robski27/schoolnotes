

2.1 Paden
Hieronder zie je een lijst van paden. Welke paden zijn relatief, absoluut of fout in Linux?
1. /home - absoluut
2. log/apt - relatief
3. ../.. - relatief
4. ~/Documents - absoluut
5. c:\users - absoluut
6. ./Desktop - relatief
7. ../../etc - relatief
8. /etc/cron.d - absoluut
9. `\usr\bin` - absoluut
10. Downloads - relatief

2.2 Eenvoudige commando's
1. Wat is de eenvoudigste wijze om een lijst van alle commando's te krijgen die beginnen met "pdf" ?
   `ls /usr/bin/pdf*` of `compgen -c | grep '^pdf'`
2. Wat doet het volgende commando: "date +%T"
   `toon de tijd`

	![[Pasted image 20240924195553.png]]
   
3. Wat doet het commando "id"?
	*toont de uid, guid en de groups waar de current user deel van is*

4. Kopieer het bestand /etc/passwd naar je bureaublad. Verplaats het bestand daarna naar de directory /tmp/tijdelijk. (Je zal die directory eerst moeten aanmaken.) Welke commando's gebruik je hiervoor

	```
	mkdir /tmp/tijdelijk/
	cp /etc/passwd /tmp/tijdelijk/
	cp /tmp/tijdelijk/passwd /home/parallels/Desktop
	```

5. Toon alle files in “/etc” waarvan de filenaam begint met een “n” en eindigt op “conf”. Gebruik een absoluut pad.

	`ls /etc/n*.conf`

	![[Pasted image 20240924200108.png]]
	

6. Geef een lijst van alle files die in de directory /var/log/ staan en op “.log” eindigen. Gebruik een absoluut pad.
   
	`ls /var/log/*.log`

7. Met welk commando kan je zien hoeveel regels er staan in het bestand /var/log/syslog?

	`cat /var/log/syslog | wc -l`

8. Zoek op hoe je een user kan locken met passwd (niet doen!)

	`sudo passwd -l username`

9. wat doet ls -ls?

	toont output in lijst vorm met disk blocks (1ste nummer)

10. wat doen de commando's "hostname" en "uname -a" 

	`hostname`:
	toont de [[NT DNS]] name of the system

	`uname -a`
	toont systeem informatie, zoals de kernel version, architecture etc
    
11. hoe kan je ls omgekeerd laten sorteren?
    
	`ls | sort -r`

12. toon de inhoud van /proc/cpuinfo
    
	`cat /proc/cpuinfo`

13. Maak 3 bestanden aan in je home-folder met namen "eenBestand", "nogeenBestand" en "laatsteBestand". Wis nu alle bestanden die eindigen in "eenBestand". Verfieer dat enkel het laatste bestand over blijft.

	```
	touch {touch {eenbestand,nogeenbestand,laatstebestand}
	rm *eenbestand
	```

14. Voer het commando "bash" uit. Wat gebeurt er dan? Wat gebeurt er als je "exit" ingeeft?

	je start een bash shell, `exit` stopt deze
