# TP2 Reseau

## Ping hôte à la VM
```bash
PS C:\Users\Notitou> ping 192.168.127.10

Envoi d’une requête 'Ping'  192.168.127.10 avec 32 octets de données :
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64
Réponse de 192.168.127.10 : octets=32 temps<1ms TTL=64

Statistiques Ping pour 192.168.127.10:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
```
---
## Ping de la VM à l'hôte

```bash
[root@localhost ~]# ping 192.168.127.1
PING 192.168.127.1 (192.168.127.1) 56(84) bytes of data.
64 bytes from 192.168.127.1: icmp_seq=1 ttl=128 time=0.215 ms
64 bytes from 192.168.127.1: icmp_seq=2 ttl=128 time=0.452 ms
64 bytes from 192.168.127.1: icmp_seq=3 ttl=128 time=0.397 ms
64 bytes from 192.168.127.1: icmp_seq=4 ttl=128 time=0.372 ms
--- 192.168.127.1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3006ms
rtt min/avg/max/mdev = 0.215/0.359/0.452/0.088 ms

```
---
## Table de routage de l'hôte
```bash
PS C:\Users\Notitou> route print -4
===========================================================================
Liste d Interfaces
  5...70 5a 0f 1e e0 1e ......Realtek PCIe FE Family Controller
 12...0a 00 27 00 00 0c ......VirtualBox Host-Only Ethernet Adapter #2
 22...46 1c a8 7b 10 ed ......Microsoft Wi-Fi Direct Virtual Adapter #5
 26...44 1c a8 7b 10 ed ......Microsoft Wi-Fi Direct Virtual Adapter #6
 30...44 1c a8 7b 10 ed ......Realtek RTL8723BE 802.11 bgn Wi-Fi Adapter
 20...44 1c a8 7b 10 ee ......Bluetooth Device (Personal Area Network)
  1...........................Software Loopback Interface 1
===========================================================================

IPv4 Table de routage
===========================================================================
Itinéraires actifs :
Destination réseau    Masque réseau  Adr. passerelle   Adr. interface Métrique
          0.0.0.0          0.0.0.0      10.33.3.253      10.33.3.184     55
        10.33.0.0    255.255.252.0         On-link       10.33.3.184    311
      10.33.3.184  255.255.255.255         On-link       10.33.3.184    311
      10.33.3.255  255.255.255.255         On-link       10.33.3.184    311
        127.0.0.0        255.0.0.0         On-link         127.0.0.1    331
        127.0.0.1  255.255.255.255         On-link         127.0.0.1    331
  127.255.255.255  255.255.255.255         On-link         127.0.0.1    331
    192.168.127.0    255.255.255.0         On-link     192.168.127.1    281
    192.168.127.1  255.255.255.255         On-link     192.168.127.1    281
  192.168.127.255  255.255.255.255         On-link     192.168.127.1    281
        224.0.0.0        240.0.0.0         On-link         127.0.0.1    331
        224.0.0.0        240.0.0.0         On-link     192.168.127.1    281
        224.0.0.0        240.0.0.0         On-link       10.33.3.184    311
  255.255.255.255  255.255.255.255         On-link         127.0.0.1    331
  255.255.255.255  255.255.255.255         On-link     192.168.127.1    281
  255.255.255.255  255.255.255.255         On-link       10.33.3.184    311
===========================================================================
Itinéraires persistants :
  Aucun
```
---
## Table de routage de la VM
```bash
[root@localhost ~]# ip route
default via 10.0.2.2 dev enp0s3 proto dhcp metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.15 metric 100
169.254.0.0/16 dev enp0s8 scope link metric 1003
192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10

```
---
## Ligne qui permet de discuter via host-only

Sur l'host:

```bash
 192.168.127.0    255.255.255.0         On-link     192.168.127.1    281
```

Sur la VM :
```bash
192.168.127.0/24 dev enp0s8 proto kernel scope link src 192.168.127.10
```
---
## Utilisation de curl
```bash
[root@localhost ~]# curl google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="http://www.google.com/">here</A>.
</BODY></HTML>
```
---
## Utilisation de dig ynov.com
```bash
;; SERVER: 10.33.10.20#53(10.33.10.20)
```
---
## Utilisation de dig google.com
```bash
;; SERVER: 10.33.10.20#53(10.33.10.20)
```
---
# Notion de ports et SSH

## Utilisation de la commande "ss -4"
```bash
[root@localhost ~]# ss -4
Netid  State      Recv-Q Send-Q Local Address:Port                 Peer Address:Port
tcp    ESTAB      0      0      192.168.127.10:ssh                  192.168.127.1:32487
```
---
## Utilisation commande "ss -t"

```bash
[root@localhost ~]# ss -t
State      Recv-Q Send-Q Local Address:Port                 Peer Address:Port   
ESTAB      0      64     192.168.127.10:ssh                  192.168.127.1:tcoaddressbook
```
---
## Utilisation de la commande "ss -l"
```bash
tcp    LISTEN     0      128     *:ssh                   *:*
tcp    LISTEN     0      128    :::ssh                  :::*
```
---
# SSH

![putty](images/putty.png)

---
# FireWall

Nous avons modifié le fichier `sshd_config` pour changer le port, et l'avons mis au port 2222.
Puis avant exécuter la commande `ss -naltp4` pour bien vérifié que le port a bien été modifié.
```bash
[root@localhost ssh]# ss -naltp4
State       Recv-Q Send-Q                                                 Local Address:Port                                                                Peer Address:Port
LISTEN      0      128                                                                *:2222                                                                           *:*             
```
---
On ne peut pas se connecter au nouveau port car le FireWall nous en empêche, pour pouvoir se reconnecter nous devons ajouter et autorisé le nouveau port grâce au FireWall.

```bash
[root@localhost ssh]# firewall-cmd --add-port=2222/tcp --permanent
```
Puis pour que les modifications soit actives : 
```bash
[root@localhost ssh]#firewall-cmd --reload
```
Puis pour vérifié que les modifications se sont bien faites avant d'essayer de se connecter nous pouvons faire cela : 
```bash
[root@localhost ssh]# firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: ssh dhcpv6-client
  ports: 2222/tcp
  protocols:
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:

```
