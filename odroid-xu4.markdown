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
  
Follow the instructions [here](http://www.configserverfirewall.com/ubuntu-linux/ubuntu-set-static-ip-address/) to set a static IP address to the Odroid. Make sure that the address is `192.168.1.XXX` where `XXX` does not conflict with other configured devices. The gateway can be kept as `192.168.1.1` since that is the IP of the bullet.
  
### Dependencies

#### Git setup

sudo apt-get install git
Follow instructions [here](https://csil-git3.cs.surrey.sfu.ca/help/ssh/README) to create a new SSH key  
  
`git config --global user.name "Guardian on ODroid"`  
`git config --global user.email "uav-exec@sfu.ca"`  

#### Point Grey FlyCapture

sudo apt-get install libraw1394-11 libgtkmm-2.4-1v5 libglademm-2.4-1v5 libusb-1.0-0

Need to add instructions for PyCapture2.   
   
pyflycapture2 API:   
`git clone ssh://git@csil-git3.cs.surrey.sfu.ca:24/Guardian/pyflycapture2.git` in the home directory.   
  
Copy the `C` include files that come with the original flycapture API from the downloaded `flycapture_xxx_whatever/include/C` folder to `/usr/include/flycapture/C` folder.   
  
Follow the instructions in the pyflycapture README to install the API.
   
USB buffer:
  
http://stackoverflow.com/questions/31995954/pointgrey-sdk-hangs-on-startcapture
  
In `/media/boot/boot.ini`, scroll down to `bootargs` line and add `usbcore.usbfs_memory_mb=1000` within the `" "` where there are already some arguments present.
  
https://www.ptgrey.com/tan/10357

#### Vision System (Air)

sudo apt-get install python-dev python-pip exiv2 libmagickwand-dev python-pyexiv2 tmux

sudo pip install numpy wand

## GPIO Communication

From the home directory:
  
```
git clone https://github.com/hardkernel/wiringPi.git
cd wiringPi
./build
sudo chmod a+s /usr/local/bin/gpio
```
This will install the base WiringPi library and a utility called gpio. Typing `gpio readall` will show the current status of all gpio pins.   
  
From the home directory, follow instructions for setting up [WiringPi2](https://github.com/hardkernel/WiringPi2-Python) from the README.  
  
Resources:

http://odroid.com/dokuwiki/doku.php?id=en:odroid-xu4#gpio_ports

http://odroid.com/dokuwiki/doku.php?id=en:xu3_enhancement_gpio30

http://odroid.com/dokuwiki/doku.php?id=en:xu4_hardware#expansion_connectors

http://odroid.com/dokuwiki/doku.php?id=en:xu3_hardware_gpio

