## 3.1 Redirecting en pipelining  
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
cat /etc/passwd | sort >> sort_passwd.txt
```
6. Schrijf de manual pagina van de passwd file weg naar “/tmp/passwdmanual”  
```bash
man passwd >> /tmp/passwdmanual
```
8. Wat is het verschil tussen:  
grep root /etc/* 2>&1 >file  
grep root /etc/* >file 2>&1  

### Summary

- In `grep root /etc/* 2>&1 >file`, only `stdout` goes to `file`, and `stderr` goes to the console.
- In `grep root /etc/* >file 2>&1`, both `stdout` and `stderr` are redirected to `file`.

The second command (`>file 2>&1`) is more commonly used when you want both the output and errors to be captured in the same file.


## 3.2 Variabelen  
1. Maak een variabele "docs" met het volledige path van je Documents folder.  
	```bash
	docs="/home/$USER/Documents"
```
◦ gebruik docs om een bestand aan te maken in je Documents folder  
```bash
	touch $docs
```
◦ gebruik docs om een lijst van alle bestanden in die folder te zien.  
```bash
	ls $docs
```
2. Wat zit er in de variabelen LOGNAME, USER, SHELL, HOME, PATH?  
	1.logname = naam waar je met ingelogd bent
1. Wat doet het commando which? Wat heeft dit te maken met de variabele PATH?  
p. 2