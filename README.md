# myNas
adminsys project 2015 epitech

# start
Se log en root; installer emacs (apt-get install -y emacs; echo "alias ne='emacs'" >> /root/.bashrc; source /root/.bashrc)

# creer users
useradd --home /home/admin --password `mkpasswd admin42` -m -k /etc/skel -s /bin/bash admin;__
groupadd users;__
useradd --home /home/papy --gid users --password `mkpasswd papy42` -m -k /etc/skel -s /bin/bash papy;__
useradd --home /home/antoine --gid users --password `mkpasswd antoine42` -m -k /etc/skel -s /bin/bash antoine;__
useradd --home /home/zouhir --gid users --password `mkpasswd zouhir42` -m -k /etc/skel -s /bin/bash zouhir;__

# configurer ssh
