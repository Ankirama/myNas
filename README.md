# myNas
adminsys project 2015 epitech

# packages
apt-get update ; apt-get install -y emacs whois sudo nfs-kernel-server samba isc-dhcp-server bind9 bind9utils bind9-doc

# lc_all problem:
http://askubuntu.com/questions/162391/how-do-i-fix-my-locale-issue

# alias
Se log en root; installer emacs:
echo "alias ne='emacs'" >> /root/.bashrc; source /root/.bashrc;

# creer users / set les droits
useradd --home /home/admin --password `mkpasswd admin42` -m -k /etc/skel -s /bin/bash admin; usermod -G admin admin; echo "admin ALL=(ALL:ALL) ALL" >> /etc/sudoers;

groupadd users;
useradd --home /home/papy --gid users --password `mkpasswd papy42` -m -k /etc/skel -s /bin/bash papy;
useradd --home /home/antoine --gid users --password `mkpasswd antoine42` -m -k /etc/skel -s /bin/bash antoine;
useradd --home /home/zouhir --gid users --password `mkpasswd zouhir42` -m -k /etc/skel -s /bin/bash zouhir;

# configurer dns
http://webadonf.net/2011/03/configurer-un-serveur-dns-avec-bind9-sur-debian-squeeze/ (edit ip)

# configurer dhcp
ajouter network adaptater (bridged)
dans /etc/network/interfaces --> http://pastebin.com/QvfW7ExU
http://webadonf.net/2011/04/configurer-le-service-dhcp-sous-debian-squeeze/

# lier dns + dhcp
http://webadonf.net/2011/04/gestion-dynamique-du-dns-avec-bind9-sous-debian-squeeze/

# configurer ssh
configurer /etc/ssh/sshd_conf  -> port 4224
                               -> PermitRootLogin no
service sshd reload

# create / mount new disk
fdisk /dev/sdb (en supposant que sdb existe fdp.)
n, *enter, *enter, *enter
w
/deb/sdb1 doit maintenant etre present (si non, c'est con.)
http://askubuntu.com/questions/154180/how-to-mount-a-new-drive-on-startup

# configurer / installer transmission

# configurer ftp
https://www.debian-administration.org/article/228/Setting_up_an_FTP_server_on_Debian

# openmediavault raspberry:
http://www.htpcguides.com/install-openmediavault-raspberry-pi-nas-server-minibian/
http://www.pintaric.net/index.php?post/2013/05/10/QNap-TS-109-%3A-Wheezy-et-OpenMediaVault
http://forums.openmediavault.org/index.php/Thread/4541-Raspberry/

# wifi + ethernet (ne pas prendre en compte, special rpi)
http://raspberrypi.stackexchange.com/questions/8851/setting-up-wifi-and-ethernet

# set up openmediavault (ne pas prendre en compte)
http://gamerz0ne.fr/2015/02/16/installer-un-serveur-nas-avec-openmediavault-omv/
http://www.eteknix.com/everybody-can-nas-beginners-guide-openmediavault/5/
http://en.jose-crispim.pt/artigos/armazenamento/armaz_art/06_openmediavault.html
http://www.yvesmasur.ch/articles/NAS/NAS-installation.pdf

# interface web
http://www.it-connect.fr/ajaxplorer-un-gestionnaire-de-fichiers-en-ligne-et-a-la-maison/#IV_Accs_l8217interface_d8217AjaXplorer

--------------------------------------------------
update:
--------------------------------------------------

# changements
usermod -G antoine antoine; usermod -G zouhir zouhir; usermod -G papy papy;
cd /mynas; chown admin:admin admin; chown antoine:antoine antoine; chown papy:papy papy; chown zouhir:zouhir zouhir;
chmod g+rwx antoine; chmod g+rwx papy; chmod g+rwx zouhir;

# changements /etc/proftpd/proftpd.conf
ajouter: 
<Directory />
HideFiles ^\..*
</Directory>

# autoban bruteforce/ddos
http://blog.nicolargo.com/2012/02/proteger-son-serveur-en-utilisant-fail2ban.html
apt-get install fail2ban
ne /etc/fail2ban/jail.conf:
    chercher [ssh] puis rajouter: 
        action = iptables[name=SSH, port=ssh, protocol=tcp]
        maxretry = 3 (au lieu de 6)
        bantime = 900 (en secondes)

# configurer mail server
https://www.isalo.org/wiki.debian-fr/Installation_sur_une_Squeeze_d'un_serveur_mail_complet_(Postfix_Postfixadmin_Dovecot_Mysql_Amavisd-new_Spamassassin_Clamav_Postgrey_Squirrelmail_Roundcube)_avec_gestion_des_filtres_Imap_et_des_quotas

apt-get install -y mysql-server 
ne /etc/bind/db.mynas --> 
smtp            IN      A       10.10.100.24
mynas.         IN      MX      10      smtp

