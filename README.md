## Yocto Links / Notes


Yocto/swupdate build for beaglebone black ok.
Doc: https://mkrak.org/2018/01/26/updating-embedded-linux-devices-part2/
[add IMAGE_FSTYPES = "ext4.gz" to build/local.conf]

[Note: the beaglebone build was abandoned as I got the Rpi build working]

## Rpi with swupdate and wifi networking

Here are the layers:

```bash
BBLAYERS ?= " \
  /home/pramode/rpi-swupdate/layers/poky/meta \
  /home/pramode/rpi-swupdate/layers/poky/meta-poky \
  /home/pramode/rpi-swupdate/layers/poky/meta-yocto-bsp \
  /home/pramode/rpi-swupdate/layers/meta-openembedded/meta-oe \
  /home/pramode/rpi-swupdate/layers/meta-swupdate \
  /home/pramode/rpi-swupdate/layers/meta-swupdate-boards \
  /home/pramode/rpi-swupdate/layers/meta-raspberrypi \
  /home/pramode/rpi-swupdate/layers/meta-openembedded/meta-python \
  /home/pramode/rpi-swupdate/layers/meta-openembedded/meta-networking \
  "
```

Files in the "layers" directory:

```
total 20
drwxrwxr-x 14 pramode pramode 4096 Apr  6 13:35 meta-openembedded
drwxrwxr-x 16 pramode pramode 4096 Apr  6 13:39 meta-raspberrypi
drwxrwxr-x 10 pramode pramode 4096 Apr  6 13:36 meta-swupdate
drwxrwxr-x 10 pramode pramode 4096 Apr  6 13:36 meta-swupdate-boards
drwxrwxr-x 11 pramode pramode 4096 Apr  6 13:35 poky
```

All of them are branch "rocko", except meta-swupdate-boards which is "master".

Here are some configuration settings in local.conf:

```bash
RPI_USE_U_BOOT = "1"
IMAGE_FSTYPES = "ext4.gz"
IMAGE_FSTYPES_append = " wic"

KERNEL_IMAGETYPE = "uImage"

WKS_FILE = "sdimage-raspberrypi.wks"

DISTRO_FEATURES_append = " bluez5 bluetooth wifi"

IMAGE_INSTALL_append = " linux-firmware-bcm43430  kernel-modules bluez5 i2c-tools python-smbus bridge-utils hostapd dhcp-server iptables wpa-supplicant crda iw wireless-tools dhcp-client"
```
## Create vmdk images

CONFIG_FSTYPES += "wic.vmdk"    # (previously it was just vmdk)


