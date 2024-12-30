# I - Setup réseau initial

## 🌞 ping d'un client du  LAN1 vers un client du LAN 2

    pc1.tp3.b2> ping 10.3.2.1

    84 bytes from 10.3.2.1 icmp_seq=1 ttl=62 time=52.180 ms
    84 bytes from 10.3.2.1 icmp_seq=2 ttl=62 time=40.191 ms
    84 bytes from 10.3.2.1 icmp_seq=3 ttl=62 time=37.349 ms
    84 bytes from 10.3.2.1 icmp_seq=4 ttl=62 time=36.120 ms
    84 bytes from 10.3.2.1 icmp_seq=5 ttl=62 time=42.943 ms

## 🌞 Capture Wireshark ping_partie1

La capture est sous le nom : ping_partie1.pcap

## 🌞 Afficher les adresses MAC des routeurs

### R1

    R1#show arp
    Protocol  Address          Age (min)  Hardware Addr   Type   Interface
    Internet  10.3.12.1               -   c401.05c3.0001  ARPA   FastEthernet0/1
    Internet  10.3.12.2              17   c402.05e1.0001  ARPA   FastEthernet0/1
    Internet  10.3.1.1                8   0050.7966.6800  ARPA   FastEthernet1/0
    Internet  192.168.122.1          11   5254.0023.04c2  ARPA   FastEthernet0/0
    Internet  192.168.122.190         -   c401.05c3.0000  ARPA   FastEthernet0/0
    Internet  10.3.1.254              -   c401.05c3.0010  ARPA   FastEthernet1/0

### R2

    R2#show arp
    Protocol  Address          Age (min)  Hardware Addr   Type   Interface
    Internet  10.3.12.1              17   c401.05c3.0001  ARPA   FastEthernet0/1
    Internet  10.3.12.2               -   c402.05e1.0001  ARPA   FastEthernet0/1
    Internet  10.3.2.1                8   0050.7966.6802  ARPA   FastEthernet1/0
    Internet  10.3.2.254              -   c402.05e1.0010  ARPA   FastEthernet1/0

## 🌞 Prouvez que vous avez déjà un accès internet sur r1

    R1#ping 1.1.1.1

    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 1.1.1.1, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 88/116/184 ms

## 🌞 Accès internet LAN1

    pc1.tp3.b2> ping 8.8.8.8

    84 bytes from 8.8.8.8 icmp_seq=1 ttl=111 time=65.656 ms
    84 bytes from 8.8.8.8 icmp_seq=2 ttl=111 time=49.001 ms

## 🌞 Accès internet LAN2

    pc3.tp3.b2> ping 8.8.8.8

    84 bytes from 8.8.8.8 icmp_seq=1 ttl=110 time=87.788 ms
    84 bytes from 8.8.8.8 icmp_seq=2 ttl=110 time=54.010 ms
    84 bytes from 8.8.8.8 icmp_seq=3 ttl=110 time=56.427 ms

# II - Router-on-a-stick

## 🌞 Tests de ping

### PC1

    PC1> show ip

    NAME        : PC1[1]
    IP/MASK     : 10.3.1.1/24
    GATEWAY     : 10.3.1.254
    DNS         :
    MAC         : 00:50:79:66:68:00
    LPORT       : 20018
    RHOST:PORT  : 127.0.0.1:20019
    MTU         : 1500

    PC1> ping 10.3.2.1

    84 bytes from 10.3.2.1 icmp_seq=1 ttl=63 time=29.754 ms
    84 bytes from 10.3.2.1 icmp_seq=2 ttl=63 time=22.250 ms
    84 bytes from 10.3.2.1 icmp_seq=3 ttl=63 time=13.515 ms

## 🌞 Tests de ping

### R1

    R1#ping 8.8.8.8

    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 8.8.8.8, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 88/108/152 ms

### PC2

    PC2> ping 1.1.1.1

    84 bytes from 1.1.1.1 icmp_seq=1 ttl=52 time=121.057 ms
    84 bytes from 1.1.1.1 icmp_seq=2 ttl=52 time=82.458 ms
    84 bytes from 1.1.1.1 icmp_seq=3 ttl=52 time=154.460 ms

### PC3

    PC3> ping 1.1.1.1

    84 bytes from 1.1.1.1 icmp_seq=1 ttl=52 time=85.677 ms
    84 bytes from 1.1.1.1 icmp_seq=2 ttl=52 time=79.487 ms
    84 bytes from 1.1.1.1 icmp_seq=3 ttl=52 time=145.795 ms

# III - Services dans le LAN

## 🌞 Prouvez avec un VPCS

    PC4> show ip

    NAME        : PC4[1]
    IP/MASK     : 10.3.2.10/24
    GATEWAY     : 10.3.2.254
    DNS         : 10.3.3.1  1.1.1.1
    DHCP SERVER : 10.3.2.253
    DHCP LEASE  : 41342, 41345/20672/36176
    MAC         : 00:50:79:66:68:03
    LPORT       : 20026
    RHOST:PORT  : 127.0.0.1:20027
    MTU         : 1500
    
    PC4> ping efrei.fr
    efrei.fr resolved to 51.255.68.208

    84 bytes from 51.255.68.208 icmp_seq=1 ttl=52 time=40.828 ms
    84 bytes from 51.255.68.208 icmp_seq=2 ttl=52 time=41.031 ms
    84 bytes from 51.255.68.208 icmp_seq=3 ttl=52 time=39.607 ms
    84 bytes from 51.255.68.208 icmp_seq=4 ttl=52 time=37.359 ms
    ^C
    PC4> ping dns.tp3.b2
    dns.tp3.b2 resolved to 10.3.3.1

    84 bytes from 10.3.3.1 icmp_seq=1 ttl=63 time=19.730 ms
    84 bytes from 10.3.3.1 icmp_seq=2 ttl=63 time=16.407 ms
    84 bytes from 10.3.3.1 icmp_seq=3 ttl=63 time=15.260 ms

