# Résumé R203 - Services sur réseau

## Sommaire :
<br>   

* > ## DNS
* > ## DHCP
* > ## SSH 
* > ## HTTP
* > ## FTP 
* > ## Wireshark

-------

# I - DNS

Obtenir adresse serveur DNS  

     cat /etc/resolv.conf

Résolver adresse avec un autre DNS

    dig mars.com @8.8.8.8

Dig types : 

    dig -t A URL ## Adresse IPv4

    dig -t CNAME URL ## Demande nom canonique

    dig -t TXT URL ## Informations descriptives sous forme de texte

    dig -t MX URL ## Trouver domaines mails

    dig -t NS URL ## Liste serveurs d'autorité

    dig -t SOA URL ## Informations autorité de la zone

    dig -t AAAA URL ## Adresse IPv6 

Rajouter des noms symboliques avec une IP :   

    nano /etc/hosts

-------

# II - DHCP

Package : 

    apt install isc-dhcp-server

Configuration : 

    /etc/dhcp/dhcpd.conf

Démarrer serveur : 

    dhcpd -d <interface>

Regarder les baux : 

    cat /var/lib/dhcp/dhcpd.leases

-----

# III - SSH

Se connecter avec une clé :

    ssh -i clé.rsa user@IP

-----

# IV - HTTP 

Package : 

    apt install apache2 

Avoir une configuration clean : 

    rm -R /etc/apache2/

    rm -R /var/www/html/

Lancer le service : 

    service apache2 start

Pour mettre les pages accessibles : 

    cd /var/www/html/

Page par défaut :

    index.html

Pour configurer le service :

    nano /etc/apache2/sites-available

On modifie ensuite chaque fichier de configuration.

Pour rediriger : 

    Redirect 301 /reglisse https://lareglisserie.fr ## Dans le fichier de configuration du virtual host

On doit restart le service : 

    service apache2 restart

Pour rajouter un port d'écoute : 

    nano /etc/apache2/ports.conf

    Listen PORT

Désactiver site (fichier de configuration) :

    a2dissite fichier.conf

Activer site (fichier de configuration) :

    a2ensite fichier.conf

Pour indiquer le répertoire du site : 

    DocumentRoot /repertoire/ 

Pour rajouter notre site avec un nom symbolique : 

    nano /etc/hosts

-----

# V - FTP 

Connexion à un serveur ftp : 

    ftp
    > open IP

Déplacer un fichier depuis notre machine locale : 

    > put 
    > /Chemin/vers/notre/fichier
    > Nom qu'on donne au fichier

Passer en mode passif :

    > Passive

Sortir du serveur FTP :

    > bye

Notions : 

    Mode passif : Cela indique que pour le transfert de fichier, ce sera le serveur lui-même qui fournira le port par lequel les données passeront. 

    Nous avons une trame de type : Entering Passive Mode (ip,ip,ip,ip,port1,port2)

    On calcule le port de transfert en faisant :

    port1*256 + port2

/////


     Mode actif : Cela indique que pour le transfert de fichier, ce sera le client lui-même qui fournira le port par lequel les données passeront. 

    Nous avons une trame de type : Entering Active Mode (ip,ip,ip,ip,port1,port2)

    On calcule le port de transfert en faisant :

    port1*256 + port2

/////

    Port écoute FTP : 21
/////

    Port transfert FTP : 22

## Installation serveur FTP : 

Package : 

     sudo apt install vsftpd

Options :

    listen = 
    listen_ipv6 = Active ou non le fait de pouvoir accepter des connexions avec des adresses IpV6

    anonymous_enable = Active ou non le fait de pouvoir se connecter au serveur FTP avec le login anonymous, 
    qui est l’utilisateur

    local_enable = Pour autoriser les utilisateurs locaux de se logger

    dirmessage_enable = Option qui sert à envoyer des messages aux utilisateurs distants lorsqu’il change de 
    répertoire

    use_localtime = Le temps utilisé sera celui de notre Time zone utilisé par notre machine

    xferlog_enable = Active le fait de logger les upload et download du serveur FTP

    connect_from_port_20 = Pour être sur d’utiliser le port 20 pour le transfert de data.

Activer le service :  

     sudo /etc/init.d/vsftpd start

Regarder les logs : 

    cat  /var/log/vsftpd.log 
-----
# Wireshark

Associer plusieurs filtres : 

    http || ftp 

Pour voir le transfert de données FTP :

    ftp || ftp-data

