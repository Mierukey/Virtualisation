# I. Routage

## 🌞 Configuration de router.tp2.efrei

    [rocky@routeurtp2 ~]$ ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group defau0
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
           valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP gro0
        link/ether 0c:0a:72:00:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s3
        altname ens3
        inet 192.168.122.29/24 brd 192.168.122.255 scope global dynamic noprefixrou0
           valid_lft 3547sec preferred_lft 3547sec
        inet6 fe80::e0a:72ff:fe00:0/64 scope link
           valid_lft forever preferred_lft forever
    3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP gro0
        link/ether 0c:0a:72:00:00:01 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        altname ens4
        inet 10.2.1.254/24 brd 10.2.1.255 scope global noprefixroute eth1
           valid_lft forever preferred_lft forever
        inet6 fe80::e0a:72ff:fe00:1/64 scope link
           valid_lft forever preferred_lft forever

## 🌞 Configuration de node1.tp2.efrei

### Ping de node1.tp2.efrei vers router.tp2.efrei :

    PC1> ping 10.2.1.254

    84 bytes from 10.2.1.254 icmp_seq=1 ttl=64 time=12.396 ms
    84 bytes from 10.2.1.254 icmp_seq=2 ttl=64 time=2.694 ms
    84 bytes from 10.2.1.254 icmp_seq=3 ttl=64 time=2.806 ms^C
    
### Ping de node1.tp2.efrei vers 8.8.8.8 :

    PC1> ping 8.8.8.8

    84 bytes from 8.8.8.8 icmp_seq=1 ttl=112 time=50.469 ms
    84 bytes from 8.8.8.8 icmp_seq=2 ttl=112 time=77.570 ms
    ^C

#

    PC1> trace 8.8.8.8
    trace to 8.8.8.8, 8 hops max, press Ctrl+C to stop
     1   10.2.1.254   4.231 ms  1.489 ms  0.820 ms
     2   192.168.122.1   3.116 ms  1.444 ms  1.330 ms
     3   10.0.3.2   2.491 ms  3.252 ms  1.884 ms
     4     *  *  *
     5     *  *  *
     6     *  *  *
     7     *  *  *
     8     *  *  *

## 🌞 Afficher la CAM Table du switch

    Switch#show mac address-table
          Mac Address Table
    -------------------------------------------

    Vlan    Mac Address       Type        Ports
    ----    -----------       --------    -----
       1    0050.7966.6800    DYNAMIC     Et0/1
       1    0c0a.7200.0001    DYNAMIC     Et0/0
    Total Mac Addresses for this criterion: 2

# II. Serveur DHCP

## 🌞 Installation et configuration du serveur DHCP sur dhcp.tp2.efrei

    [rocky@dhcptp2 ~]$ ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group defau0
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host
           valid_lft forever preferred_lft forever
    2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP gro0
        link/ether 0c:f8:d7:6a:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s3
        altname ens3
        inet 10.2.1.253/24 brd 10.2.1.255 scope global noprefixroute eth0
           valid_lft forever preferred_lft forever
        inet6 fe80::ef8:d7ff:fe6a:0/64 scope link
           valid_lft forever preferred_lft forever
#
    [rocky@dhcptp2 ~]$ ip route show
    default via 10.2.1.254 dev eth0 proto static metric 100
    10.2.1.0/24 dev eth0 proto kernel scope link src 10.2.1.253 metric 100
#
    [rocky@dhcptp2 ~]$ ping 8.8.8.8
    PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
    64 bytes from 8.8.8.8: icmp_seq=1 ttl=112 time=92.8 ms
    64 bytes from 8.8.8.8: icmp_seq=2 ttl=112 time=88.4 ms
    64 bytes from 8.8.8.8: icmp_seq=3 ttl=112 time=43.3 ms
    
    --- 8.8.8.8 ping statistics ---
    3 packets transmitted, 3 received, 0% packet loss, time 2009ms
    rtt min/avg/max/mdev = 43.254/74.819/92.780/22.390 ms
    
### Configuration du dhcp :

    [rocky@dhcptp2 ~]$ sudo dnf -y install dhcp-server
#
    [rocky@dhcptp2 ~]$ sudo vi /etc/dhcp/dhcpd.conf
    
### Contenu :

    # this DHCP server to be declared valid
    authoritative;

    # specify network address and subnetmask
    subnet 10.2.1.0 netmask 255.255.255.0 {
        # specify the range of lease IP address
        range dynamic-bootp 10.2.1.10 10.2.1.199;
        # specify broadcast address
        option broadcast-address 10.2.1.255;
        # specify gateway
        option routers 10.2.1.254;
    }

## 🌞 Test du DHCP sur node1.tp2.efrei

    PC1> dhcp
    DDORA IP 10.2.1.10/24 GW 10.2.1.254

## 🌟 BONUS

    # this DHCP server to be declared valid
    authoritative;

    # specify network address and subnetmask
    subnet 10.2.1.0 netmask 255.255.255.0 {
        # specify the range of lease IP address
        range dynamic-bootp 10.2.1.10 10.2.1.199;
        # specify broadcast address
        option broadcast-address 10.2.1.255;
        # specify gateway
        option routers 10.2.1.254;
        option domain-name-servers 1.1.1.1;
    }

## 🌞 Wireshark it !

### La capture est sous le nom : dchptp2.pcapng


