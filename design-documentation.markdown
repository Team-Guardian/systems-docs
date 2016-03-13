# Design Documentation

## Overview

This documentation provides complete information about the entire UAV system and design decisions made by the team. The material within is suitable for use in design reports for competitions or for clients.

## Table of Contents

* [Airframe](design-documentation#airframe)
* [Mechanical System](design-documentation#mechanical-system)
* [Electrical and Electronics](design-documentation#electrical-and-electronics)
* [Autopilot](design-documentation#autopilot)
* [Vision System](design-documentation#vision-system)

## Airframe

## Mechanical System

## Electrical and Electronics

## Autopilot

## Vision System

### Objectives

The vision system should be as hands-free as possible. The major tasks are:

* Capture imagery at an appropriate rate to get good overlap
* Tag images with telemetry (GPS and attitude data) and transfer them to the ground wirelessly
* Georeference images using tagged telemetry
* Detect and identify targets in images, including:
  * Polygons
  * QR codes and barcodes
  * Block letters
  * Structures reflecting or emitting high amounts of infrared light
* Output detected target information to text file and/or pass it to the autopilot system for flight path generation
* Overlay (basic) or stitch (advanced) images into a composite map using tagged telemetry

### Equations

#### Determining Visible Area in an Image

A camera with a horizontal angle of view `h` and a vertical angle of view `v` pointing at the ground at an altitude `a` makes a triangle with the ground for each of these dimensions, as in the image below. The segment `d` represents the distance or length of the visible ground from one end of the photo to the other. This is also known as ground coverage.

![fov-triangle1](https://csil-git2.cs.surrey.sfu.ca/uploads/Guardian/system-docs/581f31e664/fov-triangle1.png)

A right triangle can be made with the altitude and half of the dimension's angle of view. The base of the triangle is then half of the dimension's ground coverage.

![fov-triangle2](https://csil-git2.cs.surrey.sfu.ca/uploads/Guardian/system-docs/5af20cf3c3/fov-triangle2.png)

The ground coverage `d` can be determined by the following equation:

`tan(v/2) = d/2a`  
`a * tan(v/2) = d/2`  
`d = 2 * a * tan(v/2)`  

An example lens with angles of view of 56.3 degrees horizontal and 43.7 degrees vertical then has the following ground coverage:  
  
Horizontal coverage: `d = 1.070 * a`  
Vertical coverage: `d = 0.802 * a`

At an altitude of 100 metres, the ground coverage in a photo would be `1.070 * 100` = 107 metres by `0.802 * 100` = 80.2 metres.

#### Spatial Resolution

The smallest resolvable detail in an image is determined by the pixel resolution of the camera and the distance to the subject. This size of this smallest detail is called the spatial resolution.

`spatial resolution = real-world length / pixels covering this length`

In the above case, the camera at 100 metres has a horizontal (across image width) coverage of 107 metres and a vertical (across image height) coverage of 80.2 metres. A Point Grey Chameleon3 USB3 camera has a pixel resolution of 2448 by 2048. Applying the above formula:

`horizontal resolution = 107 metres / 2448 pixels = 4.371 cm/px`  
`vertical resolution = 80.2 metres / 2048 pixels = 3.916 cm/px`

#### Determining an Appropriate Capture Rate

Effective image compositing relies on having enough overlap between two successive images, typically about 30%. This allows compositing or stitching software to detect similar features in the images in order to line them up correctly.

The chosen image capture rate depends on the desired overlap `o` as well as the ground coverage `d`. An overlap of 30% gives `o = 0.3`. Since the airplane is moving perpendicular to the camera's horizontal dimension, we can assume that overlap will generally only occur in one dimension. For an overlap factor of 0.3, we will need to capture an image when the camera has moved a distance of `1 - 0.3 =` 0.7 times the vertical coverage.

At an altitude of 100 metres, the vertical coverage with the camera above will be 80.2 metres. We should take a photo every time we pass the threshold `t = 0.7 * 80.2 = ` 56.14 metres. With a typical cruise speed of 20 m/s for the Hugin UAV, this results in a capture interval of `threshold/velocity = 56.14/20 = ` 2.8 seconds or `1/2.8 =` 0.36 frames per second.

The general formula for this camera mounted in landscape orientation relative to the direction of flight is:

`capture interval = ((1 - overlap factor) * vertical coverage) / velocity`  
`= capture interval = ((1 - overlap factor) * 0.802 * altitude) / velocity`

The distance travelled can be calculated using the Haversine formula, given the last GPS coordinates and current GPS coordinates. If the distance is greater than the threshold `t`, then a photo should be taken. As a safeguard, a photo should be taken if one hasn't been taken in several seconds. This will protect against a GPS anomaly that prevents the distance from updating correctly. The system should be prevented from taking photos excessively quickly by requiring that the capture have occurred at least half a second ago (or shorter if intentionally capturing at a high rate) before capturing another.

#### Image Capture

Custom software running on the ODROID XU4 single-board computer onboard the UAV controls the Point Grey Chameleon3 machine vision camera. A program written in C++ utilizes Point Grey's FlyCapture SDK to control the camera by sending it trigger commands over a GPIO pin on the XU4. It is also responsible for configuring the camera resolution, image encoding, shutter speed, and other options. We detect that a photo has been taken when the camera sends a signal on a GPIO pin to the XU4. This allows us to pinpoint the exact moment of image capture, which is important for attaining highly-accurate geotagging.

An image processing script written in Python monitors image capture and receives raw image data from the camera over Linux IO streams. It uses ImageMagick to convert the uncompressed raw image to JPG to achieve small file size while maintaining high quality. Converted images are saved to disk and passed to the network module in this script to be transported to the ground wirelessly.

#### Image Transmission