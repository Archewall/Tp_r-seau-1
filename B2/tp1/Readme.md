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


#### table de routage
```bash
Destination réseau  | Masque réseau  |Adr. passerelle  |Adr. interface Métrique
          0.0.0.0   |      0.0.0.0   | 10.33.79.254    | 10.33.73.76     30
```

## II. Go further

ping b2.hello.vous

Envoi d’une requête 'ping' sur b2.hello.vous [1.1.1.1] avec 32 octets de données :
Réponse de 1.1.1.1 : octets=32 temps=23 ms TTL=55

### Go mater une vidéo youtube et déterminer, pendant qu'elle tourne

adresse IP du serveur auquel vous êtes connectés : 10.33.73.76
le port du serveur auquel vous êtes connectés : 443
le port que votre PC a ouvert en local : 56866

### Requêtes DNS

Déterminer...

. à quelle adresse IP correspond le nom de domaine www.thinkerview.com : 188.114.96.6

. à quel nom de domaine correspond l'IP 143.90.88.12 :  
```bash
nslookup 143.90.88.12
Serveur :   one.one.one.one
Address:  1.1.1.1

Nom :    EAOcf-140p12.ppp15.odn.ne.jp
Address:  143.90.88.12
```

Déterminer...
par combien de machines vos paquets passent quand vous essayez de joindre www.ynov.com : 
```bash
tracert -d www.ynov.com

Détermination de l’itinéraire vers www.ynov.com [172.67.74.226]
avec un maximum de 30 sauts :

  1    17 ms     2 ms     2 ms  10.33.79.254
  2     9 ms     4 ms     2 ms  195.7.117.145
  3    19 ms     4 ms     2 ms  86.79.195.237
  4    11 ms     3 ms     4 ms  86.65.224.196
  5    19 ms    14 ms    12 ms  194.6.147.164
  6     *        *        *     Délai d’attente de la demande dépassé.
  7     *        *        *     Délai d’attente de la demande dépassé.
  8    35 ms    18 ms    17 ms  172.67.74.226

Itinéraire déterminé.
```

### ip publique 
ip publique : 195.7.117.146


## Capture ARP
[Lien vers capture ARP](./arp.pcapng)

## Capture DNS

[Lien vers capture ARP](./Dns.pcap)

## Capture TCP

[Lien vers capture ARP](./tcp.pcap)