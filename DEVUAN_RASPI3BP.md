# lxc
checkpoint restore: missing
CONFIG_FHANDLE: enabled
CONFIG_EVENTFD: enabled
CONFIG_EPOLL: enabled
CONFIG_UNIX_DIAG: missing
CONFIG_INET_DIAG: enabled
CONFIG_PACKET_DIAG: missing
CONFIG_NETLINK_DIAG: missing
File capabilities: enabled

| Symbol: CHECKPOINT_RESTORE [=n]                                                                                                                                       |
| Type  : bool                                                                                                                                                          |
| Prompt: Checkpoint/restore support                                                                                                                                    |
|   Location:                                                                                                                                                           |
|     -> General setup                                                                                                                                                  |
| (1)   -> Configure standard kernel features (expert users) (EXPERT [=n])                                                                                              |

| Prompt: UNIX: socket monitoring interface                                                                                                                             |
|   Location:                                                                                                                                                           |
|     -> Networking support (NET [=y])                                                                                                                                  |
|       -> Networking options                                                                                                                                           |
| (1)     -> Unix domain sockets (UNIX [=y])                                                                                                                            |

cation:                                                                                                                                                           |
|     -> Networking support (NET [=y])                                                                                                                                  |
|       -> Networking options                                                                                                                                           |
| (1)     -> Packet socket (PACKET [=y])                                                                                                                                |

| Symbol: NETLINK_DIAG [=n]                                                                                                                                             |
| Type  : tristate                                                                                                                                                      |
| Prompt: NETLINK: socket monitoring interface                                                                                                                          |
|   Location:                                                                                                                                                           |
|     -> Networking support (NET [=y])                                                                                                                                  |
| (1)   -> Networking options                                                                                                                                           |

make -j 24 ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu-
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- INSTALL_MOD_PATH=output modules_install

 CONFIG_VIRTIO_PCI=y (Virtualization -> PCI driver for virtio devices)
CONFIG_VIRTIO_BALLOON=y (Virtualization -> Virtio balloon driver)
CONFIG_VIRTIO_BLK=y (Device Drivers -> Block -> Virtio block driver)
CONFIG_VIRTIO_NET=y (Device Drivers -> Network device support -> Virtio network driver)
CONFIG_VIRTIO=y (automatically selected)
CONFIG_VIRTIO_RING=y (automatically selected)

# add xfs to cmdline.txt
dwc_otg.fiq_fix_enable=2 console=ttyAMA0,115200 kgdboc=ttyAMA0,115200 console=tty1 root=/dev/mmcblk0p2 rootfstype=***xfs*** rootwait ~~~rootflags=noload~~~ net.ifnames=0


#x2go

suivre https://wiki.debian.org/sbuild#Setup

```shell
sudo apt-get install sbuild
sudo mkdir /root/.gnupg
sudo sbuild-update --keygen
sudo sbuild-adduser $LOGNAME
sudo sbuild-createchroot --include=eatmydata,ccache,gnupg ascii /srv/chroot/ascii-arm64-sbuild http://pkgmaster.devuan.org/merged
sbuild -n --jobs=2 -sA --dist=ascii --build-dep-resolver=apt --resolve-alternatives --arch=arm64 --chroot=ascii-arm64-sbuild .

sudo sbuild-shell source:ascii-arm64-sbuild # pour le debug

cd ../x2goserver
sbuild -n --jobs=2 -sA --dist=ascii --build-dep-resolver=apt --resolve-alternatives --arch=arm64 --chroot=ascii-arm64-sbuild .
cd ../x2goclient
sbuild -n --jobs=2 -sA --dist=ascii --build-dep-resolver=apt --resolve-alternatives --arch=arm64 --chroot=ascii-arm64-sbuild .
cd ..
ARAR=/var/cache/apt/archives/
sudo cp -v *deb $ARAR
sudo apt install ${ARAR}x2goserver_*.deb ${ARAR}x2goserver-common_4.1.0.1-0x2go1_arm64.deb ${ARAR}x2goserver-x2goagent_4.1.0.1-0x2go1_arm64.deb ${ARAR}libx2go-server-perl_4.1.0.1-0x2go1_all.deb ${ARAR}x2goserver-xsession_4.1.0.1-0x2go1_all.deb ${ARAR}libx2go-log-perl_*.deb ${ARAR}libx2go-server-db-perl_*.deb ${ARAR}nxagent_*.deb ${ARAR}libnx-x11-6_*.deb ${ARAR}libxcomp3_*.deb ${ARAR}libxcompshad3_*.deb ${ARAR}nx-x11-common_*.deb ${ARAR}x2goclient_*.deb
```





## debug


  ```dpkg -l | grep x2goserver```
	to check if you have the good version. on x86 I did a dist-upgrade to have the good version of nxproxy

	***ATTETION*** lorsque l'on lance un programme et qu'il se detache du pere il est considéré comme perdu et la session se ferme. gnome-term est ce type de programme donc éviter de tester le bon fonctionnement avec terminal.

#distcc

## quick'n dirty

### master
```make -j18 CC="distcc aarch64-linux-gnu-gcc-6" CXX="distcc aarch64-linux-gnu-g++-6" CPP="distcc aarch64-linux-gnu-cpp-6" build```

### slave(remote)
```EXPORT CC CPP CXX puis lancement manuel de distccd```

# ASR

## jasper

http://jasperproject.github.io/documentation/installation/#manual-installation

https://renatocunha.com/blog/2012/04/playstation-eye-audio-linux/

rechercher module-alsa-source dans /etc/source/default.pa et coller :

load-module module-alsa-source device=hw:0,0

