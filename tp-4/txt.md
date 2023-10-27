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

