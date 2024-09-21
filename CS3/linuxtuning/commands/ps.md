---
description: list all the active processes on the system
week: "1"
usage: |-
  ````bash 
  ps [options]
tags:
  - command
  - CS3
  - linuxTuning
---
## extra info:
D uninterruptible sleep (meestal I/O)  
R runnable (klaar om te draaien)  
S sleeping(slapend)  
T traced of gestopt  
Z een ontheemd ("zombie") proces  
< hoge-prioriteit (niet nice tegen anderen)  
N lage-prioriteit (nice tegen anderen)  
L paginas zitten vast in geheugen(real-time en I/O)  
s session leader  
l is multi-threaded  
\+ draait bij de voorgrondprocessen

| command  |  description                                               |
| -------- | ---------------------------------------------------------- |
| ps aux   | Alle processen met in eerste kolom de username             |
| ps auxww | Toont volledige commando's (gewone ps kapt af na 80 chars) |
| ps auxf  | Toont processen en boomstructuur processen                 |
