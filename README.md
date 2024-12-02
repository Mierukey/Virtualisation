# Virtualisation : TP1

## 🌞 Déterminer l'adresse MAC de vos deux machines
       
    ip a

node1.tp1.efrei : 0c:50:c9:16:00:00
node2.tp1.efrei : 0c:da:49:4e:00:00





 
j'ai modifié le fichier /etc/network/interfaces en y mettant :

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
