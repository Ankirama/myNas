# myNas
adminsys project 2015 epitech

# packages
apt-get update ; apt-get install -y emacs sudo nfs-kernel-server samba isc-dhcp-server bind9 bind9utils bind9-doc

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
## ipv4
echo -e "RESOLVCONF=no\nOPTIONS=\"-4 -u bind\"" > /etc/default/bind9;


# configurer dhcp


# configurer ssh

# configurer / installer transmission

# configurer ftp
