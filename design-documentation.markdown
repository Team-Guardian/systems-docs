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

A camera with a horizontal angle of view `h` and a vertical angle of view `v` pointing at the ground at an altitude `a` makes a triangle with the ground for each of these dimensions, as in the image below. The segment `d` represents the distance or length of the visible ground from one end the photo to the other.

![fov-triangle1](https://csil-git2.cs.surrey.sfu.ca/uploads/Guardian/system-docs/581f31e664/fov-triangle1.png)

A right triangle can be made with half of the dimension's angle of view. The base of the triangle is then half of the dimension's visible ground distance.

![fov-triangle2](https://csil-git2.cs.surrey.sfu.ca/uploads/Guardian/system-docs/5af20cf3c3/fov-triangle2.png)

The length of the segment d can be determined by the following equation:

tan(v/2) = d/2a  
a * tan(v/2) = d/2  
d = 2 * a * tan v/2  

An example lens with angles of view of 56.3 degrees horizontal and 43.7 degrees vertical then has the following visible ground area:  
  
Horizontal: d = 1.070 * a  
Vertical: d = 0.802 * a

At an altitude of 100 metres, the visible ground area in a photo would be 1.070 * 100 = 107 metres by 0.802 * 100 = 80.2 metres.