
# Oefeningen Linux week 3

## 3.1 Redirecting en Pipelining

1. **Wat doet volgend commando: `echo "Hello, world!" | figlet`?**

   Dit commando stuurt de string "Hello, world!" door naar het `figlet` programma, wat tekst omzet in ASCII kunst. Het resultaat is dat "Hello, world!" groot wordt weergegeven in ASCII-tekens.

2. **Schrijf een regel code die telt hoeveel bestanden er in de huidige folder staan. Zorg ervoor dat het resultaat in het bestand `/tmp/aantal` komt te staan.**

   ```bash
   ls -1 | wc -l > /tmp/aantal
   ```

3. **Toon de inhoud van de `/home` directory en schrijf deze weg in de file `homedirectories`. Gebruik hiervoor een relatief pad vanaf je home-folder.**

   ```bash
   ls /home > ~/homedirectories
   ```

4. **Geef een gesorteerde lijst van de regels die in het bestand `/etc/passwd` staan, en schrijf het weg in een bestand `passwd_sort.txt` op je desktop.**

   ```bash
   sort /etc/passwd > ~/Desktop/passwd_sort.txt
   ```

5. **Schrijf de manual pagina van de `passwd` file weg naar `/tmp/passwdmanual`.**

   ```bash
   man passwd > /tmp/passwdmanual
   ```

6. **Wat is het verschil tussen:**
   - `grep root /etc/* 2>&1 >file`
   - `grep root /etc/* >file 2>&1`

   **Antwoord:**
   - In het eerste commando worden foutmeldingen (stderr) omgeleid naar stdout en vervolgens wordt alleen stdout naar het bestand `file` geschreven.
   - In het tweede commando wordt zowel stdout als stderr omgeleid naar het bestand `file`. 

## 3.2 Variabelen

1. **Maak een variabele "docs" met het volledige path van je Documents folder.**

   ```bash
   docs=~/Documents
   ```

   - **Gebruik `docs` om een bestand aan te maken in je Documents folder:**

     ```bash
     touch $docs/bestand.txt
     ```

   - **Gebruik `docs` om een lijst van alle bestanden in die folder te zien:**

     ```bash
     ls $docs
     ```

2. **Wat zit er in de variabelen `LOGNAME`, `USER`, `SHELL`, `HOME`, `PATH`?**

   - `LOGNAME`: De naam van de ingelogde gebruiker.
   - `USER`: De naam van de gebruiker die de sessie uitvoert.
   - `SHELL`: Het pad naar de shell die wordt gebruikt, bijvoorbeeld `/bin/bash`.
   - `HOME`: Het home-directory pad van de gebruiker, bijvoorbeeld `/home/gebruikersnaam`.
   - `PATH`: Een lijst van directories waar het systeem zoekt naar uitvoerbare bestanden.

3. **Wat doet het commando `which`? Wat heeft dit te maken met de variabele `PATH`?**

   Het commando `which` toont het volledige pad naar de uitvoerbare bestanden die worden gebruikt wanneer je een commando invoert. Het zoekt naar deze bestanden in de directories die zijn opgegeven in de `PATH`-variabele.
