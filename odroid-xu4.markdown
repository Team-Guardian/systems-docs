# Odroid XU4

## Setup

Download Ubuntu16.04 image (Odroid XU3 image works for XU4 as well) from [here](https://odroid.in/ubuntu_16.04lts/) or check [odroid.com](odroid.com) for latest version. 

Once the image is downloaded, flash it to a micro-SD card. I used [Etcher](https://etcher.io/) to do this. 

Once flashing is done, plug it into the Odroid, make sure that the slider is set to uSD and not eMMC. Power up the Odroid, the red and blue lights should both come on. Connect an ethernet cable from the Odroid to a local router and connect a PC to that network as well. Check the IP assigned to the Odroid by the router from the homepage of the router. Using terminal, type the following command:

`ssh odroid@192.168.1.103` where `192.168.1.103` is a dummy IP address. The default password is `odroid`.

Once in the Odroid, type:
`sudo apt-get update`
`sudo apt-get upgrade`

After this is done, your Odroid will have the latest kernel build for Ubuntu16.04.

Need to add instructions to change Odroid to a static IP.

### Dependencies

#### Point Grey FlyCapture

sudo apt-get install libraw1394-11 libgtkmm-2.4-1v5 libglademm-2.4-1v5 libusb-1.0-0

Need to add instructions for PyCapture2.

USB buffer:

http://stackoverflow.com/questions/31995954/pointgrey-sdk-hangs-on-startcapture

In `/media/boot/boot.ini`, scroll down to `bootargs` line and add `usbcore.usbfs_memory_mb=1000`

https://www.ptgrey.com/tan/10357

#### Vision System (Air)

sudo apt-get install python-dev python-pip exiv2 libmagickwand-dev python-pyexiv2 tmux

sudo pip install numpy wand

## GPIO Communication

Need to add instructions for setting up [WiringPi2](https://github.com/hardkernel/WiringPi2-Python)

Resources:

http://odroid.com/dokuwiki/doku.php?id=en:odroid-xu4#gpio_ports

http://odroid.com/dokuwiki/doku.php?id=en:xu3_enhancement_gpio30

http://odroid.com/dokuwiki/doku.php?id=en:xu4_hardware#expansion_connectors

http://odroid.com/dokuwiki/doku.php?id=en:xu3_hardware_gpio

