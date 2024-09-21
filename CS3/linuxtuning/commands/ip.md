---
description: shows and change network connections and stats
week: "1"
usage: |-
  ````bash 
  ip [options]
tags:
  - command
  - CS3
---
## extra info:

a) Interface  
Dummy interface maken:  
ip link add type dummy  
Dummy interface opzetten/afzetten:  
ip link set dummy0 up / down  
b) IP adres  
IP adres instellen (mag meer dan 1 per interface) of replace gebruiken ipv add.  
ip address add dev dummy0 10.10.10.1/24  
ip address add dev dummy0 192.168.168.168/24  
c) Route  
Route toevoegen en verwijderen:  
ip route add 10.10.10.0/24 dev dummy0  
ip route del 10.10.10.0/24  
Default route instellen:  
ip route add 0.0.0.0/0 dev dummy0  
ip route replace default dev dummy0 (REPLACE IS OOk add