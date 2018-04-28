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

IMAGE_FSTYPES += "wic.vmdk"    # (previously it was just vmdk)

## Extended SDK Links

https://wiki.yoctoproject.org/wiki/Application_Development_with_Extensible_SDK

https://www.yoctoproject.org/docs/2.0/dev-manual/dev-manual.html#using-devtool-in-your-workflow

## ESDK commands

Create a new layer:

```
bitbake-layers create-layer path/to/layer
```

devtool usage:

```
devtool add https://github.com/whbruce/bbexample.git
devtool edit-recipe bbexample
devtool build bbexample
devtool deploy-target bbexample root@ip
devtool finish bbexample /path/to/layer
```

Creating an extensible sdk:

```
bitbake image-recipe -c populate_sdk_ext
```
Installer in tmp/deploy/sdk


Extending and updating the esdk using devtool:

```
devtool sdk-update [http url]
devtool search pkg-name
devtool sdk-install -s recipe-name
```
Publishing the SDK:

a) Define a URL for the updater: http://mysite.com/sdk-updates

b) SDK_UPDATE_URL = "..."

c) oe-publish-sdk tmp/deploy/sdk/installer.sh /path/to/sdk/updates


## Fixing error: NO GNU_HASH in ELF binary

Add to your recipe:

```
INSANE_SKIP_${PN} = "ldflags"
INSANE_SKIP_${PN}-dev = "ldflags"
```

## Enable 'populate_sdk' for an image

Add: "inherit populate_sdk" to the recipe corresponding to the image.

## Note on running postinstall script (swupdate) to preserve data across updates

[to be written]

## Yocto image with Grub - for running inside virtualbox with swupdate integration

https://wiki.yoctoproject.org/wiki/TipsAndTricks/Running_YP_Image_On_AWS

Convert img to vmdk: http://www.linuxdesk.com/2017/03/qemu-img-usage-examples.html

https://serverfault.com/questions/401351/update-grub2-without-hardware-access-e-g-in-a-chroot?rq=1
