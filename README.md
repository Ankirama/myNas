__________________________
Le README.md n'est pas a jour et contient surement des incoherences car j'ai par moment oublie d'update ce que je faisais.
Si vous avez le moindre probleme me contacter (mar_b@epitech.eu)
J'essayerai de le mettre a jour plus tard si j'ai du temps
__________________________

# myNas
adminsys project 2015 epitech

# packages
apt-get update ; apt-get install -y emacs whois sudo nfs-kernel-server samba isc-dhcp-server bind9 bind9utils bind9-doc bzip2

# lc_all problem:
http://askubuntu.com/questions/162391/how-do-i-fix-my-locale-issue

# alias
Se log en root; installer emacs:
echo "alias ne='emacs'" >> /root/.bashrc; source /root/.bashrc;

# creer users / set les droits
```
useradd --home /home/admin --password `mkpasswd admin42` -m -k /etc/skel -s /bin/bash admin; usermod -G admin admin; echo "admin ALL=(ALL:ALL) ALL" >> /etc/sudoers;
```
```
groupadd users;
useradd --home /home/papy --gid users --password `mkpasswd papy42` -m -k /etc/skel -s /bin/bash papy;
useradd --home /home/antoine --gid users --password `mkpasswd antoine42` -m -k /etc/skel -s /bin/bash antoine;
useradd --home /home/zouhir --gid users --password `mkpasswd zouhir42` -m -k /etc/skel -s /bin/bash zouhir;
```

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
```
fdisk /dev/sdb (en supposant que sdb existe fdp.)
n, *enter, *enter, *enter
w
```
/deb/sdb1 doit maintenant etre present (si non, c'est con.)
http://askubuntu.com/questions/154180/how-to-mount-a-new-drive-on-startup

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

# changements (tout ce qui suit ou la plupart des chsoes est a faire avec sudo, ne pas le faire en root a cause des droits generes)
```
usermod -G antoine antoine; usermod -G zouhir zouhir; usermod -G papy papy;
cd /mynas; chown admin:admin admin; chown antoine:antoine antoine; chown papy:papy papy; chown zouhir:zouhir zouhir;
chmod g+rwx antoine; chmod g+rwx papy; chmod g+rwx zouhir;
```

# changements /etc/proftpd/proftpd.conf
ajouter:
\<Directory /\>
HideFiles ^\\..*
\</Directory\>

# autoban bruteforce/ddos
http://blog.nicolargo.com/2012/02/proteger-son-serveur-en-utilisant-fail2ban.html
```
apt-get install fail2ban
ne /etc/fail2ban/jail.conf:
    chercher [ssh] puis rajouter:
        action = iptables[name=SSH, port=ssh, protocol=tcp]
        maxretry = 3 (au lieu de 6)
        bantime = 900 (en secondes)
```

# configurer mail server
https://www.isalo.org/wiki.debian-fr/Installation_sur_une_Squeeze_d'un_serveur_mail_complet_(Postfix_Postfixadmin_Dovecot_Mysql_Amavisd-new_Spamassassin_Clamav_Postgrey_Squirrelmail_Roundcube)_avec_gestion_des_filtres_Imap_et_des_quotas

```
apt-get install -y mysql-server
ne /etc/bind/db.mynas -->
smtp            IN      A       10.10.100.24
mynas.         IN      MX      10      smtp
```

# configurer control panel (ajenti)
```
wget -O- https://raw.github.com/Eugeny/ajenti/master/scripts/install-debian.sh | sh
```
changer password
aller dans plugings: installer ctdb
                     installer S.M.A.R.T.
aller dans samba: puis configurer (je dois le faire)

# quota (limit size user directory)
https://www.debian-administration.org/article/47/Limiting_your_users_use_of_disk_space_with_quotas

# configurer samba
http://www.linux-france.org/article/serveur/samba-mhp/smb-conf.htm
http://www.commentcamarche.net/contents/1027-mise-en-place-de-samba-sous-linux

# configurer quota:
```
apt-get install -y quota libnet-ldap-perl
emacs /etc/fstab --> la ligne ou y'a mynas (derniere normalement) changer defaults par defaults,usrquota
```
(reboot ou mount -o remount /mynas)
```
setquota -u antoine 500000 500000 0 0 /dev/sdb1; setquota -u papy 500000 500000 0 0 /dev/sdb1; setquota -u zouhir 500000 500000 0 0 /dev/sdb1; setquota -u admin 1000000 1000000 0 0 /dev/sdb1;
```

# configurer / installer transmission
https://www.guillaume-leduc.fr/la-seedbox-facile-sous-debian-avec-transmission.html (methode sale / a chier)
http://voidandany.free.fr/index.php/executer-transmission-avec-lutilisateur-de-son-choix-et-deplacer-son-fichier-de-configuration-vers-home/ (tres performant, je remercie l'auteur)

```
apt-get install -y transmission-deamon;
service transmission-daemon stop;
```
```
emacs /etc/default/transmission-daemon; --> commenter OPTIONS\
```
```
emacs /etc/init.d/transmission-daemon; --> changer USER=debian.. par USER=admin
```
```
mkdir -p ~/.config/transmission-daemon;
cp /etc/transmission-daemon/settings.json ~/.config/transmission-daemon/;
chown admin:admin -R ~/.config;
mkdir /mynas/admin/downloads```
```
```
emacs ~/.config/transmission-daemon/settings.json; -->
  "download-dir": "/mynas/admin/downloads"
  "rpc-enabled": true
  "rpc-password": "tonpassword"
  "rpc-username": "admin ou ce que tu veux"
  "rpc-whitelist-enabled": false
```

# configurer subsonic
http://www.subsonic.org/pages/installation.jsp#debian

```
apt-get install openjdk-7-jre; wget http://downloads.sourceforge.net/project/subsonic/subsonic/5.2.1/subsonic-5.2.1.deb?r=http%3A%2F%2Fwww.subsonic.org%2Fpages%2Fdownload2.jsp%3Ftarget%3Dsubsonic-5.2.1.deb&ts=1432778765&use_mirror=vorboss && mv subsonic-5.2.1.deb?r=http:%2F%2Fwww.subsonic.org%2Fpages%2Fdownload2.jsp?target=subsonic-5.2.1.deb&ts=1432778765&use_mirror=vorboss subsonic.deb && sudo dpkg -i subsonic.deb;
```

changer reglages / parametres en fonction des gouts

# subdomain / virtualhost
cd /etc/apache2/sites-available
pour chaque virtualhost creer un fichier, voici ma liste:
  http://pastebin.com/7pgXcML5
  comprend transmission, subsonic, ajenti, h5ai

# h5ai
http://yaui.me/how-to-set-up-apache-and-h5ai-in-raspi/

```
apt-get install -y apache2-doc apache2-utils libapache2-mod-php5 php5 php-pear php5-xcache php5-mysql mysql-server mysql-client
```

suivre le tuto sauf pour le Vhost, ne pas mettre le Directory Index ... mais:

```
emacs /etc/apache2/mods-available/dir.conf puis rajouter /_h5ai/server/php/index.php
```

# git-server
https://www.digitalocean.com/community/tutorials/how-to-install-git-on-debian-7
https://www.sheevaboite.fr/articles/installer-serveur-git-auto-heberge-partie-1
https://www.sheevaboite.fr/articles/installer-son-propre-serveur-git-partie-2

# gitlab
http://eserdeniz.fr/articles/view/4/installer-gitlab-sous-debian-7-avec-nginx-et-mysql
http://stackoverflow.com/questions/22825497/installing-gitlab-missing-modernizer

# webalizer
http://www.debianhelp.co.uk/webalizer.htm

# mediawiki
https://www.rosehosting.com/blog/how-to-install-mediawiki-on-debian-wheezy/
http://www.mediawiki.org/wiki/Manual:Running_MediaWiki_on_Ubuntu/fr
