# TP IV
## I DHCP Client
adresse du serveur DHCP (ynov) : 
```
ipconfig /all
```
trouver les serveur DHCP dans la carte réseau sans fil : 10 . 33 . 79 . 254

heure d'obtention du bail DHCP, 
Vendredi 27 octobre 2023 09:25:16

expire samedi 28 octobre 2023 09:24:37

forcer l'os a faire un échange :
```
ipconfig /release
```
```
ipconfig /renews
```
ce qui me redonne la même adresse IP 

capture des 4 trames DHCP
[capture Wireshark](./tp4_dhcp_client.pcapng)

la trame "offer" contient les information proposé au client

## serveur DHCP

### Tableau d'adressage 

    machine        |    Lan1
```
router.tp4.b1      |    10.4.1.254/24  
dhcp.tp4.b1        |    10.4.1.253/24
node1.tp4.b1       |    N/A
node2.tp4.b1       |    10.4.1.12/24

pour attribuer une ip a chacune des machines j'ai utilisé la commmande :

```
cd /etc/sysconfig/network-scripts
sudo nano ifcfg-enp0s3
DEVICE=enp0s3

BOOTPROTO=static
ONBOOT=yes

IPADDR= l'ip de la machine que je souhaite changer 
NETMASK=255.255.255.0

sudo systemctl restart NetworkManager
```

**Démarrage du serveur DHCP**

```bash
[alexandre@dhcp ~]$ sudo systemctl enable --now dhcpd
Created symlink /etc/systemd/system/multi-user.target.wants/dhcpd.service → /usr/lib/systemd/system/dhcpd.service.
[alexandre@dhcp ~]$ sudo firewall-cmd --add-service=dhcp
success
[alexandre@dhcp ~]$ sudo firewall-cmd --runtime-to-permanent
success
```

**Vérification de l'état du serveur DHCP**

```bash
[alexandre@dhcp ~]$ sudo systemctl status dhcpd
● dhcpd.service - DHCPv4 Server Daemon
     Loaded: loaded (/usr/lib/systemd/system/dhcpd.service; enabled; preset>
     Active: active (running) since Fri 2023-10-27 15:09:30 CEST; 4min 49s >
```


- node1.tp4.b1 a bien récupéré une IP dynamiquement

```bash
[alexandre@localhost ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:de:36:da brd ff:ff:ff:ff:ff:ff
    inet 10.4.1.137/24 brd 10.4.1.255 scope global ✨dynamic✨ noprefixroute enp0s3
       valid_lft 336sec preferred_lft 336sec
    inet6 fe80::a00:27ff:fede:36da/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

```bash
[alexandre@localhost ~]$ nmcli con show enp0s3
```

- node1.tp4.b1 a enregistré un bail DHCP
   - **déterminer la date exacte de création du bail**

      vendredi 27 octobre 2023 15:21:37 GMT+02:00 DST
   - **déterminer la date exacte d'expiration**

      vendredi 27 octobre 2023 16:40:02 GMT+02:00 DST
   - **déterminer l'adresse IP du serveur DHCP**

      dhcp_server_identifier = 10.4.1.253

- Vous pouvez ping router.tp4.b1 et node2.tp4.b1 grâce à cette nouvelle IP récupérée

```bash
alexandre@localhost ~]$ ping 10.4.1.254
PING 10.4.1.254 (10.4.1.254) 56(84) bytes of data.
64 bytes from 10.4.1.254: icmp_seq=1 ttl=64 time=0.285 ms
64 bytes from 10.4.1.254: icmp_seq=2 ttl=64 time=0.395 ms

--- 10.4.1.254 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1036ms
rtt min/avg/max/mdev = 0.285/0.340/0.395/0.055 ms
alexandre@localhost ~]$ ping 10.4.1.12
PING 10.4.1.12 (10.4.1.12) 56(84) bytes of data.
64 bytes from 10.4.1.12: icmp_seq=1 ttl=64 time=0.407 ms
64 bytes from 10.4.1.12: icmp_seq=2 ttl=64 time=0.349 ms

--- 10.4.1.12 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1044ms
rtt min/avg/max/mdev = 0.349/0.378/0.407/0.029 ms
```
