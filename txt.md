# TP 1 😺
## I. Affichage d'infos !
```    
ipconfig 
```              
### interface wi-fi
        nom : carte réseau sans fil wi-fi  

        adresse mac: E0-0A-F6-B0-73-D5

        adresse ip: 10.33.51.62

### interface ethernet

        nom : carte Ethernet Ethernet

        adresse mac : E4-A8-DF-D1-98-CA

        adresse : je ne suis pas connecté a un reseaux

### Afficher le gateway
```
ipconfig /all
```
Passerelle par défaut : 10.33.51.254

### mac de la passerrelle
```
arp /a
```
table ARP, adresse physique :  7c-5a-1c-cb-fd-a4

### afficher les infos de la carte IP

    ouvrir panneau de config - réseau internet - centre réseau partage - modifier les paramètre de partage - wifi - détail 

carte ip : 10.33.51.62

mac : E0-0A-F6-B0-73-D5

gateway : 10.33.51.254


### 2. Modification d'information
#### A. Modification d'adresse IP

    configuration wi-fi - wi-fi ynov - modification attribution adresse ip - manuel

adresse IP : 10.33.51.68

masque de sous réseaux : 255.255.255.0

gateway : 10.33.51.254


## II. exploration locale en DUO (avec Diane😺)

🌞 **Modification de nos adresses IP en utilisant le GUI (windows)**

Adresse IP choisis : 10.10.10.33 et 10.10.10.32

`>> ipconfig /all`

> Résultat sur ma machine :

    Carte Ethernet Ethernet :
        Adresse IPv4. . . . . . . . . . . . . .: 10.10.10.32 (préféré)

`>> ping 10.10.10.33`

> Résultat sur ma machine :

    Statistiques Ping pour 10.10.10.33:
        Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
    Durée approximative des boucles en millisecondes :
        Minimum = 3ms, Maximum = 5ms, Moyenne = 4ms

`>> arp -a`

> Résultat sur ma machine :

    Interface : 10.10.10.32 
        Adresse Internet      Adresse physique      Type
        10.10.10.33           D8-80-83-D0-03-05    dynamique

*Adresse MAC de l'autre machine :*  D8-80-83-D0-03-05

### 4. netcat


## III. Manipulations d'autres outils/protocoles côté client

### 1. DHCP
```
ipconfig /all
```
adresse ip DHCP : 10.33.51.254

date d'expiration :  mardi 17 octobre 2023 13:45:23

### DNS
```
ipconfig /all
```
ip serveur DNS : 10.33.10.2

pour google : nslookup google.com = 8.8.8.8
pour Ynov : 