*****************************
Odroid XU4 Setup Instructions
*****************************

Setup
=====

**Prerequisites.** Download the full Ubuntu image (not minimal) for ODroid XU4 from `here <https://odroid.in/ubuntu_16.04lts/>`_. Grab a copy of `Etcher <https://etcher.io/>`_ to flash the image onto a micro-SD card - you have one ready, right?

*Note*: as of writing this, Ubuntu 16.04 is not the latest version of Ubuntu available for the ODroid XU4. The reason we're sticking with 16.04 for now is because some of the software we use - especially the less popular one - might not be compatible with the latest version of Ubuntu. For example, the camera control software provided by FLIR relies on libraries that have been deprecated in Ubuntu 18.04 and are only available in Ubuntu 16.04.

**Booting Up.** After Etcher is done flashing the image, safely eject the card and plug the micro-SD into the ODroid. Before turning on the power, make sure the boot media slider is set to uSD (not eMMC). Red and blue LEDs will turn on when the ODroid is ready. 

**Connecting to ODroid.** Connect the ODroid to a router with an ethernet cable - your PC must be on the router's network, too, either via a wired or wireless connection. The router must be connected to the internet, otherwise you won't be able to update the ODroid and pull code from repositories.

After you connect the ODroid to the router, check it's IP address either via the homepage of the router, or by scanning the network using `nmap <https://nmap.org/>`_. Let's assume the IP address of the ODroid is ``192.168.0.17``. Connect to the ODroid by typing in your terminal:

.. code-block:: bash

    ssh odroid@192.168.0.17

Confirm you want to continue connecting; when prompted for a password, enter ``odroid``. Remember, you won't see the characters you're entering in the terminal - this is a security feature. If everything goes well, your terminal prompt will change to ``odroid@odroid:~$``.

**Updating.** After you successfully connect to the ODroid, get the latest updates for Ubuntu by typing in the terminal:

.. code-block:: bash

    sudo apt-get update
    sudo apt-get upgrade

Remember, the default ``sudo`` password is ``odroid``. The update may take a few minutes - at this point you can grab a cup of coffee, and by the time you're back the update will have finished. We recommend you reboot the ODroid just in case, although it is not required.

.. code-block:: bash

    sudo reboot

The ODroid might take about a minute to reboot - after a minute you can start trying to connect to the board via ``ssh`` (as described above). After you have successfully connected, the ODroid is ready to be configured to run your on-board programs.

Collecting Dependencies
=======================

**Git.** Git doesn't come with Ubuntu out-of-the-box, so install it with the following command:

.. code-block:: bash

    sudo apt-get install git

**Point Grey/FLIR Spinnaker.** To run any camera control software on the ODroid, you will need to install the Spinnaker SDK packages provided by Point Grey. First, you will need to get tarball with the SDK installer onto the ODroid. Downloading from the website requires you to register an account - if you don't want to get one, you can download the tarball from the Team Guardian repository.

.. code-block:: bash

    cd ~/Documents
    git clone https://github.com/Team-Guardian/spinnaker.git

The version of Spinnaker SDK in our repository is not necessarily the latest, but it's the version we successfully ran our code with before. Once you've downloaded the tarballs, extract the files with the following command.

.. code-block:: bash

    tar xvfz spinnaker-<version>_armhf.tar.gz

Next, move into the directory you just extracted (``ls`` if you're unsure what the name of it is). From here, follow the instructions in the README_ARM on installing the Spinnaker SDK. When you get prompted for the name of the user to be added to the user group, enter ``odroid``. Once the installation is complete, reboot the ODroid.

After you're done installing the SDK, you will likely need to update the size of USBFS buffer to be able to get images from the camera. There are instructions on how to do that in README_ARM, but they only work on Desktop versions of Ubuntu. To change the size of the buffer on the ODroid, you should modify ``/media/boot/boot.ini`` file instead (`StackOverflow explanation <https://stackoverflow.com/questions/31995954/pointgrey-sdk-hangs-on-startcapture>`_). In the line that sets the ``bootargs``, append ``usbcore.usbfs_memory_mb=1000`` to whatever is already written in quotes (``" "``). 

.. code-block:: bash

    sudo nano /media/boot/boot.ini

Once you're finished editing the ``boot.ini`` file, reboot the ODroid. After it turns back on, make sure that the buffer size has been changed successfully.

.. code-block:: bash

    cat /sys/module/usbcore/parameters/usbfs_memory_mb

*Note*: if you search Google for "spinnaker sdk odroid xu4 install", you will likely come across these `instructions <https://www.ptgrey.com/tan/11145>`_ on the Point Grey website. We tried following those, but they didn't work because the kernel version they were written for is older than the one we are using. These instructions also tell you to recompile the kernel to change the size of USBFS buffer, which is not necessary (see README_ARM/TROUBLESHOOT USB STREAM).

**WiringPi.** `WiringPi <http://wiringpi.com/>`_ is a driver library for accessing GPIO pins on the ODroid. From the Documents folder (enter ``cd ~/Documents`` in the terminal to quickly get there), type in the terminal: 

.. code-block:: bash

    git clone https://github.com/hardkernel/wiringPi.git
    cd wiringPi/
    ./build

After the build script finishes executing, test your installation of WiringPi.

.. code-block:: bash

    gpio -v
    gpio readall

If the installation was successful, you should see something similar in your terminal.

.. image:: assets/odroid-xu4/odroid-xu4-wiring-pi-test.png
    :alt: Output of `gpio -v` and `gpio readall` commands
    :scale: 70 %

Finally, get a version of WiringPi with a Python wrapper from `Hardkernel's repository <https://github.com/hardkernel/WiringPi2-Python>`_:

.. code-block:: bash

    git clone https://github.com/hardkernel/WiringPi2-Python.git

Finish installing WiringPi2-Python by following the instructions in the README.

**PySimpleBGC.** PySimpleBGC is a Python package providing an API to communicate with `gimbal control board <https://www.basecamelectronics.com/simplebgc32ext/>`_. Install it with `pip <https://pip.pypa.io/en/stable/>`_:

.. code-block:: bash

    sudo pip install pysimplebgc