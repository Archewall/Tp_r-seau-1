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

enfin pour verifier que l'ip a bien changer utiliser la commande " ip a" 