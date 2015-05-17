# myNas
adminsys project 2015 epitech

# packages
apt-get update ; apt-get install -y emacs whois sudo nfs-kernel-server samba isc-dhcp-server bind9 bind9utils bind9-doc

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
http://webadonf.net/2011/04/configurer-le-service-dhcp-sous-debian-squeeze/

# lier dns + dhcp
http://webadonf.net/2011/04/gestion-dynamique-du-dns-avec-bind9-sous-debian-squeeze/

# configurer ssh

# configurer / installer transmission

# configurer ftp

# openmediavault raspberry:
http://www.htpcguides.com/install-openmediavault-raspberry-pi-nas-server-minibian/
http://www.pintaric.net/index.php?post/2013/05/10/QNap-TS-109-%3A-Wheezy-et-OpenMediaVault
http://forums.openmediavault.org/index.php/Thread/4541-Raspberry/
