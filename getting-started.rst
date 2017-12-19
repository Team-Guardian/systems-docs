********************************
Getting Started on Team Guardian
********************************

Software/Vision System
======================

Team Guardian makes heavy use of Linux, Python, and Git, all of which are great tools and widely used in the industry. Becoming familiar with these tools will enable you to make meaningful contributions to the software/vision system team in a short time.

Below is a collection of tutorials and interactive practice tools to get you started:

1. `Linux command line tutorial <http://linuxcommand.org/lc3_learning_the_shell.php>`_;
#. `Linux command line practice <https://www.shortcutfoo.com/app/dojos/command-line>`_;
#. `Python tutorial and practice <https://www.codecademy.com/learn/python>`_;
#. `Git interactive tutorial <https://try.github.io/>`_;
#. `Git branching interactive tutorial <http://learngitbranching.js.org/>`_;

Camera Control Software
-----------------------

On-Board Computer Software
--------------------------

**ODroid XU4.** Equipment on board the drone is controlled from an ODroid XU4 computer. Read more on how to setup an ODroid board to run your programs in the `installation guide <odroid-xu4.html>`_.

**Networking.** *This section is WIP.*

Ground Station Software
-----------------------

Photos captured by the camera on board of the drone are streamed back to the ground station for processing. The deciding factor in overall team performance is how efficiently we can analyze the images. We build software to streamline common analysis steps and automate certain identification and classification steps. Lastly, we build a control dashboard to manage the ground station from a single place. 

**Target Analysis and Geolocation (TAG) Tool.** TAG is a tool for analyzing large sets of images and identifying points of interest (POI). It's written in Python in the Qt framework. To learn more about TAG, read the `documentation <http://tagger.rtfd.io>`_ and go through the `source code on GitHub <https://github.com/Team-Guardian/tagger>`_.

**Automatic Object Detection and Classification.** *This section is WIP.*

Autopilot
=========

Electrical
==========

**Battery Handling.** Drone and some equipment is powered with lithium-polymer (LiPo) batteries that can be dangerous if handled improperly. Read more on LiPo safety in our `battery handling instructions <battery-handling.html>`_.

Mechanical
==========