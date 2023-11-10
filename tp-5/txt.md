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

connection en ssh 
```
ssh alexandre@10.5.1.11
```
ssh utilise un TCP
on peut verifier grace a wireshark
ou grace a la  commande 
```
ss -tulnp
```


### 2. routage