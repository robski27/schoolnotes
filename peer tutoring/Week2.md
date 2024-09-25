#peertutoring
Oefeningen Linux week 2  
2.1 Paden  
Hieronder zie je een lijst van paden. Welke paden zijn relatief, absoluut of fout in  
Linux?  
1. /home  
2. log/apt  
3. ../..  
4. ~/Documents  
5. c:\users  
6. ./Desktop  
7. ../../etc  
8. /etc/cron.d  
9. \usr\bin  
## 10.Downloads  
## 2.2 Eenvoudige commando's  
### 1. Wat is de eenvoudigste wijze om een lijst van alle commando's te krijgen die beginnen met "pdf" ?  
```bash
compgen -c pdf
```
### 2. Wat doet het volgende commando: "date +%T"  
```bash
date +%T # geeft HH:MM:SS
```
### 3. Wat doet het commando "id"?  

dit geeft de UID van de user en groups etc.

### 4. Kopieer het bestand /etc/passwd naar je bureaublad. Verplaats het bestand daarna naar de directory /tmp/tijdelijk. (Je zal die directory eerst moeten  aanmaken.) Welke commando's gebruik je hiervoor?  
```bash
cp /etc/passwd ~/Desktop
mkdir /tmp/tijdelijk
mv ~/Desktop/passwd /tmp/tijdelijk
```
### 5. Toon alle files in “/etc” waarvan de filenaam begint met een “n” en eindigt op  
“conf”. Gebruik een absoluut pad. 
```bash
ls /etc/n*conf

```
### 6. Geef een lijst van alle files die in de directory /var/log/ staan en op “.log”  eindigen. Gebruik een absoluut pad.  
``` bash
ls /var/log/*.log
```

### 7. Met welk commando kan je zien hoeveel regels er staan in het bestand  /var/log/syslog?

```bash
wc -l /var/log/syslog
```
### 8. Zoek op hoe je een user kan locken met passwd (niet doen!)  
```bash
sudo passwd -l $USER
```
### 9. wat doet ls -ls?  
Het commando ls -ls toont de bestanden in de huidige directory met hun bestandsgrootte in blokken en geeft extra informatie, zoals de bestandspermissies en eigenaarschap.

### 10.wat doen de commando's "hostname" en "uname -a"  

geven info over je systeem zoals bv welke distro etc.

### 11.hoe kan je ls omgekeerd laten sorteren?  
```bash
ls -r
```
### 12.toon de inhoud van /proc/cpuinfo 
```bash
cat /proc/cpuinfo
```
### 13.Maak 3 bestanden aan in je home-folder met namen "eenBestand",  "nogeenBestand" en "laatsteBestand". Wis nu alle bestanden die eindigen in  "eenBestand". Verfieer dat enkel het laatste bestand over blijft.  

```bash
touch ~/eenBestand ~/nogeenBestand ~/laatseBestand

rm ~/*eenBestand
```
14.Voer het commando "bash" uit. Wat gebeurt er dan? Wat gebeurt er als je  
"exit" ingeeft?  
het opent een nieuwe shell sessies