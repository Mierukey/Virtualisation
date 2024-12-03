# I - Most simplest LAN

## 🌞 Déterminer l'adresse MAC de vos deux machines
       
## node1.tp1.efrei : 
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
        link/ether 0c:da:49:4e:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
 
## node2.tp1.efrei :
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
        link/ether 0c:50:c9:16:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
 
## 🌞 Définir une IP statique sur les deux machines
 
 
    root@debian:/home/debian# nano /etc/network/interfaces

## node1.tp1.efrei :
    #and how to activate them. For more information, see interfaces(5).

    source /etc/network/interfaces.d/*

    #The loopback network interface
    auto lo
    iface lo inet loopback

    #DHCP config for ens4
    #auto ens4
    #iface ens4 inet dhcp
    
     Static config for ens4
    auto ens4
    iface ens4 inet static
        address 10.1.1.1
        netmask 255.255.255.0
        gateway 10.1.1.100
        dns-nameservers 10.1.1.100

## node2.tp1.efrei :
    # and how to activate them. For more information, see interfaces(5).

    source /etc/network/interfaces.d/*

    # The loopback network interface
    auto lo
    iface lo inet loopback

    # DHCP config for ens4
    #auto ens4
    #iface ens4 inet dhcp

     Static config for ens4
    auto ens4
    iface ens4 inet static
            address 10.1.1.2
            netmask 255.255.255.0
            gateway 10.1.1.100
            dns-nameservers 10.1.1.100
#
    root@debian:/home/debian# systemctl restart networking.service ; sudo ifup ens4

## node1.tp1.efrei :
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:da:49:4e:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.1/24 brd 10.1.1.255 scope global ens4
           valid_lft forever preferred_lft forever
        inet6 fe80::eda:49ff:fe4e:0/64 scope link
           valid_lft forever preferred_lft forever

## node2.tp1.efrei :
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:50:c9:16:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.2/24 brd 10.1.1.255 scope global ens4
           valid_lft forever preferred_lft forever
        inet6 fe80::e50:c9ff:fe16:0/64 scope link
           valid_lft forever preferred_lft forever

## 🌞 Effectuer un ping d'une machine à l'autre

## node1.tp1.efrei :
    root@debian:/home/debian# ping 10.1.1.2
    PING 10.1.1.2 (10.1.1.2) 56(84) bytes of data.
    64 bytes from 10.1.1.2: icmp_seq=1 ttl=64 time=44.1 ms
    64 bytes from 10.1.1.2: icmp_seq=2 ttl=64 time=1.31 ms
    64 bytes from 10.1.1.2: icmp_seq=3 ttl=64 time=23.0 ms
    ^C
    --- 10.1.1.2 ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 2005ms
    rtt min/avg/max/mdev = 1.305/22.804/44.082/17.464 ms
## node2.tp1.efrei :
    root@debian:/home/debian# ping 10.1.1.1
    PING 10.1.1.1 (10.1.1.1) 56(84) bytes of data.
    64 bytes from 10.1.1.1: icmp_seq=1 ttl=64 time=7.83 ms
    64 bytes from 10.1.1.1: icmp_seq=2 ttl=64 time=1.56 ms
    64 bytes from 10.1.1.1: icmp_seq=3 ttl=64 time=2.47 ms
    ^C
    --- 10.1.1.1 ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 2003ms
    rtt min/avg/max/mdev = 1.562/3.953/7.826/2.763 ms

## 🌞 Wireshark !
Protocole utilisé pour le ping : ICMP
 
Capture dans le dépôt sous le nom : ping.pcapng

## 🌞 ARP
    root@debian:/home/debian# ip neigh
    10.1.1.2 dev ens4 lladdr 0c:50:c9:16:00:00 STALE
    10.1.1.100 dev ens4 INCOMPLETE

# II - Ajoutons un switch
 
## 🌞 Déterminer l'adresse MAC de vos trois machines

## node1.tp1.efrei
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:da:49:4e:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.1/24 brd 10.1.1.255 scope global ens4
           valid_lft forever preferred_lft forever
        inet6 fe80::eda:49ff:fe4e:0/64 scope link
           valid_lft forever preferred_lft forever

## node2.tp1.efrei
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:50:c9:16:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.2/24 brd 10.1.1.255 scope global ens4
           valid_lft forever preferred_lft forever
        inet6 fe80::e50:c9ff:fe16:0/64 scope link
           valid_lft forever preferred_lft forever

## node3.tp1.efrei
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:1b:a0:0a:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.3/24 brd 10.1.1.255 scope global ens4
           valid_lft forever preferred_lft forever
        inet6 fe80::e1b:a0ff:fe0a:0/64 scope link
           valid_lft forever preferred_lft forever

## 🌞 Définir une IP statique sur les trois machines

    root@debian:/home/debian# nano /etc/network/interfaces

## node1.tp1.efrei :
    #and how to activate them. For more information, see interfaces(5).

    source /etc/network/interfaces.d/*

    #The loopback network interface
    auto lo
    iface lo inet loopback

    #DHCP config for ens4
    #auto ens4
    #iface ens4 inet dhcp
    
     Static config for ens4
    auto ens4
    iface ens4 inet static
        address 10.1.1.1
        netmask 255.255.255.0
        gateway 10.1.1.100
        dns-nameservers 10.1.1.100

## node2.tp1.efrei :
    # and how to activate them. For more information, see interfaces(5).

    source /etc/network/interfaces.d/*

    # The loopback network interface
    auto lo
    iface lo inet loopback

    # DHCP config for ens4
    #auto ens4
    #iface ens4 inet dhcp

     Static config for ens4
    auto ens4
    iface ens4 inet static
            address 10.1.1.2
            netmask 255.255.255.0
            gateway 10.1.1.100
            dns-nameservers 10.1.1.100
## node3.tp1.efrei :
    # and how to activate them. For more information, see interfaces(5).

    source /etc/network/interfaces.d/*

    # The loopback network interface
    auto lo
    iface lo inet loopback

    # DHCP config for ens4
    #auto ens4
    #iface ens4 inet dhcp

     Static config for ens4
    auto ens4
    iface ens4 inet static
            address 10.1.1.3
            netmask 255.255.255.0
            gateway 10.1.1.100
            dns-nameservers 10.1.1.100
#
    root@debian:/home/debian# systemctl restart networking.service ; sudo ifup ens4

## node1.tp1.efrei
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:da:49:4e:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.1/24 brd 10.1.1.255 scope global ens4
           valid_lft forever preferred_lft forever
        inet6 fe80::eda:49ff:fe4e:0/64 scope link
           valid_lft forever preferred_lft forever

## node2.tp1.efrei
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:50:c9:16:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.2/24 brd 10.1.1.255 scope global ens4
           valid_lft forever preferred_lft forever
        inet6 fe80::e50:c9ff:fe16:0/64 scope link
           valid_lft forever preferred_lft forever

## node3.tp1.efrei
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:1b:a0:0a:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.3/24 brd 10.1.1.255 scope global ens4
           valid_lft forever preferred_lft forever
        inet6 fe80::e1b:a0ff:fe0a:0/64 scope link
           valid_lft forever preferred_lft forever

## 🌞 Effectuer des ping d'une machine à l'autre

## Ping de node1.tp1.efrei à node2.tp1.efrei
    root@debian:/home/debian# ping 10.1.1.2
    PING 10.1.1.2 (10.1.1.2) 56(84) bytes of data.
    64 bytes from 10.1.1.2: icmp_seq=1 ttl=64 time=11.1 ms
    64 bytes from 10.1.1.2: icmp_seq=2 ttl=64 time=2.34 ms
    64 bytes from 10.1.1.2: icmp_seq=3 ttl=64 time=2.19 ms
    ^C
    --- 10.1.1.2 ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 2006ms
    rtt min/avg/max/mdev = 2.191/5.195/11.055/4.144 ms

## Ping de node2.tp1.efrei à node3.tp1.efrei
    root@debian:/home/debian# ping 10.1.1.3
    PING 10.1.1.3 (10.1.1.3) 56(84) bytes of data.
    64 bytes from 10.1.1.3: icmp_seq=1 ttl=64 time=5.37 ms
    64 bytes from 10.1.1.3: icmp_seq=2 ttl=64 time=2.10 ms
    64 bytes from 10.1.1.3: icmp_seq=3 ttl=64 time=1.32 ms
    ^C
    --- 10.1.1.3 ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 2006ms
    rtt min/avg/max/mdev = 1.324/2.934/5.374/1.754 ms

## Ping de node1.tp1.efrei à node3.tp1.efrei
    root@debian:/home/debian# ping 10.1.1.3
    PING 10.1.1.3 (10.1.1.3) 56(84) bytes of data.
    64 bytes from 10.1.1.3: icmp_seq=1 ttl=64 time=21.4 ms
    64 bytes from 10.1.1.3: icmp_seq=2 ttl=64 time=1.89 ms
    64 bytes from 10.1.1.3: icmp_seq=3 ttl=64 time=1.73 ms
    ^C
    --- 10.1.1.3 ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 2005ms
    rtt min/avg/max/mdev = 1.731/8.344/21.417/9.243 ms

# III - Serveur DHCP

## 🌞 Donner un accès Internet à la machine dhcp.tp1.efrei

    root@debian:/home/debian# ping 8.8.8.8
    PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
    64 bytes from 8.8.8.8: icmp_seq=1 ttl=113 time=165 ms
    64 bytes from 8.8.8.8: icmp_seq=2 ttl=113 time=38.8 ms
    64 bytes from 8.8.8.8: icmp_seq=3 ttl=113 time=81.4 ms
    ^C
    --- 8.8.8.8 ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 2005ms
    rtt min/avg/max/mdev = 38.830/95.219/165.408/52.588 ms

## 🌞 Installer et configurer un serveur DHCP

    root@debian:/home/debian# apt update
#
    root@debian:/home/debian# apt install isc-dhcp-server
#
    root@debian:/home/debian# nano /etc/dhcp/dhcpd.conf
## Contenu :
    subnet 10.1.1.0 netmask 255.255.255.0 {
      range 10.1.1.10 10.1.1.50;
      option routers 10.1.1.100;
      option domain-name-servers 8.8.8.8, 8.8.4.4;
      default-lease-time 600;
      max-lease-time 7200;
    }
#
    root@debian:/home/debian# nano /etc/default/isc-dhcp-server
## Contenu :
    INTERFACESv4="ens4"
    INTERFACESv6=""
#
    root@debian:/home/debian# systemctl start isc-dhcp-server
#
    root@debian:/home/debian# systemctl enable isc-dhcp-server

## 🌞 Récupérer une IP automatiquement depuis les 3 nodes

## Sur chaque node :
    root@debian:/home/debian# nano /etc/network/interfaces
## Contenu :
    # This file describes the network interfaces available on your system
    # and how to activate them. For more information, see interfaces(5).

    source /etc/network/interfaces.d/*

    # The loopback network interface
    auto lo
    iface lo inet loopback

    # DHCP config for ens4
    auto ens4
    iface ens4 inet dhcp

    # Static config for ens4
    #auto ens4
    #iface ens4 inet static
    #       address 192.168.1.100
    #       netmask 255.255.255.0
    #       gateway 192.168.1.1
    #       dns-nameservers 192.168.1.1
#
    root@debian:/home/debian# systemctl restart networking.service ; sudo ifup ens4
## node1.tp1.efrei :
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:27:16:58:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.10/24 brd 10.1.1.255 scope global dynamic ens4
           valid_lft 349sec preferred_lft 349sec
        inet6 fe80::e27:16ff:fe58:0/64 scope link
           valid_lft forever preferred_lft forever
## node2.tp1.efrei :
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:63:17:a9:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.11/24 brd 10.1.1.255 scope global dynamic ens4
           valid_lft 587sec preferred_lft 587sec
        inet6 fe80::e63:17ff:fea9:0/64 scope link
           valid_lft forever preferred_lft forever
## node3.tp1.efrei
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:2e:fa:f3:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.12/24 brd 10.1.1.255 scope global dynamic ens4
           valid_lft 598sec preferred_lft 598sec
        inet6 fe80::e2e:faff:fef3:0/64 scope link
           valid_lft forever preferred_lft forever

## 🌞 Wireshark !

## La capture est sous le nom : dhcp.pcapng

## 🌞 Configurez dnsmasq

    root@debian:/home/debian# apt install dnsmasq
#
    root@debian:/home/debian# nano /etc/dnsmasq.conf
## Contenu :
    dhcp-authoritative
    # Plage DHCP
    dhcp-range=10.1.1.210,10.1.1.250,12h
    # Netmask
    dhcp-option=1,255.255.255.0
    # Route
    dhcp-option=3,192.168.1.1
#
    root@debian:/home/debian# /etc/init.d/dnsmasq restart
## 🌞 Test !
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:27:16:58:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.219/24 brd 10.1.1.255 scope global dynamic ens4
           valid_lft 43133sec preferred_lft 43133sec
        inet6 fe80::e27:16ff:fe58:0/64 scope link
           valid_lft forever preferred_lft forever

## 🌞 Now race !

## Essai 1 :
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:7f:fd:da:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.15/24 brd 10.1.1.255 scope global dynamic ens4
           valid_lft 594sec preferred_lft 594sec
        inet6 fe80::e7f:fdff:feda:0/64 scope link
           valid_lft forever preferred_lft forever

## Essai 2 :
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default     qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group     default qlen 1000
        link/ether 0c:15:57:ed:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.16/24 brd 10.1.1.255 scope global dynamic ens4
           valid_lft 597sec preferred_lft 597sec
        inet6 fe80::e15:57ff:feed:0/64 scope link
           valid_lft forever preferred_lft forever

## Essai 3 :
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default     qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group     default qlen 1000
        link/ether 0c:07:b3:92:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.1.1.17/24 brd 10.1.1.255 scope global dynamic ens4
           valid_lft 546sec preferred_lft 546sec
        inet6 fe80::e07:b3ff:fe92:0/64 scope link
           valid_lft forever preferred_lft forever

## 🌞 Wireshark !

## Capture sous le nom : race.pcapng

# BONUS : DHCP starvation
    root@debian:/home/debian# dhcpstarv -i ens4
    [   76.025833] device ens4 entered promiscuous mode
    11:28:14 12/03/24: no renewal time option in DHCPOFFER
    11:28:14 12/03/24: got address 10.1.1.19 for 00:16:36:51:e9:0b from 10.1.1.253
    11:28:15 12/03/24: no renewal time option in DHCPOFFER
    11:28:15 12/03/24: got address 10.1.1.20 for 00:16:36:d6:34:f3 from 10.1.1.253
    11:28:16 12/03/24: no renewal time option in DHCPOFFER
    11:28:16 12/03/24: got address 10.1.1.21 for 00:16:36:4a:14:65 from 10.1.1.253
    11:28:17 12/03/24: no renewal time option in DHCPOFFER
    11:28:17 12/03/24: got address 10.1.1.22 for 00:16:36:30:36:01 from 10.1.1.253
    11:28:18 12/03/24: no renewal time option in DHCPOFFER
    11:28:18 12/03/24: got address 10.1.1.23 for 00:16:36:b3:5a:56 from 10.1.1.253
    11:28:19 12/03/24: no renewal time option in DHCPOFFER
    11:28:19 12/03/24: got address 10.1.1.24 for 00:16:36:21:e4:ff from 10.1.1.253
    11:28:20 12/03/24: no renewal time option in DHCPOFFER
    11:28:20 12/03/24: got address 10.1.1.25 for 00:16:36:f0:3d:36 from 10.1.1.253
    11:28:21 12/03/24: no renewal time option in DHCPOFFER
    11:28:21 12/03/24: got address 10.1.1.26 for 00:16:36:fc:67:98 from 10.1.1.253
    11:28:22 12/03/24: no renewal time option in DHCPOFFER
    11:28:22 12/03/24: got address 10.1.1.27 for 00:16:36:a6:83:53 from 10.1.1.253
    11:28:23 12/03/24: no renewal time option in DHCPOFFER
    11:28:23 12/03/24: got address 10.1.1.28 for 00:16:36:55:63:12 from 10.1.1.253
    11:28:24 12/03/24: no renewal time option in DHCPOFFER
    11:28:24 12/03/24: got address 10.1.1.29 for 00:16:36:de:07:fd from 10.1.1.253
    11:28:25 12/03/24: no renewal time option in DHCPOFFER
    11:28:25 12/03/24: got address 10.1.1.30 for 00:16:36:17:17:28 from 10.1.1.253
    11:28:26 12/03/24: no renewal time option in DHCPOFFER
    11:28:26 12/03/24: got address 10.1.1.31 for 00:16:36:10:09:81 from 10.1.1.253
    11:28:27 12/03/24: no renewal time option in DHCPOFFER
    11:28:27 12/03/24: got address 10.1.1.32 for 00:16:36:72:40:79 from 10.1.1.253
    11:28:28 12/03/24: no renewal time option in DHCPOFFER
    11:28:28 12/03/24: got address 10.1.1.33 for 00:16:36:b2:d0:11 from 10.1.1.253
    11:28:29 12/03/24: no renewal time option in DHCPOFFER
    11:28:29 12/03/24: got address 10.1.1.34 for 00:16:36:96:84:d6 from 10.1.1.253
    11:28:30 12/03/24: no renewal time option in DHCPOFFER
    11:28:30 12/03/24: got address 10.1.1.35 for 00:16:36:6a:bd:ae from 10.1.1.253
    11:28:31 12/03/24: no renewal time option in DHCPOFFER
    11:28:31 12/03/24: got address 10.1.1.36 for 00:16:36:2a:75:a8 from 10.1.1.253
    11:28:32 12/03/24: no renewal time option in DHCPOFFER
    11:28:32 12/03/24: got address 10.1.1.37 for 00:16:36:e6:89:04 from 10.1.1.253
    11:28:33 12/03/24: no renewal time option in DHCPOFFER
    11:28:33 12/03/24: got address 10.1.1.38 for 00:16:36:2f:a1:49 from 10.1.1.253
    11:28:34 12/03/24: no renewal time option in DHCPOFFER
    11:28:34 12/03/24: got address 10.1.1.39 for 00:16:36:c2:61:59 from 10.1.1.253
    11:28:35 12/03/24: no renewal time option in DHCPOFFER
    11:28:35 12/03/24: got address 10.1.1.40 for 00:16:36:90:99:10 from 10.1.1.253
    11:28:36 12/03/24: no renewal time option in DHCPOFFER
    11:28:36 12/03/24: got address 10.1.1.41 for 00:16:36:aa:a3:45 from 10.1.1.253
    11:28:38 12/03/24: no renewal time option in DHCPOFFER
    11:28:38 12/03/24: got address 10.1.1.42 for 00:16:36:00:93:e7 from 10.1.1.253
    11:28:39 12/03/24: no renewal time option in DHCPOFFER
    11:28:39 12/03/24: got address 10.1.1.43 for 00:16:36:02:91:cd from 10.1.1.253
    11:28:40 12/03/24: no renewal time option in DHCPOFFER
    11:28:40 12/03/24: got address 10.1.1.44 for 00:16:36:ff:a1:0c from 10.1.1.253
    11:28:41 12/03/24: no renewal time option in DHCPOFFER
    11:28:41 12/03/24: got address 10.1.1.45 for 00:16:36:81:35:f9 from 10.1.1.253
    11:28:42 12/03/24: no renewal time option in DHCPOFFER
    11:28:42 12/03/24: got address 10.1.1.46 for 00:16:36:f2:fd:cb from 10.1.1.253
    11:28:43 12/03/24: no renewal time option in DHCPOFFER
    11:28:43 12/03/24: got address 10.1.1.47 for 00:16:36:f3:33:07 from 10.1.1.253
    11:28:44 12/03/24: no renewal time option in DHCPOFFER
    11:28:44 12/03/24: got address 10.1.1.48 for 00:16:36:5b:99:6b from 10.1.1.253
    11:28:45 12/03/24: no renewal time option in DHCPOFFER
    11:28:45 12/03/24: got address 10.1.1.49 for 00:16:36:18:14:a0 from 10.1.1.253
    11:28:46 12/03/24: no renewal time option in DHCPOFFER
    11:28:46 12/03/24: got address 10.1.1.50 for 00:16:36:aa:be:36 from 10.1.1.253
    11:28:47 12/03/24: no renewal time option in DHCPOFFER
    11:28:47 12/03/24: got address 10.1.1.14 for 00:16:36:5f:d7:0b from 10.1.1.253
    11:28:48 12/03/24: no renewal time option in DHCPOFFER
    11:28:48 12/03/24: got address 10.1.1.10 for 00:16:36:37:a3:62 from 10.1.1.253
    11:28:49 12/03/24: no renewal time option in DHCPOFFER
    11:28:49 12/03/24: got address 10.1.1.13 for 00:16:36:2e:df:88 from 10.1.1.253
    11:28:50 12/03/24: no renewal time option in DHCPOFFER
    11:28:50 12/03/24: got address 10.1.1.11 for 00:16:36:de:45:6b from 10.1.1.253
    11:28:51 12/03/24: no renewal time option in DHCPOFFER
    11:28:51 12/03/24: got address 10.1.1.12 for 00:16:36:b7:c9:f5 from 10.1.1.253
    11:28:52 12/03/24: no renewal time option in DHCPOFFER
    11:28:52 12/03/24: got address 10.1.1.15 for 00:16:36:a2:e0:71 from 10.1.1.253
    11:28:53 12/03/24: no renewal time option in DHCPOFFER
    11:28:53 12/03/24: got address 10.1.1.16 for 00:16:36:fc:e0:24 from 10.1.1.253
    11:28:54 12/03/24: no renewal time option in DHCPOFFER
    11:28:54 12/03/24: got address 10.1.1.17 for 00:16:36:b2:94:d4 from 10.1.1.253
