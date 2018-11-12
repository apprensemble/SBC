# Devuan sur orangepi pc2

La plateforme n'est pas encore supportée par devuan et je me suis donnée comme exercice de le porter manuellement puis de l'integrer sur arm-sdk.

## construction u-boot

cloner les sources : 

```sh
git clone git://git.denx.de/u-boot.git
```

puis suivre les indication du fichier board/sunxi/README.sunxi64

En résumer le document dit de 

### construire l'Arm Trusted Firmware

  https://github.com/ARM-software/arm-trusted-firmware/blob/master/docs/user-guide.rst

```sh
git clone https://github.com/ARM-software/arm-trusted-firmware.git
cd arm-trusted-firmware
export CROSS_COMPILE=aarch64-linux-gnu-
make plt=sun50i_a64 bl31
```
### exporter le resultat

puis faire un make config (orangepi_pc2_defconfig)

puis make

### ensuite??

construire le noyau???

Je suis tenté d'essayer de juste booter u-boot :p :)
