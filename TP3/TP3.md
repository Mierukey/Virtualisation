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

## 🌞 Prouvez avec un VPCS (DNS + DHCP)

    PC4> ip dhcp
    DORA IP 10.3.2.10/24 GW 10.3.2.254

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

## 🌞 Preuve avec un client (NGINX)

    debian@debian:/etc/network$ curl web.tp3.b2
    <!doctype html>
    <html>
      <head>
        <meta charset='utf-8'>
        <meta name='viewport' content='width=device-width, initial-scale=1'>
        <title>HTTP Server Test Page powered by: Rocky Linux</title>
        <style type="text/css">
          /*<![CDATA[*/
    
          html {
            height: 100%;
            width: 100%;
          }
            body {
      background: rgb(20,72,50);
      background: -moz-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 9                                0%)  ;
      background: -webkit-linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1                                ) 90%) ;
      background: linear-gradient(180deg, rgba(23,43,70,1) 30%, rgba(0,0,0,1) 90%);
      background-repeat: no-repeat;
      background-attachment: fixed;
      filter: progid:DXImageTransform.Microsoft.gradient(startColorstr="#3c6eb4",endColorstr="#3c95b4",GradientType=1);
            color: white;
            font-size: 0.9em;
            font-weight: 400;
            font-family: 'Montserrat', sans-serif;
            margin: 0;
            padding: 10em 6em 10em 6em;
            box-sizing: border-box;
    
          }
    

      h1 {
        text-align: center;
        margin: 0;
        padding: 0.6em 2em 0.4em;
        color: #fff;
        font-weight: bold;
        font-family: 'Montserrat', sans-serif;
        font-size: 2em;
      }
      h1 strong {
        font-weight: bolder;
        font-family: 'Montserrat', sans-serif;
      }
      h2 {
        font-size: 1.5em;
        font-weight:bold;
      }
    
      .title {
        border: 1px solid black;
        font-weight: bold;
        position: relative;
        float: right;
        width: 150px;
        text-align: center;
        padding: 10px 0 10px 0;
        margin-top: 0;
      }
    
      .description {
        padding: 45px 10px 5px 10px;
        clear: right;
        padding: 15px;
      }
    
      .section {
        padding-left: 3%;
       margin-bottom: 10px;
      }
    
      img {
    
        padding: 2px;
        margin: 2px;
      }
      a:hover img {
        padding: 2px;
        margin: 2px;
      }
    
      :link {
        color: rgb(199, 252, 77);
        text-shadow:
      }
      :visited {
        color: rgb(122, 206, 255);
      }
      a:hover {
        color: rgb(16, 44, 122);
      }
      .row {
        width: 100%;
        padding: 0 10px 0 10px;
      }
    
      footer {
        padding-top: 6em;
        margin-bottom: 6em;
        text-align: center;
        font-size: xx-small;
        overflow:hidden;
        clear: both;
      }
    
      .summary {
        font-size: 140%;
        text-align: center;
      }

      #rocky-poweredby img {
        margin-left: -10px;
      }

      #logos img {
        vertical-align: top;
      }

      /* Desktop  View Options */

      @media (min-width: 768px)  {

        body {
          padding: 10em 20% !important;
        }

        .col-md-1, .col-md-2, .col-md-3, .col-md-4, .col-md-5, .col-md-6,
        .col-md-7, .col-md-8, .col-md-9, .col-md-10, .col-md-11, .col-md-12 {
          float: left;
        }

        .col-md-1 {
          width: 8.33%;
        }
        .col-md-2 {
          width: 16.66%;
        }
        .col-md-3 {
          width: 25%;
        }
        .col-md-4 {
          width: 33%;
        }
        .col-md-5 {
          width: 41.66%;
        }
        .col-md-6 {
          border-left:3px ;
          width: 50%;


        }
        .col-md-7 {
          width: 58.33%;
        }
        .col-md-8 {
          width: 66.66%;
        }
        .col-md-9 {
          width: 74.99%;
        }
        .col-md-10 {
          width: 83.33%;
        }
        .col-md-11 {
          width: 91.66%;
         }
        .col-md-12 {
          width: 100%;
        }
      }

      /* Mobile View Options */
      @media (max-width: 767px) {
        .col-sm-1, .col-sm-2, .col-sm-3, .col-sm-4, .col-sm-5, .col-sm-6,
        .col-sm-7, .col-sm-8, .col-sm-9, .col-sm-10, .col-sm-11, .col-sm-12 {
          float: left;
        }

        .col-sm-1 {
          width: 8.33%;
        }
        .col-sm-2 {
          width: 16.66%;
        }
        .col-sm-3 {
          width: 25%;
        }
        .col-sm-4 {
          width: 33%;
        }
        .col-sm-5 {
          width: 41.66%;
        }
        .col-sm-6 {
          width: 50%;
        }
        .col-sm-7 {
          width: 58.33%;
        }
        .col-sm-8 {
          width: 66.66%;
        }
        .col-sm-9 {
          width: 74.99%;
        }
        .col-sm-10 {
          width: 83.33%;
        }
        .col-sm-11 {
          width: 91.66%;
        }
        .col-sm-12 {
          width: 100%;
        }
        h1 {
          padding: 0 !important;
        }
      }


      </style>
      </head>
      <body>
        <h1>HTTP Server <strong>Test Page</strong></h1>

        <div class='row'>

          <div class='col-sm-12 col-md-6 col-md-6 '></div>
              <p class="summary">This page is used to test the proper operation of
                an HTTP server after it has been installed on a Rocky Linux system.
                If you can read this page, it means that the software is working
                correctly.</p>
          </div>

          <div class='col-sm-12 col-md-6 col-md-6 col-md-offset-12'>


            <div class='section'>
              <h2>Just visiting?</h2>

              <p>This website you are visiting is either experiencing problems or
              could be going through maintenance.</p>

              <p>If you would like the let the administrators of this website know
              that you've seen this page instead of the page you've expected, you
              should send them an email. In general, mail sent to the name
              "webmaster" and directed to the website's domain should reach the
              appropriate person.</p>

              <p>The most common email address to send to is:
              <strong>"webmaster@example.com"</strong></p>

              <h2>Note:</h2>
              <p>The Rocky Linux distribution is a stable and reproduceable platform
              based on the sources of Red Hat Enterprise Linux (RHEL). With this in
              mind, please understand that:

            <ul>
              <li>Neither the <strong>Rocky Linux Project</strong> nor the
              <strong>Rocky Enterprise Software Foundation</strong> have anything to
              do with this website or its content.</li>
              <li>The Rocky Linux Project nor the <strong>RESF</strong> have
              "hacked" this webserver: This test page is included with the
              distribution.</li>
            </ul>
            <p>For more information about Rocky Linux, please visit the
              <a href="https://rockylinux.org/"><strong>Rocky Linux
              website</strong></a>.
            </p>
            </div>
          </div>
          <div class='col-sm-12 col-md-6 col-md-6 col-md-offset-12'>
            <div class='section'>

              <h2>I am the admin, what do I do?</h2>

            <p>You may now add content to the webroot directory for your
            software.</p>

            <p><strong>For systems using the
            <a href="https://httpd.apache.org/">Apache Webserver</strong></a>:
            You can add content to the directory <code>/var/www/html/</code>.
            Until you do so, people visiting your website will see this page. If
            you would like this page to not be shown, follow the instructions in:
            <code>/etc/httpd/conf.d/welcome.conf</code>.</p>

            <p><strong>For systems using
            <a href="https://nginx.org">Nginx</strong></a>:
            You can add your content in a location of your
            choice and edit the <code>root</code> configuration directive
            in <code>/etc/nginx/nginx.conf</code>.</p>

            <div id="logos">
              <a href="https://rockylinux.org/" id="rocky-poweredby"><img src="icons/poweredby.png" alt="[ Powered by Rocky Linux ]" /></a> <!-- Rocky -->
              <img src="poweredby.png" /> <!-- webserver -->
            </div>
          </div>
          </div>

          <footer class="col-sm-12">
          <a href="https://apache.org">Apache&trade;</a> is a registered trademark of <a href="https://apache.org">
          the Apache Software Foundation</a> in the United States and/or other countries.<br />
          <a href="https://nginx.org">NGINX&trade;</a> is a registered trademark of <a href="https://">F5 Networks, Inc.</a>.
          </footer>

      </body>
    </html>
#
    debian@debian:/etc/network$ curl supersite.tp3.b2
Sors exactement la même chose
