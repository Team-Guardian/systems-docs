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

### Equations

#### Determining Visible Area in an Image

A camera with a horizontal angle of view `h` and a vertical angle of view `v` pointing at the ground at an altitude `a` makes a triangle with the ground for each of these dimensions, as in the image below. The segment `d` represents the distance or length of the visible ground from one end of the photo to the other. This is also known as ground coverage.

![fov-triangle1](https://csil-git2.cs.surrey.sfu.ca/uploads/Guardian/system-docs/581f31e664/fov-triangle1.png)

A right triangle can be made with the altitude and half of the dimension's angle of view. The base of the triangle is then half of the dimension's ground coverage.

![fov-triangle2](https://csil-git2.cs.surrey.sfu.ca/uploads/Guardian/system-docs/5af20cf3c3/fov-triangle2.png)

The ground coverage `d` can be determined by the following equation:

tan(v/2) = d/2a  
a * tan(v/2) = d/2  
d = 2 * a * tan(v/2)  

An example lens with angles of view of 56.3 degrees horizontal and 43.7 degrees vertical then has the following ground coverage:  
  
Horizontal: d = 1.070 * a  
Vertical: d = 0.802 * a

At an altitude of 100 metres, the ground coverage in a photo would be 1.070 * 100 = 107 metres by 0.802 * 100 = 80.2 metres.

#### Spatial Resolution

The smallest resolvable detail in an image is determined by the pixel resolution of the camera and the distance to the subject. This size of this smallest detail is called the spatial resolution.

spatial resolution = real-world length / pixels covering this length

In the above case, the camera at 100 metres has a horizontal (across image width) coverage of 107 metres and a vertical (across image height) coverage of 80.2 metres. A Point Grey Chameleon3 USB3 camera has a pixel resolution of 2448 by 2048. Applying the above formula:

horizontal resolution = 107 metres / 2448 pixels = 4.371 cm/px  
vertical resolution = 80.2 metres / 2048 pixels = 3.916 cm/px