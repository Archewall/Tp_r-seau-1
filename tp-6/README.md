# TP6 : Un LAN maîtrisé meo ?

## Serveur DNS

### SETUP

🌞 Dans le rendu, je veux

- un cat des 3 fichiers de conf (fichier principal + deux fichiers de zone)

- un systemctl status named qui prouve que le service tourne bien

- une commande ss qui prouve que le service écoute bien sur un port

🌞 Ouvrez le bon port dans le firewall

- grâce à la commande ss vous devrez avoir repéré sur quel port tourne le service

- ouvrez ce port dans le firewall de la machine dns.tp6.b1 (voir le mémo réseau Rocky)

### TEST

🌞 Sur la machine john.tp6.b1

🌞 Sur votre PC

🦈 Capture tp6_dns.pcapng

## Serveur DHCP

🌞 Installer un serveur DHCP

🌞 Test avec john.tp6.b1

prouvez-que

- vous avez une IP dans la bonne range
- vous avez votre routeur configuré automatiquement comme passerelle
- vous avez votre serveur DNS automatiquement configuré
- vous avez un accès internet

🌞 Requête web avec john.tp6.b1