# Lab 1: Linux Tuning Algemeen #Lab
## 0) Software
NODIG: Voor deze oefeningen zal je wat extra softwarepakketten nodig hebben.
Voor de laatste oefening moet je compileren.
Installeer hiervoor gcc, g++ en build-essential

`sudo apt install gcc g++ build-essential`

## 1)  Kopieer met "time" een groot bestand vanaf je cdrom of usb naar je schijf. Doe dit 2 keer. Vergelijk de 2 metingen.

`time cp /cdrom/BigBuckBunny /tmp/ #BigBuckBunny is a big video file` 

Eerste keer duurt langer, de 2de keer zit het bestand nog in de cache dus gaat het sneller

## 2) Schakel process accounting aan. Kijk welk proces het meest gebruikt wordt.

```bash
sudo apt install acct
mkdir -p /var/account
# of
mkdir -p /var/log/account

sudo accton /var/account/pacct
# of
sudo accton /var/log/account/pacct
sa 

#om terug af te zetten
sudo accton off
```
accton geeft procesgegevens weer, de procesgegevens worden opgeslagen in het bestand /var/account/pacct
sa geeft een overzicht van de verzamelde procesgegevens (sa = system accounting/summary accounting = statistieken)
## 3) Draai het tooltje vmstat (om de 2 seconden, 10 metingen).
Hoeveel processen staan er maximaal te wachten om te draaien?
Hoeveel processen staan er maximaal te wachten op I/O?
`vmstat 2 10`
``` output
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0      0 3521576  61580 1543536    0    0   450   168  291  207  1  3 95  1  0
 1  0      0 3521224  61580 1543576    0    0     0     0 1184  593  1  3 96  0  0
 1  0      0 3521468  61580 1543576    0    0     0     0  578  259  0  1 99  0  0
 3  0      0 3521624  61588 1543568    0    0     0     6  696  290  1  1 98  0  0
```

- r: het aantal processen die wachten om op de CPU te draaien.
- b: het aantal processen die wachten op I/O.
vmstat = virtual memory statistics
systeemmonitoringtool da info geeft over CPU-gebruik, geheugengebruik, I/O en processtatistieken
- 2 = aantal seconden tussen de metingen
- 10 = aantal metingen dat worden uitgevoerd
## 4) Bekijk de cache die de kernel gebruikt. Welke cache gebruikt het meeste geheugen?

```bash
sudo slabtop
```

slabtop is een tool die informatie over “slabs” toont.
slabs = geheugenblok dat door kernel wordt gebruikt om objecten van specifieke typen efficiënt te beheren (om kernel-objects en data-structuren op te slaan).
Kijk naar waar ‘cache size’ het hoogst is.

## 5) Draaien er applicaties die ipc gebruiken op je systeem?

```bash
ipcs
```

ipcs = tool voor info over inter-process communication resources die gebruikt worden
IPC = processen op een systeem die met elkaar kunnen communiceren of gegevens met elkaar kunnen uitwisselen.

## 6) Start een vi editor op.
#### a) Gebruik ldd om te kijken welke libraries vi gebruik.
#### b) Gebruik pmap om te kijken hoeveel geheugen vi gebruikt.
#### c) Wat is het verschil tussen de bibliotheken die pmap toont en de bibliotheken die ldd toont?
```bash
vi & #start op in background

#a
ldd /usr/bin/vi
	linux-vdso.so.1 (0x00007ffd1b1de000)
	libm.so.6 => /lib/x86_64-linux-gnu/libm.so.6 (0x00007eb9aed19000)
	libtinfo.so.6 => /lib/x86_64-linux-gnu/libtinfo.so.6 (0x00007eb9af242000)
	libselinux.so.1 => /lib/x86_64-linux-gnu/libselinux.so.1 (0x00007eb9af216000)
	libsodium.so.23 => /lib/x86_64-linux-gnu/libsodium.so.23 (0x00007eb9af1be000)
	libacl.so.1 => /lib/x86_64-linux-gnu/libacl.so.1 (0x00007eb9af1b4000)
	libgpm.so.2 => /lib/x86_64-linux-gnu/libgpm.so.2 (0x00007eb9af1ac000)
	libpython3.10.so.1.0 => /lib/x86_64-linux-gnu/libpython3.10.so.1.0 (0x00007eb9ae600000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007eb9ae200000)
	/lib64/ld-linux-x86-64.so.2 (0x00007eb9af284000)
	libpcre2-8.so.0 => /lib/x86_64-linux-gnu/libpcre2-8.so.0 (0x00007eb9aec80000)
	libexpat.so.1 => /lib/x86_64-linux-gnu/libexpat.so.1 (0x00007eb9aec4f000)
	libz.so.1 => /lib/x86_64-linux-gnu/libz.so.1 (0x00007eb9aec33000)
#b
pmap $(pgrep vi)
# Output te lang
```
##### c:  total          2445416K

ldd = commando voor “list dynamic dependencies” → toont welke shared libraries door een programma worden gebruikt
pmap toont geheugengebruik van een proces (memory maps)
pgrep = grep proces-id

verschil:
ldd toont statische informatie over afhankelijkheden voor uitvoering (alleen shared libraries die nodig zijn om het programma uit te voeren).
pmap toont werkelijk geheugengebruik van het proces (meer informatie dan alleen shared libraries, ook code, gegevens segmenten en geheugen-mapped files).

## 7) Test met hdparm de prestaties uit van je schijf
```bash
sudo hdparm -t /dev/sda
	/dev/sda:
		Timing buffered disk reads: 478 MB in  3.01 seconds = 158.67 MB/sec
```

hdparm = tool om prestaties van opslagapparaten (SSDs,...) te testen en optimaliseren -> gebruiken om schijf parameters te configureren om lees-schrijf snelheden te meten.

## 8) Gebruik lsof om te kijken welke processen (al-dan-niet gewenst) het netwerk gebruiken.
```bash
sudo lsof -i
```
```output
COMMAND   PID            USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
systemd-r 512 systemd-resolve   13u  IPv4   6644      0t0  UDP localhost:domain 
systemd-r 512 systemd-resolve   14u  IPv4   6645      0t0  TCP localhost:domain (LISTEN)
avahi-dae 592           avahi   12u  IPv4   1804      0t0  UDP *:mdns 
avahi-dae 592           avahi   13u  IPv6   1805      0t0  UDP *:mdns 
avahi-dae 592           avahi   14u  IPv4   1806      0t0  UDP *:59938 
avahi-dae 592           avahi   15u  IPv6   1807      0t0  UDP *:34642 
NetworkMa 596            root   25u  IPv4   7720      0t0  UDP nemo-VirtualBox:bootpc->_gateway:bootps 
cupsd     691            root    6u  IPv6   7010      0t0  TCP ip6-localhost:ipp (LISTEN)
cupsd     691            root    7u  IPv4   7011      0t0  TCP localhost:ipp (LISTEN)
```

## 9) Gebruik het tooltje ip om een extra netwerkkaart aan te maken met de naam dummy1. 
Deze krijgt het adres 1.2.3.4/24.
```bash
sudo ip link add dummy1 type dummy
sudo ip addr add 1.2.3.4/24 dev dummy1
sudo ip link set dummy1 up
```
Test uit of je deze kan pingen. Verwijder dan deze interface`sudo ip link delete dummy1`.

```bash
ping 1.2.3.4
	PING 1.2.3.4 (1.2.3.4) 56(84) bytes of data.
	64 bytes from 1.2.3.4: icmp_seq=1 ttl=64 time=0.027 ms
	64 bytes from 1.2.3.4: icmp_seq=2 ttl=64 time=0.027 ms
```
type dummy = een dummy interface die geen echte fysieke verbinding heeft, maar wel netwerkfunctionaliteiten biedt
dev dummy1 = de dummy1 interface, dev = device
link set wijzigt de status van de interface (zoals no shutdown)

## 10) Schrijf een korte oneindige loop in shell script.
Zorg met cpulimit dat de oneindige loop zo weinig mogelijk cputijd gebruikt. Lukt dit?
```bash
echo "while true; do echo 'Running…'; sleep 1; done" > infinite.sh
sudo chmod +x infinite.sh
sudo apt-get install cpulimit
cpulimit -l 10 ./infinite.sh
 
pkill infinite.sh
```
cpulimit = tool om cpu-gebruik van een proces te beperken.
cpulimit -l 10 = limiet van 10%


## 11. Maak een extra swapfile met de naam pagefile.sys. Zorg dat het systeem deze swapfile ook gebruikt. Waarom gebruikt linux een swapartitie en meestal geen swapfile?


## 12. Verander een kernelparameter zodat deze zo veel mogelijk swapt. Start enkele programma's en zorg ervoor dat je swapfile gebruikt wordt (zoek zelf uit hoe je dat kan zien).

