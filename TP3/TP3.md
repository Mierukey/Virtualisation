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

    
