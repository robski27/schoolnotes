---
description: show info about processes
week: 
usage: |-
  ````bash 
  vmstat
tags:
  - command
  - CS3
  - linuxTuning
---
## extra info:

CPU  
us Percentage user processen in gebruik  
sy Percentage systeemprocessen in gebruik  
id Percentage dat de CPU niets te doen heeft  
(Bij een lege run-queue start linux de thread "idle" (die dus niks doet)  
wa Percentage dat de CPU wacht op I/O  
Processen  
r Processen die wachten op de CPU (runnable)  
b Processen die geblokkeerd zijn  
(wachten op I/O of zijn uitgeswapt)  

RAM  
free RAM geheugen dat nog vrij is in KB  
buff Gegevens die in RAM staan, maar nog naar disk moeten geschreven worden  
cache Gegevens, in RAM, die uitgelezen zijn van de schijf om later te gebruiken  
si Aantal KB dat ingeswapt wordt  
so Aantal KB dat uitgeswapt wordt  
bi Aantal blocks ingelezen van de schijf  
bo Aantal blocks weggeschreven naar de schijf

ACTIE:  
Het tooltje vmstat is vooral handig om de belasting van de processor te bekijken en  
beoordelen.  
 De procs kolom r (runnable) geeft het aantal wachtenden voor de processor(en) weer.  
Als dit aantal groter is dan 4 keer het aantal processoren, dan moeten de processen te  
lang wachten om te kunnen draaien. De processen moeten dan al teveel timeslices  
wachten om nog voldoende interactief te blijven. Snellere processoren kunnen  
hiervoor een oplossing zijn (niet méér processoren).  
 De procs kolom b (blocked) geeft het aantal geblokkeerde processen aan. Deze  
wachten dus op I/O. Wanneer dit aantal hoog is, is het aan te raden om je  
schijfactiviteit te bekijken met iostat