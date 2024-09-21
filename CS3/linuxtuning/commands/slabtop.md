---
description: shows realtime kernal slab cache
week: "1"
usage: |-
  ````bash 
  slabtop
tags:
  - command
  - CS3
  - linuxTuning
---
## extra info:
Slab allocatie is een strategie voor toewijzing van kernel geheugen.  
Een slab bestaat uit 1 of meer aaneengesloten pagina’s. Een cache is een logische eenheid  
van opslag en gebruikt 1 of meerdere slabs.  
Er is één cache voor elke unieke datastructuur van de kernel.  
Elke cache wordt bevolkt met objects die instanties zijn van de kernel datastructuur die de  
cache representeert. Omdat een slab een aantal objecten kan bevatten, vermijdt men interne  
fragmentatie.
![[Pasted image 20240921154838.png]]

![[Pasted image 20240921154830.png]]
