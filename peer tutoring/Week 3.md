3.1 Redirecting en pipelining  
1. Wat doet volgend commando: echo "Hello, world!" | figlet  
```bash
echo "Hello, world!" | figlet 
```

2. Schrijf een regel code die telt hoeveel bestanden er in de huidige folder staan.  
```bash
ls | wc
```
3. Zorg ervoor dat het resultaat in het bestand /tmp/aantal komt te staan.
```bash
ls | wc >> /tmp/aantal
```
4. Toon de inhoud van “/home” directory en schrijf deze weg in de file  
“homedirectories”. Gebruik hiervoor een relatief pad vanaf je home-folder.  
```bash
ls ../ >> homedirectories
```

5. Geef een gesorteerde lijst van de regels die in het bestand /etc/passwd staan,  
en schrijf het weg in een bestand passwd_sort.txt op je desktop

```bash

```
5. Schrijf de manual pagina van de passwd file weg naar “/tmp/passwdmanual”  
6. Wat is het verschil tussen:  
grep root /etc/* 2>&1 >file  
grep root /etc/* >file 2>&1  
3.2 Variabelen  
1. Maak een variabele "docs" met het volledige path van je Documents folder.  
◦ gebruik docs om een bestand aan te maken in je Documents folder  
◦ gebruik docs om een lijst van alle bestanden in die folder te zien.  
2. Wat zit er in de variabelen LOGNAME, USER, SHELL, HOME, PATH?  
3. Wat doet het commando which? Wat heeft dit te maken met de variabele PATH?  
p. 2