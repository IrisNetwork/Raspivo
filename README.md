# Rasbian
wget http://sourceforge.net/projects/minibian/files/2015-02-18-wheezy-minibian.tar.gz/download

dd de l'image

# Booter sur le raspberry

# Clés ssh
mkdir ~/.ssh

echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDVatDcbt/++gv3Lassv1v+JgJPYoQaeNLQV6HjSb/3ZHcNr/2+qvx83fBk/ZAbhqpjo3QqbJS+hFF5YnDcu7I5YCJ1OeMN87ZX0+dg9yWAKXI4zCmChEGOpCY9HMnS/Nk/N+uXlQsENv7lsU14Wv2usiV7XyiD7GuzvjbmTr4eJmf+3m8vp3dFkohIjX4v8sxq2UXCmU4fyOk1Ns19eNRPIsvRr7zG1kvzb2Z27wDp+9+73wIa8Cvbr4iAS+qfWPrgWAuHmSKBUDtoanRwNzqcdaK3/HT6o8oGZ21/ki4t7DgPvAxIqt9zeA02yLR7RJl5RQJVBZJ26b7j8rVCVtPN grabouin" >> ~/.ssh/authorized_keys
  
# ipv6
sed -i -n -e :a -e '1,2!{P;N;D;};N;ba' /etc/network/interfaces

# Resize de la carte sd
fdisk /dev/mmcblk0

p

d 2

n p 2 (start from le p)

w
  
shutdown -r now

resize2fs /dev/mmcblk0p2

# Installation
cat > /tmp/locales.tmp << EOF

locales locales/locales_to_be_generated multiselect     en_US.UTF-8 UTF-8

locales locales/locales_to_be_generated multiselectfr_FR.UTF-8 UTF-8

locales locales/default_environment_locale      select  fr_FR.UTF-8

EOF

debconf-set-selections /tmp/locales.tmp

dpkg-reconfigure locales

while ! apt-get update ; do echo Erreur on recommence ; done

while ! apt-get -d install zsh ncurses-term git mpg123 libmpg123-0 -y ; do echo Erreur on recommence ; done

apt-get install zsh ncurses-term git mpg123 libmpg123-0 -y

chsh -s /bin/zsh

git clone git://github.com/robbyrussell/oh-my-zsh.git /etc/oh-my-zsh

while ! apt-get update ; do echo Erreur on recommence ; done

while ! apt-get upgrade -y ; do echo Erreur on recommence ; done

while ! apt-get dist-upgrade -y ; do echo Erreur on recommence ; done

reboot

# Install classique
cat > /etc/apt/sources.list.d/irisnetwork.list << EOF

deb http://www.iris-network.fr/raspivo wheezy main

EOF

while ! apt-get update ; do echo Erreur on recommence ; done

while ! apt-get install -y xivo ; do echo Erreur on recommence ; done

reboot

# FINI !
## Se connecter à l'interface web, et faire comme d'habitude