## 13. Compileer de voorziene c en cpp source bestanden en start de gecompileerde programma's op.
Zoek zelf uit wat er fout loopt wanneer je het programma opstart.
Voor bestand b.c moet je lokaal een apache server draaien.

# Lab 2: Linux Tuning Sytemd #Lab

## 1) chkservice
Installeer chkservice
Het commando dat je daarvoor gebruikt is:
```bash
sudo apt install chkservice
```
chkservice is een programma voor het beheren en controleren van de status van systemd-diensten en of deze automatisch starten bij het opstarten van het systeem.
#### Met welk commando zie je de bestanden die chkservice installeert:
```bash
dpkg -L chkservice
```

Commando toont lijst van alle bestanden die door chkservice-pakket op je systeem geïnstalleerd zijn. 
dpkg = Debian Package Management
-L vraagt om een lijst te geven.
``` output
/.
/usr
/usr/bin
/usr/bin/chkservice
/usr/share
/usr/share/doc
/usr/share/doc/chkservice
/usr/share/doc/chkservice/changelog.Debian.gz
/usr/share/doc/chkservice/copyright
/usr/share/man
/usr/share/man/man8
/usr/share/man/man8/chkservice.8.gz
```
Zorg dat de nginx server opgestart is, maar niet automatisch opstart bij het opstarten van de machine.
Welke tekens worden er gebruikt om aan te geven dat dit juist is ingesteld?
```bash
#automatisch opstarten disablen
sudo systemctl disable nginx
```
#### check met chkservice: 
```bash 
chkservice
```

Commando toont een lijst van systemd-diensten en laat zien of ze automatisch opstarten bij het opstarten van het systeem.
- vinkje betekent dat de service automatisch opstart
- geen vinkje betekent dat service niet automatisch opstart

## 2) Gebruik systemd om de services weer te geven die de langste opstarttijd hebben.
Welk commando gebruik je hiervoor?
Welke 4 services nemen het meeste tijd in beslag?
```bash
#commando met lijst met tijd dat systemd services nodig hadden om op te starten
systemd-analyze blame

#4 services met meeste tijd
systemd-analyze blame | head -n 4
```
 systemd-analyze blame geeft lijst van alle systemd-services die tijdens opstart van systeem zijn uitgevoerd + de tijd die ze nodig hadden om op te starten.
```output
42.180s plymouth-quit-wait.service
35.765s vboxadd.service
 2.126s snapd.seeded.service
 1.202s networkd-dispatcher.service
```
## 3) Stop en start de nginx service met systemctl. Dit doe je met commando:
```bash
sudo systemctl stop nginx
sudo systemctl start nginx
```
## 4) Bekijk de log van nginx Dit doe je met commando:
```bash
sudo journalctl -u nginx
```
journalctl is een logviewer voor systemd-services
-u nginx wilt zeggen da ge alleen de logs van nginx sevice wilt zien

## 5) Schrijf "helloworldd" een script (service) die om de tien seconden Hello World en de datum en tijd toont.
Het script moet luisteren naar het eerste argument "start" om op te starten en het eerste argument "stop" om de opgestarte service te stoppen.
Test dit script eerst commandline uit!
Zet helloworldd in /usr/local/sbin
```bash
#!/bin/bash
LOG_FILE="/var/log/helloworld.log"
PID_FILE="/tmp/helloworldd.pid"

if [ "$1" == "start" ]; then
    # Controleer of het proces al draait
    if [ -f "$PID_FILE" ]; then
        echo "Service is al gestart (PID: $(cat $PID_FILE))"
        exit 1
    fi
    # Start de service
    echo "Service starten..."
    while true; do
        echo "Hello World $(date)" >> "$LOG_FILE"
        sleep 10
    done &
    # Sla de PID van het achtergrondproces op
    echo $! > "$PID_FILE"
    echo "Service gestart met PID $(cat $PID_FILE)"
elif [ "$1" == "stop" ]; then
    # Controleer of het proces actief is
    if [ ! -f "$PID_FILE" ]; then
        echo "Service draait niet."
        exit 1
    fi
    # Stop de service
    echo "Service stoppen..."
    kill "$(cat $PID_FILE)" && rm -f "$PID_FILE"
    echo "Service gestopt."
else
    echo "Gebruik: $0 {start|stop}"
    exit 1
fi
```
```bash 
sudo chmod +x /usr/local/sbin/helloworldd
sudo /usr/local/sbin/helloworldd start

sudo /usr/local/sbin/helloworldd stop

#check log om te zien of het heeft gewerkt
tail -f /var/log/helloworld.log
```
Schrijf in /etc/systemd/system/ een helloworldd.service die zorgt dat jouw script wordt opgestart. Je kan als inspiratie /lib/systemd/system/nginx.service gebruiken.
Zet wel het Type op simple (forking start een kind op, maar dat doen wij niet)
Geef het commando systemctl daemon-reload om de veranderingen in te laten.
```bash
[Unit]
Description=Service dat het script helloworld uitvoert.

[Service]
Type=simple
ExecStart=/usr/local/sbin/helloworldd start
ExecStop=/usr/local/sbin/helloworldd stop
ExecStop=-/sbin/start-stop-daemon --quiet --stop --retry QUIT/5 --pidfile /run/nginx.pid

[Install]
WantedBy=multi-user.target
```
Door de service te starten met “systemctl start helloworldd” zal het script uitgevoerd worden met het start command → ExecStart.
Service stoppen met “systemctl stop helloworldd”, dan zal script uitgevoerd worden met het stop command.

Door in `tail -f /var/log/helloworld.log` te gaan kijken, kan je zien dat er helloworld is bijgekomen en het script dus heeft gwerkt.

## 6. Op Centos/Redhat systemen bestaat een systemd service en die heet firewalld. Op Debian systemen bestaat die nog niet. Straks wel, want jij gaat die maken!

Deze bash functie veegt alle regels van de firewall en laat alles toe:
```bash
firewalld_stop() {
  iptables -F
  iptables -X
  iptables -P INPUT ACCEPT
  iptables -P FORWARD ACCEPT
  iptables -P OUTPUT ACCEPT
}
```
Deze bash functie veegt alle regels van de firewall, verbiedt alle icmp versie 4 pakketten en laat de rest toe
```bash
firewalld_start() {
  iptables -F
  iptables -X
  iptables -P INPUT ACCEPT
  iptables -P FORWARD ACCEPT
  iptables -P OUTPUT ACCEPT
  iptables -A INPUT -p icmp -j DROP
  iptables -A OUTPUT -p icmp -j DROP
}
```
Schrijf het script /usr/local/sbin/firewalld dat deze functies gebruikt.
Voorzie ook dat je script een restart en een reload kan doen (doe voor beide een stop en start)
Test uit met een ping versie 4.
```bash
#!/bin/bash
firewalld_stop() {
    iptables -F
    iptables -X
    iptables -P INPUT ACCEPT
    iptables -P FORWARD ACCEPT
    iptables -P OUTPUT ACCEPT
}

firewalld_start() {
    iptables -F
    iptables -X
    iptables -P INPUT ACCEPT
    iptables -P FORWARD ACCEPT
    iptables -P OUTPUT ACCEPT
    iptables -A INPUT -p icmp -j DROP
    iptables -A OUTPUT -p icmp -j DROP
}

firewalld_restart() {
    firewalld_stop
    firewalld_start
}

firewalld_reload() {
    firewalld_stop
    firewalld_start
}

case "$1" in
    start)
        firewalld_start
        ;;
    stop)
        firewalld_stop
        ;;
    restart)
        firewalld_restart
        ;;
    reload)
        firewalld_reload
        ;;
    *)
        echo "Usage: $0 {start|stop|restart|reload}"
        exit 1
        ;;esac

#while true; do
#    sleep 3600  # Houd de service actief met een lange slaap
#done
```
Kheb die while true moeten toevoegen omdat anders als je die als een service opstart, dan dacht het systeem meteen dat het script al klaar was met runnen.
#### Schrijf nu een firewalld service
```bash
[Unit]
Description=Service dat het script firewalld uitvoert.

[Service]
Type=simple
ExecStart=/usr/local/sbin/firewalld start
RemainAfterExit=true #dit kan ook ipv de loop!!
ExecStop=/usr/local/sbin/firewalld stop
#ExecReload=/usr/local/sbin/firewalld reload

[Install]
WantedBy=multi-user.target
```
Test nu uit of je de firewalld service kan opstarten/stoppen/status bekijken via systemctl.
```bash
sudo systemctl daemon-reload
sudo systemctl start firewalld.service 
sudo systemctl status firewalld.service 
sudo systemctl stop firewalld.service 
```


# Lab 3: LVM #Lab #todo 
# Lab 4: DNS #Lab
NODIG: Virtuele ubuntu met bind9,
             Configureer met eth0 Host Only adapter, eth1 NAT      

