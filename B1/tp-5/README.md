# TP5 : TCP, UDP et services réseau

## I. first steps 

application :

- league of legends : UDP; 
 ip = 162.249.77.233 ; port du serveur = 5337 ; port local = 55686


- DISCORD : UDP ;
ip = 66.22.196.155
; port du serveur = 50015 ; 
port local = 50040


- steam DL cube world : TCP ; 
ip = 185.25.182.3 ; 
port du serveur = 443 ; 
port local = 53145


- youtube : UDP ; 
ip = 91.68.245.14 ; 
port du serveur = 443 ;
 port local = 49665


- disney + : TCP ; 
ip = 157.23.228.39 ; 
port du serveur = 443 ;
 port local = 52987


## II. Setup virtuel

### 1. SSH

création d'une machine virtuel node1.tp5.b1 avec une ip : 10.5.1.11

connection en ssh sur git bash
```
ssh alexandre@10.5.1.11
```
ssh utilise un TCP


on peut verifier grace a wireshark (capture TP_3_way)

avec netstat
```

proto   |   adresse locale |    adresse distante 

TCP     |   10.5.1.1:50715 |    10.5.1.11:ssh

```

ou grace a la  commande 
```
ss -tulnp
```

### 2. routage

 #### Prouvez que
.  node1.tp5.b1 a un accès internet

```
[alexandre@node1 network-scripts]$ ping 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=112 time=19.5 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=112 time=19.0 ms

--- 8.8.8.8 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1002ms
```

node1.tp5.b1 peut résoudre des noms de domaine publics (comme ynov.com)

```
[alexandre@node1 network-scripts]$ ping hentaihaven.com
PING hentaihaven.com (188.114.96.2) 56(84) bytes of data.
64 bytes from 188.114.96.2 (188.114.96.2): icmp_seq=1 ttl=55 time=21.2 ms
64 bytes from 188.114.96.2 (188.114.96.2): icmp_seq=2 ttl=55 time=22.3 ms
64 bytes from 188.114.96.2 (188.114.96.2): icmp_seq=3 ttl=55 time=22.9 ms

--- hentaihaven.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
```

## Serveur WEB

## Installez le paquet nginx

```
[alexandre@servweb ~]$ sudo dnf install nginx -y
```

 Créer le site web

 ```
 [alexandre@servweb var]$ sudo mkdir www
[alexandre@servweb www]$ sudo mkdir site_web_nul
[alexandre@servweb site_web_nul]$ sudo nano index.html

# Dans le fichier
<h1>MAAAAAaaaaaaAAAAAaaaAAAAx</h1>
<h2>UWU</h2>
 ```

 Donner les bonnes permissions

```
[alexandre@servweb ~]$ sudo chown -R nginx:nginx /var/www/site_web_nul
```

Créer un fichier de configuration NGINX pour notre site web

```
[alexandre@servweb ~]$ sudo nano /etc/nginx/conf.d/site_web_nul.conf
```

 Démarrer le serveur web !

 ```
 [alexandre@servweb conf.d]$ sudo systemctl start nginx
[alexandre@servweb conf.d]$ sudo systemctl status nginx
● nginx.service - The nginx HTTP and reverse proxy server
     Loaded: loaded (/usr/lib/systemd/system/nginx.service; disabled; prese)
     Active: active (running) since Fri 2023-11-10 16:54:32 CET; 11s ago
 ```

  Ouvrir le port firewall

```
[alexandre@servweb ~]$ sudo firewall-cmd --add-port=80/tcp --permanent
success
[alexandre@servweb ~]$ sudo firewall-cmd --reload
success
```

Visitez le serveur web !

```
[alexandre@node1 ~]$ curl http://10.5.1.12
<h1>AAAAAAAAAAAAAAAAAH</h1>
<h2>UWU</h2>
```

Visualiser le port en écoute
```
[diane@servweb ~]$ ss -atnl
State             Recv-Q            Send-Q                       Local Address:Port                         Peer Address:Port            Process
LISTEN            0                 511                                0.0.0.0:80                                0.0.0.0:*
```