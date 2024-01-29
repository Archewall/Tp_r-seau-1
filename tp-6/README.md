# TP6 : Un LAN maîtrisé meo ?

## Serveur DNS

### SETUP

🌞 Dans le rendu, je veux

- un cat des 3 fichiers de conf (fichier principal + deux fichiers de zone)

```powershell
[alexandre@dns ~]$ sudo cat /etc/named.conf
[sudo] password for alexandre:

options {
        listen-on port 53 { 127.0.0.1; any; };
        listen-on-v6 port 53 { ::1; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        allow-query     { localhost; any; };
        allow-query-cache { localhost; any; };

        recursion yes;

        dnssec-validation yes;

        managed-keys-directory "/var/named/dynamic";
        geoip-directory "/usr/share/GeoIP";

        pid-file "/run/named/named.pid";
        session-keyfile "/run/named/session.key";

        /* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
        include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "." IN {
        type hint;
        file "named.ca";
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";

# référence vers notre fichier de zone
zone "tp6.b1" IN {
     type master;
     file "tp6.b1.db";
     allow-update { none; };
     allow-query {any; };
};
# référence vers notre fichier de zone inverse
zone "1.4.10.in-addr.arpa" IN {
     type master;
     file "tp6.b1.rev";
     allow-update { none; };
     allow-query { any; };
};
```


```powershell
[alexandre@dns ~]$ sudo cat /var/named/tp6.b1.db
$TTL 86400
@ IN SOA dns.tp6.b1. admin.tp6.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)

; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns.tp6.b1.

; Enregistrements DNS pour faire correspondre des noms à des IPs
dns       IN A 10.6.1.101
john      IN A 10.6.1.11
```

```powershell
[alexandre@dns ~]$ sudo cat /var/named/tp6.b1.rev
$TTL 86400
@ IN SOA dns.tp6.b1. admin.tp6.b1. (
    2019061800 ;Serial
    3600 ;Refresh
    1800 ;Retry
    604800 ;Expire
    86400 ;Minimum TTL
)
; Infos sur le serveur DNS lui même (NS = NameServer)
@ IN NS dns.tp6.b1.

; Reverse lookup
101 IN PTR dns.tp6.b1.
11 IN PTR john.tp6.b1.
```

- un systemctl status named qui prouve que le service tourne bien

```powershell
[alexandre@dns ~]$ sudo systemctl status named
● named.service - Berkeley Internet Name Domain (DNS)
     Loaded: loaded (/usr/lib/systemd/system/named.service; enabled; preset>
     Active: active (running) since Fri 2023-11-17 15:31:07 CET; 31min ago
```

- une commande ss qui prouve que le service écoute bien sur un port

```powershell
[alexandre@dns ~]$ ss -l -n -t
State   Recv-Q  Send-Q   Local Address:Port     Peer Address:Port  Process
LISTEN  0       10           127.0.0.1:53            0.0.0.0:*
LISTEN  0       10          10.6.1.101:53            0.0.0.0:*
LISTEN  0       10               [::1]:53               [::]:*
```

 Ouvrez le bon port dans le firewall

- ouvrez le port 53 sur le firewall de dns.tp6.b1

```powershell
[alexandre@dns ~]$ sudo firewall-cmd --add-port=53/udp --permanent
success
[alexandre@dns ~]$ sudo firewall-cmd --reload
success
```

### TEST

Sur la machine john.tp6.b1

- Configuration

```powershell
[alexandre@john ~]$ sudo cat /etc/resolv.conf
[sudo] password for diane:
nameserver 10.6.1.101
```

- Tests

```powershell
[alexandre@john ~]$ ping google.com
PING google.com (142.250.201.174) 56(84) bytes of data.
64 bytes from par21s23-in-f14.1e100.net (142.250.201.174): icmp_seq=1 ttl=115 time=12.5 ms
64 bytes from par21s23-in-f14.1e100.net (142.250.201.174): icmp_seq=2 ttl=115 time=12.8 ms
64 bytes from par21s23-in-f14.1e100.net (142.250.201.174): icmp_seq=3 ttl=115 time=13.4 ms

--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2003ms
rtt min/avg/max/mdev = 12.518/12.917/13.425/0.378 ms
[alexandre@john ~]$ ping dns.tp6.b1
PING dns.tp6.b1 (10.6.1.101) 56(84) bytes of data.
64 bytes from 10.6.1.101 (10.6.1.101): icmp_seq=1 ttl=64 time=0.140 ms
64 bytes from 10.6.1.101 (10.6.1.101): icmp_seq=2 ttl=64 time=0.326 ms
64 bytes from 10.6.1.101 (10.6.1.101): icmp_seq=3 ttl=64 time=0.330 ms

--- dns.tp6.b1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2028ms
rtt min/avg/max/mdev = 0.140/0.265/0.330/0.088 ms
```

Sur votre PC

```powershell
PS C:\Windows\system32> Set-DNSClientServerAddress "Wi-Fi" -ServerAddresses ("10.6.1.101")
```
```powershell
PS C:\Users\Makhov> nslookup john.tp6.b1
Serveur :   UnKnown
Address:  10.6.1.101

Nom :    john.tp6.b1
Address:  10.6.1.11
```