REF: https://help.ubuntu.com/community/BIND9ServerHowto    
#### Config bind9
File: `named.conf.local` 
```bash
//
// Do any local configuration here
//
// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "vandeneynde.local" {
        type master;
        file "/etc/bind/db.vandeneynde.local";
};
```
- File aangemaakt door bind9. Wordt gebruikt o, specifieke DNS zones te configureren, hierin moet meegegeven welke zones (domeinen) de DNS server zal beheren en ofdat het het master of slave zal zijn.
File: `db.vandeneynde.local`
```bash
$TTL    604800
@       IN      SOA     vandeneynde.local.  root.vandeneynde.local.  (
                                2       ; Serial
                           604800       ; Refresh
                            86400       ; Retry
                          2419200       ; Expire
                           604800 )     ; Negative Cache TTL

;

;  Name servers
@       IN      NS      vandeneynde.local.
; A records
@       IN      A       192.168.57.100
www     IN      A       142.250.179.142
mail    IN      A       192.168.57.100
```
- Dit is een zone bestand dat de DNS records zal opslaan. Dit is de file die we hierboven gespecifieerd hebben.

- A-records zijn voor ipv4 address koppeling
- **Na elke verandering steeds `sudo systemctl restart bind9` doen**
- Test uit of jouw DNS configuratiebestanden juist zijn. Gebruik named-checkconf en named-checkzone
```
sudo named-checkzone vandeneynde.local /etc/bind/db.vandeneynde.local 
```
#### Configureer jouw DNS server als DNS server (in /etc/resolv.conf).
```bash
# This is /run/systemd/resolve/stub-resolv.conf managed by man:systemd-resolved(8).
# Do not edit.
#
# This file might be symlinked as /etc/resolv.conf. If you're looking at
# /etc/resolv.conf and seeing this text, you have followed the symlink.
#
# This is a dynamic resolv.conf file for connecting local clients to the
# internal DNS stub resolver of systemd-resolved. This file lists all
# configured search domains.
#
# Run "resolvectl status" to see details about the uplink DNS servers
# currently in use.
#
# Third party programs should typically not access this file directly, but only
# through the symlink at /etc/resolv.conf. To manage man:resolv.conf(5) in a
# different way, replace this symlink by a static file or a different symlink.
#
# See man:systemd-resolved.service(8) for details about the supported modes of
# operation for /etc/resolv.conf.

nameserver 192.168.57.100
nameserver 127.0.0.53
options edns0 trust-ad
search .
```
##### Test met dig en nslookup
```bash
dig @192.168.57.100 vandeneynde.local
```
```output
; <<>> DiG 9.18.30-0ubuntu0.22.04.1-Ubuntu <<>> @192.168.57.100 vandeneynde.local
; (1 server found)
;; global options: +cmd
;; Got answer:
;; WARNING: .local is reserved for Multicast DNS
;; You are currently testing what happens when an mDNS query is leaked to DNS
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8017
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 45f790a0a6b41aec01000000677f9cc5d6e81c54a3ceb9c8 (good)
;; QUESTION SECTION:
;vandeneynde.local.		IN	A

;; ANSWER SECTION:
vandeneynde.local.	604800	IN	A	192.168.57.100

;; Query time: 1 msec
;; SERVER: 192.168.57.100#53(192.168.57.100) (UDP)
;; WHEN: Thu Jan 09 10:54:13 CET 2025
;; MSG SIZE  rcvd: 90
```
```bash
nslookup vandeneynde.local
```
```output
Server:		192.168.57.100
Address:	192.168.57.100#53

Name:	vandeneynde.local
Address: 192.168.57.100
```
```bash
host www.vandeneynde.local
```
```output
www.vandeneynde.local has address 142.250.179.142
```

#### reverse lookup fixen
File:`named.conf.local`
```bash
//
// Do any local configuration here
//

// Consider adding the 1918 zones here, if they are not used in your
// organization
//include "/etc/bind/zones.rfc1918";

zone "vandeneynde.local" {
        type master;
        file "/etc/bind/db.vandeneynde.local";
};

zone "57.168.192.in-addr.arpa" {
        type master;
        file "/etc/bind/db.192.168.57";
};
```
File: `/etc/bind/db.192.168.57`
```bash
$TTL    604800
@       IN      SOA     vandeneynde.local.  root.vandeneynde.local.  (
                                2       ; Serial
                           604800       ; Refresh
                            86400       ; Retry
                          2419200       ; Expire
                           604800 )     ; Negative Cache TTL
;

;  Name servers
@       IN      NS      vandeneynde.local.

; PTR records
100     IN      PTR     vandeneynde.local
100     IN      PTR     mail.vandeneynde.local
100     IN      PTR     www.vandeneynde.local
```
**`sudo systemctl restart bind9`**
```bash
dig -x 192.168.57.100
```
```output
; <<>> DiG 9.18.30-0ubuntu0.22.04.1-Ubuntu <<>> -x 192.168.57.100
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 2600
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 3, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: 67d2c689ac4e011001000000677f9fb797560e4144d92bed (good)
;; QUESTION SECTION:
;100.57.168.192.in-addr.arpa.	IN	PTR

;; ANSWER SECTION:
100.57.168.192.in-addr.arpa. 604800 IN	PTR	mail.vandeneynde.local.57.168.192.in-addr.arpa.
100.57.168.192.in-addr.arpa. 604800 IN	PTR	vandeneynde.local.57.168.192.in-addr.arpa.
100.57.168.192.in-addr.arpa. 604800 IN	PTR	www.vandeneynde.local.57.168.192.in-addr.arpa.

;; Query time: 0 msec
;; SERVER: 192.168.57.100#53(192.168.57.100) (UDP)
;; WHEN: Thu Jan 09 11:06:47 CET 2025
;; MSG SIZE  rcvd: 176
```

File: `/etc/nsswitch.conf`
```bash
# /etc/nsswitch.conf
#
# Example configuration of GNU Name Service Switch functionality.
# If you have the `glibc-doc-reference' and `info' packages installed, try:
# `info libc "Name Service Switch"' for information about this file.

passwd:         files systemd
group:          files systemd
shadow:         files
gshadow:        files

hosts:          files mdns4_minimal dns [NOTFOUND=return]
networks:       files

protocols:      db files
services:       db files
ethers:         db files
rpc:            db files

netgroup:       nis
```
- Bij hosts moet ***dns***  **voor** de not found regel staan



# Lab 5: Portscan #Lab
De log portscan2021.pcap (in binair tcpdump formaat) werd vastgesteld op een Network Intrusion Detection System (NIDS). Vijf verschillende scans werden vastgelegd naar de target. Met behulp van tcpdump/wireshark kan je elke scan manueel analyseren.

Met behulp van p0f, ettercap en snort kan je automatisch analyses laten uitvoeren.



#### 1. Hoe werd het binaire logbestand portscan2021.pcap aangemaakt?

`sudo tcpdump -i eth0 -w portscan2021.pcap`
#### 2. Wat is het IP adres van de aanvaller?
`192.168.56.1`
ontvangt heel veel reset packets lijk op een TCP connect scan aanval
#### 3. Wat is het IP adres van de target?
`192.168.1.103`
#### 4. Welke poorten staan open op de target?
- 22
- 53
- 80
#### 5. De target werd op minstens 4 manieren gescand. Geef de 4 methodes en leg uit hoe ze werken.
- TCP connect `tcp.flags.reset == 1` veel reset packets 
-  XMAS `tcp.flags.fin == 1 && tcp.flags.urg == 1 && tcp.flags.push == 1`
- TCP ACK scan `tcp.flags.ack == 1` -> follow tcp stream en kijk de flow. Als het ACK packet het begin is dan is het een ack scan
- TCP Window scan -> zelfde als ack scan maar kijkt naar de window size om te kijken of een poort open is
- Decoy scan `tcp.flags.syn == 1 && tcp.flags.ack == 0 && ip.dst == 192.168.56.103` kijken of er veel onlogische sources zijn

**NIET**
- TCP stealth scan
- Fin scan `tcp.flags.fin == 1 && tcp.flags.urg == 0 && tcp.flags.push == 0`
- Null scan `tcp.flags.fin == 0 && tcp.flags.urg == 0 && tcp.flags.push == 0 && tcp.flags.syn == 0 && tcp.flags.ack == 0`
#### 6. Welke scantool werd gebruikt om de target te scannen? Hoe weet je dit?
**nmap**
gebruik tool als `p0f`
`sudo p0f -r Downloads/portscan2021.pcap | grep scan`
#### 7. Welke commando's heeft de aanvaller gebruikt? (geef deze zo volledig mogelijk met alle parameters)
- stealth scan: `nmap -sS 192.168.56.103`
- ACK scan: `nmap -sA 192.168.56.103`
- FIN scan: `nmap -sF 192.168.56.103`
- XMAS scan:  `nmap -sX 192.168.56.103`

#### 8. Welk besturingssysteem draait de aanvaller?
` p0f -r Downloads/portscan2021.pcap`
#### 9. Stel snort in (zorg dat portscans gedetecteerd worden) en laat snort de portscan analyseren.
Welke aanval(len) zie je in de logs?
`sudo snort -A console -q -c /etc/snort/snort.conf -r Downloads/portscan2021.pcap `
- Xmas scan
- SNMP request tcp
- SNMP trap tcp
# Lab 6: Python #Lab
`sudo apt-get install thonny lynx libpython3-stdlib`
#### Basis webserver

Pas de Voorbeeldcode van de basisserver3.py aan en start de webserver op als gewone gebruiker (neem poort 1234) met python3.

Test de webserver uit. `python3 basisserver3-1.py`
#### 1. Zorg ervoor dat je webserver draait op 127.0.0.1

#### 2. Zorg ervoor dat de webserver een .html bestand kan openen. Bv met the method "open".
Voorzie hiervoor bv het bestand index.html. Test uit met lynx http://127.0.0.1:1234/index.html
```python
	#!/usr/bin/env python3
