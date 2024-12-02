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
