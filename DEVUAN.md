# Orange PI PC

## download devuan for orangePIPC

https://files.devuan.org/devuan_ascii/embedded/

more exactly : https://files.devuan.org/devuan_ascii/embedded/devuan_ascii_2.0.0_armhf_sunxi.img.xz

follow readme instructions : https://files.devuan.org/devuan_ascii/embedded/README.txt

unfortunately orangepi_pc dtb file is missing. So you must make it yourself. Now you can download this one thanks to parazyd :)

to make it yourself when your hardware is certified to be in mainline kernel follow parazyd instructions, otherwise your constructor instruction. orangepi_pc is in mainline :)


## dtbs

	conversationIRC

	10:18 grillon I'm back :)
	10:18 grillon https://paste.pound-python.org/show/wW5hUz0yD0krjLXmivpS/
	10:18 avocado`` Title: Paste #wW5hUz0yD0krjLXmivpS at spacepaste (at paste.pound-python.org)
	10:20 grillon parazyd : I cut the end because it's always the same until reboot
	10:23 parazyd grillon: Can you try this dtb instead? https://parazyd.org/pub/tmp/sun8i-h3-orangepi-pc.dtb
	10:23 grillon ok
	10:24 parazyd You've used the one from u-boot?
	10:33 grillon Yes that's the one from u-boot parazyd
	10:33 grillon your dtb works fine I'm able to boot no error
	10:33 grillon why?
	10:34 parazyd Because they're probably different. This one that I gave you comes from the linux sources that are the same kernel you're running.
	10:34 grillon mine is 13kB and yours 18kB
	10:34 grillon ok dtb are included in linux sources?
	10:37 parazyd The sources, yes. You have to compile them.
	10:38 parazyd make ARCH=arm CROSS_COMPILE=yourcompilerprefix- dtbs (if you're crosscompiling)
	10:38 parazyd ^ You would run this in the linux sources directory, naturally.
	10:38 grillon nice thank you :) I'll try
	10:39 grillon thank you for your help :)
	10:39 parazyd You're welcome ;) 


when you have your dtb file copy it to /boot/dtbs of your sdcard.

## dump u-boot-sunxi-with-spl.bin

## hdmi display output

seul la console uart fonctionnait pour moi.

	J'avais le message suivant :

	[   10.602463] sun4i-drm display-engine: failed to bind 1100000.mixer (ops 0xc0a6f510)
	: -517
	[   10.610643] sun4i-drm display-engine: Couldn't bind all pipelines components
	[   10.617738] sun4i-drm display-engine: master bind failed: -517

en cherchant un peu j'ai trouvé ça :

https://gitlab.com/ymorin/buildroot/commit/c7a445a58fac3adb5d5da2d3bbce649814646ab9#606c92b5642326d32a9316d575c3ee6505fb7a3d

et ensuite parazyd, encore merci à lui. A recompilé le noyau avec ces options supplementaires :

	CONFIG_SND_SUN8I_CODEC=y
	CONFIG_SND_SUN8I_CODEC_ANALOG=y
	CONFIG_SND_SUN4I_I2S=y
	CONFIG_SND_SUN4I_SPDIF=y
	CONFIG_SUN8I_DE2_CCU=y

a présent cela fonctionne.


## determiner la version noyau

https://stackoverflow.com/questions/47659997/reverse-engineering-a-zimage

L'outils extract-ikconfig dans les sources du noyau permet d'extraire le fichier de config du noyau et ce fichier indique la version en entete.

## partir de zero

Il y a des fichers de configuration selon l'architecture dans le rep arch/arm/boot/dts/  du noyau par example pour l'orangepi_pc :

arch/arm/boot/dts/sun8i-h3-orangepi-pc.dtb

plus d'info : https://github.com/umiddelb/armhf/wiki/How-To-compile-a-custom-Linux-kernel-for-your-ARM-device

## communication serie : uart

rien ne s'affiche à l'ecran passer en mode debug en restant appuyé sur le bouton com au moment de l'allumage.

pour les branchement et qq details sup : https://linux-sunxi.org/UART


Il n'y a que 3 pin : ground,rx,tx. Le rx de votre module va sur le tx de la carte, le tx sur le rx de la carte et ground sur ground.

Ensuite screen /dev/ttyUSBX 115200

X correspond au numero de port alloué dynamiquement.

## installation OS:

## references :

* https://wiki.archlinux.org/index.php/Orange_Pi
* http://aperiodic.net/screen/quick_reference
* https://linux-sunxi.org/UART
* https://stackoverflow.com/questions/47659997/reverse-engineering-a-zimage
* https://gitlab.com/ymorin/buildroot/commit/c7a445a58fac3adb5d5da2d3bbce649814646ab9#606c92b5642326d32a9316d575c3ee6505fb7a3d
* http://www.orangepi.org/Docs/Building.html
* https://files.devuan.org/devuan_ascii/embedded/README.txt
* https://linux-sunxi.org/Orange_Pi_PC
* https://linux-sunxi.org/Mainline_U-Boot#Supported_Devices
* https://linux-sunxi.org/AR100
* https://www.denx.de/wiki/DULG/Manual
* https://www.gnupg.org/howtos/fr/GPGMiniHowto-3.html

## pourquoi devuan

http://lkml.iu.edu//hypermail/linux/kernel/1408.1/02496.html