from http.server import BaseHTTPRequestHandler, HTTPServer
import time

hostName = "127.0.0.1"
hostPort = 1234

class MyServer(BaseHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        self.send_header("Content-type", "text/html")
        self.end_headers()
        with open("/home/nemo/Downloads/index.html", "r") as file:
            self.wfile.write(file.read().encode("utf-8"))

myServer = HTTPServer((hostName, hostPort), MyServer)
print(time.asctime(), "Server Starts - %s:%s" % (hostName, hostPort))

try:
    myServer.serve_forever()
except KeyboardInterrupt:
    pass

myServer.server_close()
print(time.asctime(), "Server Stops - %s:%s" % (hostName, hostPort))

```
#### 3. Een html bestand heeft als mimetype (en voor een browser als Content-type) text/html.

Gebruik nu "import mimetypes" om ook andere content dan HTML te voorzien.

Test uit met bv lynx http://127.0.0.1:1234/index.txt en lynx http://127.0.0.1:1234/image.jpg.
```python
	#!/usr/bin/env python3
from http.server import BaseHTTPRequestHandler, HTTPServer
import time
import mimetypes

hostName = "127.0.0.1"
hostPort = 1234

class MyServer(BaseHTTPRequestHandler):
    def do_GET(self):
        self.send_response(200)
        mimetype, _ = mimetypes.guess_type(self.path)
        self.send_header("Content-type", mimetype)
        self.end_headers()
        with open("/home/nemo/Downloads" + self.path, "rb") as file:
            self.wfile.write(file.read())

myServer = HTTPServer((hostName, hostPort), MyServer)
print(time.asctime(), "Server Starts - %s:%s" % (hostName, hostPort))

try:
    myServer.serve_forever()
except KeyboardInterrupt:
    pass

myServer.server_close()
print(time.asctime(), "Server Stops - %s:%s" % (hostName, hostPort))
```
#### 4. Kijk na of de client vanuit de localhost komt. Als dit niet het geval is geef je een 404 error boodschap.
```python
class MyServer(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.client_address[0] != "127.0.0.1":
            self.send_response(404)
            self.end_headers()
            self.wfile.write(b"404 not found")
            return 
        else:
            self.send_response(200)
            mimetype, _ = mimetypes.guess_type(self.path)
            self.send_header("Content-type", mimetype)
            self.end_headers()
            with open("/home/nemo/Downloads" + self.path, "rb") as file:
                self.wfile.write(file.read())
```
#### 5. Installeer als root de psutil module
`sudo pip3 install psutil`
Zorg nu dat je met behulp van deze module bij het oproepen van ps.cgi een proceslijst krijgt in je browser.
```python
    #!/usr/bin/env python3  
from http.server import BaseHTTPRequestHandler, HTTPServer  
import time  
import mimetypes  
import psutil  
  
hostName = "127.0.0.1"  
hostPort = 1234  
  
class MyServer(BaseHTTPRequestHandler):  
    def do_GET(self):  
        if self.client_address[0] != "127.0.0.1":  
            self.send_response(404)  
            self.end_headers()  
            self.wfile.write(b"404 not found")  
            return  
        else:  
            if self.path == "/ps.cgi":  
                self.send_response(200)  
                self.send_header("Content-type", "text/html")  
                self.end_headers()  
                self.wfile.write(b"<h1> List <h1>")  
                self.wfile.write(b"<ul>")  
                for process in psutil.process_iter():  
                    self.wfile.write(f"<li>{process}</li>".encode("utf-8"))  
                self.wfile.write(b"</ul>")  
                return  
            else:  
                self.send_response(200)  
                mimetype, _ = mimetypes.guess_type(self.path)  
                self.send_header("Content-type", mimetype)  
                self.end_headers()  
                with open("/home/nemo/Downloads" + self.path, "rb") as file:  
                    self.wfile.write(file.read())  
  
myServer = HTTPServer((hostName, hostPort), MyServer)  
print(time.asctime(), "Server Starts - %s:%s" % (hostName, hostPort))  
  
try:  
    myServer.serve_forever()  
except KeyboardInterrupt:  
    pass  
  
myServer.server_close()  
print(time.asctime(), "Server Stops - %s:%s" % (hostName, hostPort))
```
#### 6. EXTRA: voorzie een filelist met de bestanden die in de opstartdirectory staan
```python
#!/usr/bin/env python3
from http.server import BaseHTTPRequestHandler, HTTPServer
import time
import mimetypes
import psutil
import os

hostName = "127.0.0.1"
hostPort = 1234

class MyServer(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.client_address[0] != "127.0.0.1":
            self.send_response(404)
            self.end_headers()
            self.wfile.write(b"404 not found")
            return
        else:
            if self.path == "/ps.cgi":
                self.send_response(200)
                self.send_header("Content-type", "text/html")
                self.end_headers()
                self.wfile.write(b"<h1> List <h1>")
                self.wfile.write(b"<ul>")
                for process in psutil.process_iter():
                    self.wfile.write(f"<li>{process}</li>".encode("utf-8"))
                self.wfile.write(b"</ul>")
                return
            elif self.path == "/filelist":
                self.send_response(200)
                mimetype, _ = mimetypes.guess_type(self.path)
                self.send_header("Content-type", mimetype)
                self.end_headers()
                directory_path = '/home/nemo/Downloads'
                files = os.listdir(directory_path)
                self.wfile.write(b"<h1>File List</h1>")
                self.wfile.write(b"<ul>")
                for file in files:
                    file_path = os.path.join(directory_path, file)
                    if os.path.isfile(file_path):
                        self.wfile.write(f"<li>{file}</li>".encode("utf-8"))
                self.wfile.write(b"</ul>")
                return
            else:
                self.send_response(200)
                mimetype, _ = mimetypes.guess_type(self.path)
                self.send_header("Content-type", mimetype)
                self.end_headers()
                with open("/home/nemo/Downloads" + self.path, "rb") as file:
                    self.wfile.write(file.read())

myServer = HTTPServer((hostName, hostPort), MyServer)
print(time.asctime(), "Server Starts - %s:%s" % (hostName, hostPort))

try:
    myServer.serve_forever()
except KeyboardInterrupt:
    pass

myServer.server_close()
print(time.asctime(), "Server Stops - %s:%s" % (hostName, hostPort))
```

### Eenvoudige GUI:
**(Opgelet: noem je bestand niet easygui.py, want dan probeert hij bij een import easygui zichzelf te importeren)**
idk wil net werken #todo

# Lab 6b: Scapy #Lab #kijkgewoonbijkato #ikhebgeenzin
NODIG:
1. Ubuntu 18.04 LTS: http://ftp.belnet.be/ubuntu-releases/18.04/.  Voorzie een Host-Only adapter en een NAT adapter!
`sudo apt-get install nmap wireshark tcpdump python3-scapy python3-crypto ipython3 python3-graphviz imagemagick`
#### Sniffing
File: `01_sniff.py`
```python
#!/usr/bin/env python3
#######################################################################
__author__ = "jan.celis@kdg.be"
__description__ = "scapy sniffer, sniffs 10 packets"
__license__ = "GPLv3 https://www.gnu.org/licenses/gpl-3.0.nl.html"
__arguments__ = "none"
__version__ = "03-12-2019"
__requires__ = "sudo apt install tcpdump python3-scapy"
#######################################################################
from scapy.all import sniff
about= "##### "+ __description__ + " #####"
print(about)
packets=sniff(count=10)
```
File: `01b_sniff.py`
```python
#!/usr/bin/env python3
from scapy.all import sniff
from os import getuid
if getuid() != 0 :
print("Warning: Starting as non root user!")
try:
packets=sniff(count=10)
packets.summary()
```
# Lab7: TLS #Lab 
## certs #tls_cert
```bash
sudo apt install easy-rsa
mkdir ~/easy-rsa
ln -s /usr/share/easy-rsa/* ~/easy-rsa/
chmod 700 ~/easy-rsa
cd ~/easy-rsa
./easyrsa init-pki
```
File: `~/easy-rsa/vars`
```
set_var EASYRSA_REQ_COUNTRY    "BE"
set_var EASYRSA_REQ_PROVINCE   "Antwerp"
set_var EASYRSA_REQ_CITY       "Antwerp"
set_var EASYRSA_REQ_ORG        "KdG"
set_var EASYRSA_REQ_EMAIL      "nemo.vandeneynde@student.kdg.be"
set_var EASYRSA_REQ_OU         "MIT"
set_var EASYRSA_ALGO           "ec"
set_var EASYRSA_DIGEST         "sha512"
```
```bash
./easyrsa build-ca nopass

sudo cp  ~/easy-rsa/pki/ca.crt /tmp/ca.crt
sudo cp /tmp/ca.crt /usr/local/share/ca-certificates/
sudo update-ca-certificates
```
```bash
sudo apt install openssl


mkdir ~/server-csr
cd ~/server-csr/
openssl genrsa -out nemo-server.key
openssl req -new -key nemo-server.key -out nemo-server.req
```
```
Country Name (2 letter code) [AU]:BE
State or Province Name (full name) [Some-State]:Antwerp
Locality Name (eg, city) []:Antwerp
Organization Name (eg, company) [Internet Widgits Pty Ltd]:KdG
Organizational Unit Name (eg, section) []:MIT
Common Name (e.g. server FQDN or YOUR name) []:nemo-server
Email Address []:nemo.vandeneynde@student.kdg.be
```
**Let Op `Common Name (e.g. server FQDN or YOUR name) []:nemo-server`**
```bash
cp ~/server-csr/nemo-server.req /tmp/
cd ~/easy-rsa
./easyrsa import-req /tmp/nemo-server.req nemo-server
```
```bash
./easyrsa sign-req server nemo-server
```

## Apache2 server with TLS
#### 1. Installeer apache en activeer je SSL module
De module mod_ssl zit standaard in het pakket apache-common
```bash
sudo apt install apache2 -y
sudo a2enmod ssl
sudo systemctl restart apache2
```
#### 2. Maak een nieuwe site
```bash
sudo touch /etc/apache2/sites-available/sslsite.conf
```
#### 3. Activeer je nieuwe site (ook al is deze nu nog leeg)
```bash
sudo a2ensite sslsite
sudo systemctl reload apache2
```
#### 4. Pas het bestand /etc/apache2/sites-available/sslsite.conf aan met volgende gegevens:
```
NameVirtualHost *:443
<VirtualHost *:443>
        DocumentRoot /var/www/
        SSLEngine on
        SSLCertificateFile /home/nemo/easy-rsa/pki/issued/nemo-server.crt
        SSLCertificateKeyFile /home/nemo/server-csr/nemo-server.key
</VirtualHost>
```
#### 5. Kijk na in /etc/apache2/ports.conf of de server ook op poort 443 zal luisteren

#### 6. Maak je eigen certificaat en sleutel aan (met jouw naam en emailadres van KdG) en zet deze klaar op de plaats die je aangaf in /etc/apache2/sites-available/sslsite
kijk ->[[#certs tls_cert]] 
file: `/etc/hosts`
```
127.0.0.1       localhost
127.0.1.1       nemo-VirtualBox
127.0.0.1       nemo-server
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

#### 7. Start Apache2 opnieuw op
  `systemctl restart apache2`

#### 8. Je kan het certificaat uittesten via de browser op hetzelfde adres.
Browswer settings
- certificates
- View Certificates
- Import
- ca.crt file
#### 9. Zorg ervoor dat apache enkel minstens TLS versie 1.2 gebruikt
File: `/etc/apache2/sites-available/sslsite.conf`
```
NameVirtualHost *:443
<VirtualHost *:443>
        DocumentRoot /var/www/
        SSLEngine on
        SSLProtocol all -SSLv3 -TLSv1.1 +TLSv1.2
        SSLCertificateFile /home/nemo/easy-rsa/pki/issued/nemo-server.crt
        SSLCertificateKeyFile /home/nemo/server-csr/nemo-server.key
</VirtualHost>
```
Test:
```bash
openssl s_client -connect nemo-server:443 -tls1_1
#foutmelding 
openssl s_client -connect nemo-server:443 -tls1_2
wel output
```

#### 10. Installeer en configureer proftpd zodat FTP over TLS mogelijk is.
- Test eerst uit zonder TLS.
- Test uit met Filezilla met de optie "Require explicit FTP over TLS"
```bash
sudo apt install proftpd
sudo service proftpd start
```
```
sudo openssl req -x509 -nodes -newkey rsa:2048 -keyout /etc/ssl/private/proftpd.key -out /etc/ssl/certs/proftpd.crt
```
```
Country Name (2 letter code) [AU]:BE
State or Province Name (full name) [Some-State]:Antwerp
Locality Name (eg, city) []:Antwerp
Organization Name (eg, company) [Internet Widgits Pty Ltd]:KdG
Organizational Unit Name (eg, section) []:MIT
Common Name (e.g. server FQDN or YOUR name) []:nemo-server
Email Address []:nemo.vandeneynde@student.kdg.be
```
**Let Op `Common Name (e.g. server FQDN or YOUR name) []:nemo-server`**
```bash
sudo apt install proftpd-mod-crypto
```
File: `/etc/proftpd/proftpd.conf` voeg dit toe
```
LoadModule mod_tls.c

<IfModule mod_tls.c>
  TLSEngine on
  TLSLog /var/log/proftpd/tls.log
  TLSProtocol TLSv1.2 TLSv1.3
  TLSOptions NoCertRequest
  TLSRSACertificateFile /etc/ssl/certs/proftpd.crt
  TLSRSACertificateKeyFile /etc/ssl/private/proftpd.key
</IfModule>
```
```bash
sudo service proftpd restart
```
##### Installeer en open filezilla
- file
- site manager
- new site
- Host: nemo-server
- Encryption: Require explicit FTP over TLS
- User en Password instellen
# Lab 8: Deb packages #Lab 

***REF: Cursus Jan over Deb (Rich man's deb)***

#### Opzetten Email en username
maak volgend script en voer uit
```bash
#!/bin/bash
cat >>~/.bashrc <<EOF
DEBEMAIL="nemo.vandeneynde@student.kdg.be"
DEBFULLNAME="Nemo Van den Eynde"
export DEBEMAIL DEBFULLNAME
EOF
```

#### Maken van een executable
```bash
sudo apt-get install dh-make debhelper fakeroot devscripts zenity menu
```
maak en run volgend script packege kan aangepast worden naar wat jouw packege naam gaat zijn
```bash
#!/bin/bash
# Create my first debian package
package="snake"
version="1"
mkdir -p "${package}/${package}-${version}"
cd "${package}/${package}-${version}"
cat > ${package} <<EOF
#!/bin/sh
# (C) 2011 Uname Zenity, GPL3+
zenity --warning --text="\`exec uname -a\`"
EOF
chmod 755 $package
cd ..
tar -cvzf ${package}-${version}.tar.gz ${package}-${version}
```
#### dh_make
```bash
dh_make --native
```
heb optie p gebruikt voor een python script

#### control
Het debian/control bestand is één van de belangrijkste bestanden voor het genereren van
een pakket. Opgelet! Verwijder niet de lege lijn in dit bestand.
De lijn met Homepage mag je verwijderen
- Section: python

*Geldige Sections zijn:
admin, cli-mono, comm, database, debug, devel, doc, editors, education, electronics,
embedded, fonts, games, gnome, gnu-r, gnustep, graphics, hamradio, haskell, httpd,
interpreters, introspection, java, kde, kernel, libdevel, libs, lisp, localization, mail, math,
metapackages, misc, net, news, ocaml, oldlibs, otherosfs, perl, php, python, ruby, science,
shells, sound, tasks, tex, text, utils, vcs, video, web, x11, xfce, zope*

- Dependencies
	- Build-depends
		De build dependencies zijn programma's die je nodig hebt om het programma te
		compileren.
		Je mag bij Build-Depends ${shlibs:Depends} verwijderen.
		Typische invullingen voor Build-Depends zijn bv python, java, mono of perl afhankelijk
		van het programma dat nodig is om alles te compileren
	- Depends
		Software die nodig is om de applicatie te runnen.
- Description 
	De Description moet je invullen. De eerste lijn geeft KORT weer wat je programma doet.  
	Bij Ubuntu Software Center wordt deze lijn getoond, wanneer je de deb wilt installeren.
	De volgende lijnen zijn een Extended description (uitgebreide omschrijving). Deze  
	beginnen met een spatie. Je moet zelf een regeleinde voorzien (er is geen automatische  
	lijn wrap)  
	Als je een volgende paragraaf wil starten schrijf je gewoon spatie punt
- Architecture: all
#### Changelog
De changelog houdt alle wijzigingen bij. Sommige IDE's voegen zelf changelog items
toe. **Opgelet de distribution "unstable" is niet toegelaten op een Ubuntu systeem.**
**package**: Naam van je pakket
**version**: Versienummer (update van je pakket krijgt hogere nummer)
**distribution**: Eén van de opties uit dit bestand:
/usr/share/lintian/vendors/ubuntu/main/data/changes-file/known-dists
**urgency**: low, medium, high of critical
```
snake (1) UNRELEASED; urgency=medium

  * Initial Release.

 -- Nemo Van den Eynde <nemo.vandeneynde@student.kdg.be>  Tue, 14 Jan 2025 09:53:52 +0100
```
#### manpage
```bash
mv manpage.1.ex snake.6
```

File extention:

| Section | Description           | Notes                              |
| ------- | --------------------- | ---------------------------------- |
| 1       | User command          | executable commands or scripts     |
| 2       | System calls          | Functions provided by the kernel   |
| 3       | Library calls         | Functions withing system libraries |
| 4       | Special files         | Usually found in `/dev`            |
| 5       | File formats          | E.g. `/etc/passwd`'s file format   |
| 6       | games                 | games or other frivolous programs  |
| 7       | Macro packages        | ***Idk wtf dit is***               |
| 8       | System administration | Programs typicaly only run by root |
| 9       | Kernel routines       | Non-standard calls and internals   |
#### Rules
Idk blijf hier maar af.
Uit de cursus: ***In de rules file kan je bestaande stappen overrulen.***
#### {pakket_naam}.desktop
Een .desktop bestand is de manier waarop je applicatie in de menustructuur (of dash bij
unity op Ubuntu) verschijnt.
Ze gelden onder andere ook voor Gnome, KDE, Cinnamon en XFCE.
```
[Desktop Entry]
Name=Snake
Comment=Pakket met zenity
GenericName=Snake
X-GNOME-FullName=Snake
Exec=Snake
Icon=snake-icon
StartupNotify=false
Terminal=false
Type=Application
Categories=games;
```
- Je ziet dat de Icon geen extensie heeft. Deze icon met extensie xpm of png wordt
automatisch gezocht in /usr/share/pixmaps.
- Je ziet ook dat er geen pad voor de applicatie staat. Standaard moet elke applicatie
beschikbaar zijn in /usr/bin of /usr/sbin zonder dat iemand een pad moet ingeven.
- Je kan bv het bestand /usr/share/applications/pakket.desktop uittesten door hier op te
dubbelklikken in de filemanager nautilus. Opgelet. De naam van het bestand wordt
afgeleid uit het Name veld!
#### install file
Hier definieer je welke files waar moeten komen
bv.
```
snake usr/bin
snake-1.py usr/lib
snake-icon.png usr/share/pixmaps
snake.desktop /usr/share/applications
```

#### builden van den package
```bash
debuild -us -uc
```
De mappen structuur na een build!
```tree
.
├── debian
│   ├── changelog
│   ├── control
│   ├── copyright
│   ├── debhelper-build-stamp
│   ├── files
│   ├── install
│   ├── manpage.md.ex
│   ├── manpage.sgml.ex
│   ├── manpage.xml.ex
│   ├── postinst.ex
│   ├── postrm.ex
│   ├── preinst.ex
│   ├── prerm.ex
│   ├── README
│   ├── README.Debian
│   ├── README.source
│   ├── rules
│   ├── salsa-ci.yml.ex
|	├── snake.6
│   ├── snake
│   │   ├── DEBIAN
│   │   │   ├── control
│   │   │   └── md5sums
│   │   └── usr
│   │       ├── bin
│   │       │   └── snake
│   │       ├── lib
│   │       │   └── snake-1.py
│   │       └── share
│   │           ├── applications
│   │           │   └── snake.desktop
│   │           ├── doc
│   │           │   └── snake
│   │           │       ├── changelog.gz
│   │           │       ├── copyright
│   │           │       └── README.Debian
│   │           ├── doc-base
│   │           │   └── snake.snake
│   │           └── pixmaps
│   │               └── snake-icon
│   ├── snake.cron.d.ex
│   ├── snake.doc-base.ex
│   ├── snake-docs.docs
│   ├── snake.substvars
│   └── source
│       └── format
├── snake
├── snake-1.py
├── snake.desktop
└── snake-icon.png
```
# Lab 9: Apparmor #Lab 
***Inspiratie op uw favoriete youtube kanaal: https://www.youtube.com/watch?v=VCrnQTCNRYsLinks to an external site.***
`sudo apt-get install apparmor-utils auditd mono-complete`

REF1. http://www.mono-project.com/docs/getting-started/application-deployment/
#### 1. Download het mono programma getstuff.exe
Dit programma downloadt het python programma httpserver.py
Volg de Layout Recommendation vanuit REF1 om dit als een regulier programma op je systeem te
voorzien:
- maak de directory /usr/lib/getstuff aan
`sudo mkdir /usr/lib/getstuff`
- copieer getstuff.exe naar /usr/lib/getstuff/getstuff.exe
`sudo cp Downloads/getstuff-1.exe /usr/lib/getstuff/`
- schrijf in /usr/bin/getstuff dat je /usr/lib/gestuff/getstuff.exe met mono opstart
```bash
#!/bin/sh
exec /usr/bin/mono usr/lib/getstuff/getstuff.exe "$@"
```
- maak /usr/bin/getstuff executable
`sudo chmod +x /usr/bin/getstuff`

#### 2. Maak als root een apparmor profiel aan voor getstuff
a) `sudo aa-genprof /usr/bin/getstuff`
b) Start nu de applicatie
- IN EEN ANDERE TERMINAL
- ALS GEWONE GEBRUIKER.
Doe dit vanuit de homedirectory van de gebruiker.
Het profiel werkt nu in complain mode
c) Kies Scan en duid alles aan wat je nodig hebt!
Let er op dat getstuff moet werken voor alle gebruikers!
Let op temp files en procesnummers
d) Finish (profiel werkt in enforce mode)

```
#S->I->A->save->finish
#	I en A blijven herhalen tot er andere opties komen
```
#### 3. Doe sudo su en tik cd /root
Start nu het programma getstuff op als root.
De download mag niet meer lukken (kijk na of het bestand httpserver.py geschreven wordt)
Kijk na in /var/log/audit/audit.log
```
sudo su 
cd /root
getstuff
ls
```

#### 4. Zorg ervoor dat root nu ook kan downloaden naar de directory /root
```bash
sudo aa-logprof 
sudo apparmor_parser -r /etc/apparmor.d/usr.bin.getstuff
```
**of**
```bash
nano /etc/apparmor.d/usr.bin.getstuff

sudo systemctl reload apparmor
```
voeg: `/root/httpserver.py w`, toe

#### 5. Maak, gelijkaardig aan getstuff.exe, volgens REF1 een executable /usr/bin/httpserver voor httpserver.py aan. (deze draait niet met mono maar met python3)
```bash
sudo mkdir /usr/lib/httpserver
sudo cp ./Downloads/httpserver.py /usr/lib/httpserver/httpserver.py
sudo touch /usr/bin/httpserver
echo -e '#!/bin/sh\npython3 /usr/lib/httpserver/httpserver.py "$@"' | sudo tee /usr/bin/httpserver
sudo chmod +x /usr/bin/httpserver
```
#### 6. Maak nu ook een apparmor profiel voor /usr/bin/httpserver. Let erop dat je ook uittest of je de httpserver kan bereiken vanuit je browser vanop http://localhost:1234
`sudo aa-genprof /usr/bin/httpserver`

***Opmerking: Je profiel opnieuw maken kan je door /etc/apparmor.d/usr.bin.getstuff te verwijderen en de machine terug op te starten***
# Lab 10: Ansible
2 machines **`master` `192.168.57.102` en `node` `192.168.57.103` 
#### Nodige installaties
##### Master:
```bash
hostnamectl set-hostname master
sudo su
apt install ansible openssh-server
```
Pas in `/etc/hosts` alles aan zodat alleen de host only ip's gebruikt worden voor de hostnames
```
127.0.0.1 localhost
192.168.57.102 master
192.168.57.103 node
```

#### Aanmaken van juiste gebruikers *(bijde machines)*
```bash
groupadd ansible
useradd -m -u 1001 -g ansible ansible
echo ansible:supersecret| chpasswd -c SHA512
```
verander in etc passwd de regel naar
`ansible:x:1001:1001::/home/ansible:/bin/bash`
~~`ansible:x:1001:1001::/home/ansible:/bin/sh`~~
#### Passwordloze ssh
```bash
su - ansible
ssh-keygen -t rsa #default locatie !!geen password
ssh-copy-id node #of master op node
```

#### ansible test
```bash
ansible all --inventory "node," -m shell -a 'echo Ansible '
```
```output
node | CHANGED | rc=0 >>
Ansible
```
#### Asnible hosts
Schrijf in `/etc/ansible/hosts`(moest folder en bestand nog aanmaken)
```
[nodes]
node
```
Je kan nu zonder --inventory of -i een commando uitvoeren op de shell op een host
```bash
ansible all -m shell -a 'echo Ansible op ;hostname'
```

#### Root rechten
Ansible user op node MOET root rechten hebben ->
`echo "ansible ALL=(ALL) NOPASSWD: ALL"> /etc/sudoers.d/ansi`
test on master:
```bash
ansible all -m shell -a 'sudo whoami'
```
```
[WARNING]: Consider using 'become', 'become_method', and 'become_user' rather than running sudo
node | CHANGED | rc=0 >>
root
```

#### Apt install on node
*via master*
```bash
#installeer stress en htop op node
ansible all -b -m apt -a "name=stress state=present"
ansible all -b -m apt -a "name=htop state=present"
# verwijderen 
ansible all -b -m apt -a "name=htop state=absent"
```

#### Anisble copy
```bash
echo '#!/bin/bash' > cmd.sh
echo 'echo Hello world' >> cmd.sh
echo 'ip a|grep -o 192.*/|cut -d/ -f1' >> cmd.sh
ansible all -m copy -a 'src=cmd.sh dest=cmd.sh'
ansible all -m copy -a 'src=cmd.sh dest=cmd.sh mode=755'
ansible all -m shell -a './cmd.sh'
```
```
node | CHANGED | rc=0 >>
Hello world
192.168.57.103
```
#### templates
Soms willen we in bestanden variabelen at runtime aanpassen. Om dat te doen hebben we
templates nodig.
Templates worden in jinja2 geschreven. Variabelen worden ingesloten in dubbele
accolades.
in file: `cmd-template.sh.j2`
```bash
#!/bin/bash
echo {{ bericht }}
ip a|grep -o 192.*/|cut -d/ -f1
```
```bash
ansible all -m template -a "src=cmd-template.sh.j2 dest=cmd-template.sh mode=755" -e "bericht=Hello"

ansible all -m shell -a './cmd-template.sh'
```
```
node | CHANGED | rc=0 >>
Hello
192.168.57.103
```

#### synchronize
Ansible kan gebruik maken van rsync om bestanden te synchroniseren tussen source en
destination.
```bash
#rsync installeren
ansible all -b -m apt -a "name=rsync state=present"

ansible all -m synchronize -a 'src=cmd.sh dest=cmd.sh'
```

#### Get_url
```bash
ansible all -m get_url -a"url='https://github.com/luckylittle/ansible-cheatsheet/blob/master/ansible-cheatsheet.txt' dest=."

ansible all -m shell -a 'ls -al *.txt'
```
```
node | CHANGED | rc=0 >>
-rw-rw-r-- 1 ansible ansible 658458 jan 14 16:09 ansible-cheatsheet.txt
```

#### Git
```bash
ansible all -b -m apt -a "name=git,tree state=present"
ansible all -m git -a"repo=https://github.com/luckylittle/ansible-cheatsheet.git dest=./cheatsheet"

ansible all -m shell -a 'tree cheat*'
```
```
node | CHANGED | rc=0 >>
cheatsheet
├── ansible-cheatsheet.txt
├── DO407-exam-notes.txt
├── ec2.ini
├── ec2.py
└── README.md
0 directories, 5 files
```
## Ansible Playbook (webserver)
Een ansible playbook definieert specifieke taken.
Een YAML bestand start met drie dashes --- om aan te geven dat het een YAML bestand
is.
• Inspringen moet met spaties en mag niet met een tab.
• Opsommingen maak je met een streepje
• Elke task krijgt een naam en een actie.
Maak het bestand `playbook-apt.yml` met volgende inhoud:
```yaml
---
- name:
	hosts: all
	tasks:
	- name: Install apache2 package
		apt: name=apache2 state=present update_cache=true
	- name: Start apache2 server
		service: name=apache2 state=started
```
Hierbij geeft "hosts" weer op welke hosts het script moet draaien. Bij "hosts:all" kijkt
ansible in het bestand /etc/ansible/hosts.
De module apt installeert apt packages (state=present). Wanneer apt de optie
update_cache=true meekrijgt zal er eerst een apt-get update plaatsvinden (de pakketlijsten
krijgen dan een update zodat de laatste versies bekend zijn)
```bash
ansible-playbook --become playbook-apt.yml
```
#### webserver deploy
File: `webserver-deploy.yml`
```yaml
---
- name: Webserver Playbook - Deploy
  hosts: all
  tasks:
  - name: Install apache2 package
    apt:
      name: apache2
      state: present
  - name: Start and enable apache2 service
	service:
      name: apache2
      enabled: true
      state: started
  - name: Create a custom index.html file
    copy:
      mode: "644"
      dest: /var/www/html/index.html
      content: |
        Ansible Works
        This is my webpage
```
File: `webserver-remove.yml`
```yaml
---
- name: Webserver playbook - Remove
  hosts: all
  tasks:
  - name: Stop en disable apache2 service
    service:
      name: apache2
      enabled: false
      state: stopped
  - name: Remove apache2 package
    apt:
      name: apache2
      state: absent
```
```bash
ansible-playbook webserver-deploy.yml
```
#### Webserver met een template
File: `webserver-fact.yml`
```yaml
---
- name: Update index.html with host IP
  hosts: all
  become: true
  gather_facts: true
  tasks:
  - name: Get host IP address
    set_fact:
      host_ip: "{{ ansible_default_ipv4.address }}"
  - name: Update index.html with host IP
    template:
      mode: "644"
      src: index-ip.html.j2
      dest: /var/www/html/index.html
```
File: `template index-ip.html.j2`
```HTML
<!DOCTYPE html>
<html>
<head>
  <title>Welcome to My Ansible Server</title>
</head>
<body>
  <h1>Hello World!</h1>
  <p>My IP is: {{ host_ip }}</p>
</body>
</html>
```
#### Handlers
Handlers zijn zoals tasks in een playbook. In tegenstelling tot een task, zal een handler
enkel opgestart worden wanneer ze door een notify getriggerd worden bij een
statusverandering.
File: `webserver-handler.yml`
```yaml
---
- name: Restart Apache when website changes
  hosts: all
  become: true
  gather_facts: true
  tasks:
  - name: Install apache2 package
    apt:
      name: apache2
      state: present
  - name: Start and enable apache2 service
    service:
      name: apache2
      enabled: true
      state: started
  - name: Create a custom index.html file
    copy:
      mode: "644"
      dest: /var/www/html/index.html
      content: |
        Ansible time {{ ansible_date_time.iso8601 }}
        on my webpage
    notify:
    - Restart Apache
    - Handler
  handlers:
  - name: Restart Apache Handler
    service:
      name: apache2
      state: restarted
```
```bash
ansible-playbook webserver-handler.yml
```
#### Variabele declareren
File: `variables.yml`
```yaml
---
- name: Variabele gebruiken
  hosts: all
  vars:
    - bericht1: "hello"
    - bericht2: "world"
  tasks:
  - name: Show var
    debug: msg="Bericht {{ bericht1 }} {{ bericht2 }}"
```
```bash
ansible-playbook variables.yml
```
File: `mysecrets.yml`
```yaml
secret1: "hello"
secret2: "secretworld"
```
File: `variables-with-file.yml`
```yaml
---
- name: Variabele file gebruiken
  hosts: all
  vars_files:
  - mysecrets.yml
  tasks:
  - name: Show var
    debug: msg="Bericht {{ secret1 }} {{ secret2 }}"
```
#### Facts
File: `myfacts.yml`
```yaml
---
- name: Facts gebruiken
  hosts: all
  gather_facts: true
  tasks:
  - name: Show a fact var
    ansible.builtin.debug:
      var: ansible_facts['nodename']
  - name: Show a fact debug message
    debug: msg="Hostname {{ ansible_facts['nodename'] }} "
  - name: Echo a fact in the shell put it in myvar
    shell: "echo 'Host {{ansible_nodename}}'"
    register: myvar
  - name: Show a variable as debug message
    debug: msg=" {{ myvar.stdout }} "
```
```bash
ansible-playbook myfacts.yml
```
#### Output van commands
File: `date-time.yml`
```yaml
---
- name: Uses ansible_date_time fact and date cmd
  hosts: all
  tasks:
  - name: Ansible fact - ansible_date_time
    debug:
      msg: "Date: {{ ansible_date_time.date }} Time{{ ansible_date_time.time }}"
  - name: Get timestamp from the system
    shell: "date +%Y-%m-%d%H-%M-%S"
    register: datetime
  - name: Set variables
    set_fact:
      cur_date: "{{ datetime.stdout[0:10]}}"
      cur_time: "{{ datetime.stdout[10:]}}"
  - name: System timestamp - date time
    debug:
      msg: "Date: {{ cur_date }} Time: {{ cur_time }}"
```

# Lab 11: Nagios #Lab #todo 

# Lab 12: ISCSI #Lab 
**Alle te installeren pakketten:**
target: `sudo apt install openssh-server targetcli-fb` `192.168.57.107`
node: `sudo apt install openssh-server open-iscsi lvm2`

File: `/etc/ssh/sshd_config`
```
PermitRootLogin yes
UseDNS no
GSSAPIAuthentication no
```
```bash
systemctl restart ssh
systemctl enable ssh

sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw reload
```

#### Target config
```bash
#apt install openssh-server targetcli-fb -y
apt remove tgt -y # vloekt met targetcli
mkdir -p /iscsi
```
In dit voorbeeld gebruiken we FileIO (een schijfbestand of image) en maken twee stores
aan. Eén voor data en de andere voor fencing:
```bash
targetcli

cd /backstores/fileio
create datadisk /iscsi/data1.img 550M
cd /iscsi
create iqn.2025-01.be.kdg:data1

#LUN aanmaken
cd iqn.2025-01.be.kdg:data1/tpg1/luns
create /backstores/fileio/datadisk
```
Bij de Access Control List kan je instellen welke initiator is toegelaten. Enkel die naam
krijgt toegang. In het voorbeeld hieronder is dat `iqn.2025-01.be.kdg:init1`.
Meestal wordt ook een eenvoudige chap authenticatie ingesteld met een username en
password.
```bash
cd /iscsi/iqn.2025-01.be.kdg:data1/tpg1/acls
create iqn.2025-01.be.kdg:init1

exit
```
##### Bekijken van instellingen
```bash
targetcli
cd /iscsi/
ls
```
```
o- iscsi ............................................................. [Targets: 1]
  o- iqn.2025-01.be.kdg:data1 ........................................... [TPGs: 1]
    o- tpg1 ................................................ [no-gen-acls, no-auth]
      o- acls ........................................................... [ACLs: 1]
      | o- iqn.2025-01.be.kdg:init1 .............................. [Mapped LUNs: 1]
      |   o- mapped_lun0 .............................. [lun0 fileio/datadisk (rw)]
      o- luns ........................................................... [LUNs: 1]
      | o- lun0 ........... [fileio/datadisk (/iscsi/data1.img) (default_tg_pt_gp)]
      o- portals ..................................................... [Portals: 1]
        o- 0.0.0.0:3260 ...................................................... [OK]
```
```bash
#backup
targetcli saveconfig /root/target.json
#restore
targetcli restoreconfig /root/target.json
```
```bash
ufw allow 3260/tcp
ufw reload
```
#### Node config
```bash
#sudo apt install openssh-server open-iscsi lvm2
iscsiadm -m discovery -t sendtargets -p 192.168.57.107
```
File `/etc/iscsi/initiatorname.iscsi`
```
InitiatorName=iqn.2025-01.be.kdg:init1
```
```bash
systemctl restart iscsid
iscsiadm --mode node --targetname iqn.2025-01.be.kdg:data1 --portal 192.168.57.107 --login
```

#### Opdracht
```bash
sudo mkfs.ext4 /dev/sdb
sudo mkdir -p /www
sudo mount /dev/sdb /www
#check
df -h
```
```bash
apt install apache2
echo "<h1>iSCSI Webserver Test</h1>" | sudo tee /www/index.html
```
File `/etc/apache2/sites-available/000-default.conf`
```
DocumentRoot /www 

# add
<Directory /www>
        Options Indexes FollowSymlinks
        AllowOverride None
        Require all granted
</Directory>
```
```bash
sudo systemctl restart apache2
curl http://localhost
```
Bij opstarten mounten
```bash
sudo iscsiadm -m node --targetname iqn.2025-01.be.kdg:data1 --portal 192.168.57.107 --op update -n node.startup -v automatic
echo "/dev/sdb /www ext4 defaults,_netdev,nofail 0 0" | sudo tee -a /etc/fstab
sudo umount /www
sudo mount -a
df -h
sudo systemctl enable apache2
```
```bash
sudo reboot
```
```bash
df -h

systemctl status apache2

curl http://localhost
```
Script voor nginx van @Kato namen en ip nog niet aangepast
```bash
#!/bin/bash
set -e

sudo apt update
sudo apt install -y open-iscsi nginx

sudo iscsiadm -m discovery -t sendtargets -p 192.168.56.154
sudo iscsiadm -m node --targetname iqn.2024-10.be.kdg:data1 --portal 192.168.56.154 --login
sudo iscsiadm -m node --targetname iqn.2024-10.be.kdg:data1 --portal 192.168.56.154 --op update -n node.startup -v automatic

sudo mkfs.ext4 /dev/sdb
sudo mkdir -p /www
sudo mount /dev/sdb /www

echo "/dev/sdb /www ext4 defaults,_netdev,nofail 0 0" | sudo tee -a /etc/fstab

echo "<h1>iSCSI Webserver Test</h1>" | sudo tee /www/index.html
sudo sed -i 's|root /var/www/html;|root /www;|' /etc/nginx/sites-available/default
sudo systemctl restart nginx
sudo systemctl enable nginx

echo "Setup complete! Test your webserver at http://<initiator-ip>"
```
# Lab 13: Loadbalancing #Lab  
3 ubuntu servers 1master (loadbalancer) 2 nodes met apche2
File: `/etc/hosts`
```
127.0.0.1 localhost
127.0.1.1 master
192.168.57.49   master
192.168.57.51   node1
192.168.57.52   node2
# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```
zorg dat alle pings naar elkaar werken
#### Apache mod proxy balancer
```bash
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod headers
sudo a2enmod proxy_balancer

sudo systemctl restart apache2
```
File: `/etc/apache2/apache2.conf`
```
Header add Set-Cookie "BALANCEID=hej.%{BALANCER_WORKER_ROUTE}e; path=/;" env=BALANCER_ROUTE_CHANGED

<Location /balancer-manager>
        SetHandler balancer-manager
        Require all granted
</Location>

ProxyRequests Off

<Proxy balancer://192.168.56.49>
        BalancerMember http://node1:80 route=node1
        BalancerMember http://node2:80 route=node2
</Proxy>

ProxyPass /balancer-manager !

ProxyPass / balancer://192.168.56.49/ stickysession=BALANCEID nofailover=Off
ProxyPassReverse / http://node1/
ProxyPassReverse / http://node2/
```
```
sudo a2enmod lbmethod_byrequests
sudo systemctl restart apache2
```

```HTML
<html>
    <body>
        <h1>Welkom op Node 1</h1>
        <p>Dit is de webpagina van Node 1.</p>
    </body>
</html>
```

```HTML
<html>
    <body>
        <h1>Welkom op Node 2</h1>
        <p>Dit is de webpagina van Node 2.</p>
    </body>
</html>

```
####  TESTSCRIPT WEBSERVER
Schrijf een script dat om de 2 seconden de inhoud van een website opvraagt en dit toont op het scherm.
Wanneer index.html op node 1 "Webserver 1" en op node 2 "Webserver 2" bevat, toont het script:
Webserver 1
Webserver 2
Ga er van uit dat er html code (en niet enkel tekst) staat in het bestand index.html.
Bij falen toont het script volgende foutboodschap:
"Verbinding mislukt"
Test het script uit op je APACHE PROXY BALANCER, tijdens het afwisselend uitschakelen van node1 en node2
```bash
#!/bin/bash

# Ensure the script is called with the load balancer IP as the first argument
if [ -z "$1" ]; then
    echo "Usage: $0 <LOAD_BALANCER_IP>"
    exit 1
fi

LOAD_BALANCER_IP="$1"

check_website() {
    CONTENT=$(curl -s "$LOAD_BALANCER_IP")

    if echo "$CONTENT" | grep -q "Node 1"; then
        echo "Webserver 1"
    elif echo "$CONTENT" | grep -q "Node 2"; then
        echo "Webserver 2"
    else
        echo "Connection failed"
    fi
}

while true; do
    check_website
    sleep 2
done
```
je kan websevers laten wegvallen met `sudo systemctl stop apache2`