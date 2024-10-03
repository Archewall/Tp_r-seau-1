### TP1 : Maîtrise réseau du votre poste

## I. Affichage d'infos !

```bash 
ipconfig /all
```
    adresse mac: E0-0A-F6-B0-73-D5
    adresse ip: 10.33.73.76
    notation CIDR: 192.168.133.1/24
    masque de sous réseau : 255.255.240.0
```
adresse réseau LAN auquel connectés en WiFi : 10.33.73.76
Nombre d'adresse IP dispo dans ce réseau : 254 adresses IP disponibles pour les hôtes
```
```bash
hostname : Aqua
```

IP passerelle du réseau :  10.33.79.254

#### pour trouver mon adresse MAC de la passerelle du réseau
```bash
ping mon IP passerelle, suite a ca les infos vont ce rendre dans la table arp je dois donc : arp -a 
trouver l'interface avec mon IP ici : 10.33.73.76 et ensuite chercher mon ip passerelle l'adresse mac est écrit a coté

Adresse mac passerelle réseau :  7c-5a-1c-d3-d8-76
```

adresse ip DHCP : 10.33.79.254
serveur DNS :  1.1.1.1

```bash
Destination réseau  | Masque réseau  |Adr. passerelle  |Adr. interface Métrique
          0.0.0.0   |      0.0.0.0   | 10.33.79.254    | 10.33.73.76     30
```