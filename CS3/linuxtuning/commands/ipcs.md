---
description: looks at what processes are sharing memory or are using ipc
week: "1"
usage: |-
  ````bash 
  ipcs
tags:
  - command
  - CS3
  - linuxTuning
---
## extra info:
pcs -p geeft de procesID van het laatste proces dat het gedeelde geheugen gebruikte  
Programma's zoals Oracle, MySQL en GIMP gebruiken veelvuldig gedeeld geheugen.  
Verder zullen de kernel parameters kernel.shmmax, kernel.shmall en kernel.shmmni invloed  
hebben op de allocatie van gedeeld geheugen.  
kernel.shmmni minimum allocatieeenheid (bv per 4KB)  
kernel.shmall maximale aantal gedeelde geheugenblokken per proces  
kernel.shmmax maximale totale hoeveelheid gedeeld geheugen  
Mogelijk moet je bij het gebruik van databanken een van deze waarden aanpassen. Zie  
hiervoor verder in hoofdstuk 5 bij het sysctl commando.