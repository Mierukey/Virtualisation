# Virtualisation : TP1


Récupérer les adresses MAC :


Je les ai récupéré sur les 2 machines Debian avec la commande : ip a

Ce qui m'a donné : "0c:50:c9:16:00:00" et "0c:da:49:4e:00:00"


Mettre une IP fixe :


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
