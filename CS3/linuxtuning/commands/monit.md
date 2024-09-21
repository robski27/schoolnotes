---
description: monitor processes, services, CPU, RAM, ports, and protocols
week: "1"
usage: see extra info
tags:
  - command
  - CS3
  - linuxTuning
---
## extra info:

sudo vi /etc/default/monit  
Hier aanpassen startup=1 in plaats van startup=0  
Start service: sudo /etc/init.d/monit start  
Voorbeeldconfiguratie:  
cat /etc/monit/monitrc  
# Opstarten in achtergrond en om de 2 minuten nakijken  
set daemon 120  
# Log naar de systeemlog  
set logfile syslog facility log_daemon  
# Log doorsturen naar mailserver  
set mailserver localhost # primary mailserver  
# Mailformaat:  
From: monit@$HOST # sender  
Subject: monit alert — $EVENT $SERVICE # subject  
$EVENT Service $SERVICE  
Date: $DATE  
Action: $ACTION  
Host: $HOST # body  
Description: $DESCRIPTION  
# Monit heeft een ingebouwde webserver met configuratie-mogelijkheid  
set httpd port 2812  
use address localhost # alleen vanuit localhost toegangkelij
