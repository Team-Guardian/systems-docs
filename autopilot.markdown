## Autopilot

### Objectives

The goal of the autopilot is to keep the aircraft in straight and level flight for the entire duration of autopilot control. It should respond to disturbances and apply the necessary control movements to keep the aircraft stable and on course. It should execute coordinated turns to prevent adverse yaw causing sideways forces on the airframe. It should safely climb and descend on demand and adhere to the specified flight plan.

### Flight Planning

Flight paths can be structured in many different ways. In general, an aerial survey should aspire to limit the number and frequency of turns made by the aircraft, in order to provide smooth image sequences and stay within the limits of gimbals.

The mission area at Southport Airport in Manitoba is a rectangle approximately 2.1 km x 0.5 km spanning the airport's left and right runways. This represents an area of 1.05 square km. Our approach is typically a high-altitude survey followed by a targeted low-altitude pass over points of interest. Below is an example flight path for the Point Grey Chameleon3 camera with a resolution of 2448x2048, a lens with a focal length of 12 mm, and an altitude of 200 m.

![path1](https://csil-git2.cs.surrey.sfu.ca/uploads/Guardian/system-docs/87bd0c3340/path1.png)

This full survey allows us to get an overview of the mission area and identify points of interest before devoting flight time to them, since the flight window is limited to 45 minutes during Unmanned Systems Canada competitions. Front and side overlap of 50% give us a flight that lasts 16 minutes and produces almost 300 images. The operator has approximately 3 seconds to classify each image as unimportant or as needing further review before the next image arrives. If more time is needed, images will be queued until they are reviewed. The overlap amounts can be further reduced to get an even quicker overview of the area and reduce the number of images needing to be processed.

After images have been classified and interesting ones identified, target locations can be determined. Since photos are automatically georeferenced, the operator can easily point to a pixel and determine its real GPS location. This information can be used to generate the targeted flight plan, wherein images are only taken as the aircraft approaches and leaves a target. Since there are a small number of targets to survey, we can fly significantly lower to achieve better spatial resolution in the images. This allows us to resolve small details like the components of a QR code. A detailed pass might be flown at 100 metres or 50 metres AGL. The mission objectives that can be accomplished in these low passes are:

* Crop areas
 * Shape
 * Centroid geolocation
 * Physical area
 * QR code decoding (crop type)
 * IR detection (crop health)
 * Drop target identification (crop containing P symbol)