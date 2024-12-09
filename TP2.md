# I. Routage

## 🌞 Configuration de router.tp2.efrei

    ip a
#
    root@debian:/home/debian# ip a
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
           valid_lft forever preferred_lft forever
        inet6 ::1/128 scope host noprefixroute
           valid_lft forever preferred_lft forever
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:a0:05:17:00:00 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
        inet 10.2.1.254/24 brd 10.2.1.255 scope global ens4
           valid_lft forever preferred_lft forever
        inet6 fe80::ea0:5ff:fe17:0/64 scope link
           valid_lft forever preferred_lft forever
    3: ens5: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
        link/ether 0c:a0:05:17:00:01 brd ff:ff:ff:ff:ff:ff
        altname enp0s5
        inet 192.168.122.53/24 brd 192.168.122.255 scope global dynamic ens5
           valid_lft 3520sec preferred_lft 3520sec
        inet6 fe80::ea0:5ff:fe17:1/64 scope link
           valid_lft forever preferred_lft forever
