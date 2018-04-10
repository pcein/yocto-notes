## Yocto Links / Notes


Yocto/swupdate build for beaglebone black ok.
Doc: https://mkrak.org/2018/01/26/updating-embedded-linux-devices-part2/
[add IMAGE_FSTYPES = "ext4.gz" to build/local.conf]

[Note: the beaglebone build was abandoned as I got the Rpi build working]

## Rpi with swupdate and wifi networking

Here are the layers:

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

