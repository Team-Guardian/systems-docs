# Georeferencing

## Existing Method

This method was devised by Irene in 2013. This is an explanation of how it works.

### Purpose

Given camera distortion information (intrinsic matrix and k-values), camera GPS location, and camera orientation, a GPS location can be calculated for a requested pixel location in an image produced by this physical camera at this location and orientation. Site elevation is not taken into account and this may produce different results at different flying sites.

### Input

* Latitude
* Longitude
* Altitude
* Pitch
* Roll
* Yaw
* Pixel X (column)
* Pixel Y (row)

### Output

* Latitude
* Longitude

### Procedure

Coordinates need to be transformed from geodetic (latitude/longitude/altitude) to camera reference frame so that the target pixel can be turned into offsets (northing and easting) to be added to the camera position.

#### 1. Build the Earth ellipsoid and calculate the radius of curvature

```
a = 6378137 #semi-major earth axis
b = 6356752.3142 #semi-minor earth axis
f = (a-b)/a #flattening factor
e2 = 2*f - f*f #first eccentricity, squared
M = (a*(1-e2))/sqrt(1-e2*sin(lat)**2)**3 #meridian radius of curvature
N = a / sqrt(1-e2*sin(lat)**2) #prime vertical radius of curvature
```

The equatorial and polar radii of the Earth are used to construct an ellipsoid. The prime vertical radius of curvature is defined (http://clynchg3c.com/Technote/geodesy/radiigeo.pdf) as the length of a line extending down normal to the surface of the Earth at the desired latitude, ending when it intersects the polar axis. Unless the Earth is perfectly spherical, the line will not intersect the polar axis at the center of the Earth.


#### 2. Convert from geodetic frame to ECEF (Earth-centered, Earth-fixed)

```
#calculate Earth Centered Earth Fixed plane coordinates
Xe = (N + alt)*cos(lat)*cos(lon)
Ye = (N + alt)*cos(lat)*sin(lon)
Ze = (N*(1-(e2))+alt)*sin(lat)
```

The above are the standard formulas for converting from geodetic to ECEF (https://en.wikipedia.org/wiki/Geographic_coordinate_conversion#From_geodetic_to_ECEF_coordinates).


#### 3. Convert to ECEF at ground level

```
#set up origin, 0 altitude directly below the aircraft
Xo = (N + 0)*cos(lat)*cos(lon)
Yo = (N + 0)*cos(lat)*sin(lon)
Zo = (N*(1-(e2))+0)*sin(lat)
```

Set up ECEF coordinates for the ground directly below the camera, used in conversion to NED.

Note that the above calculations erroneously use 0 as the altitude, which will produce an ECEF result for sea level. Instead of 0 altitude, the site elevation should be used.


#### 4. Convert from ECEF to NED

Position_ned = Rotation_ecef_to_ned (Position_ecef - Position_ecef_ground)

Below, the rotation matrix is given by T_ned_ecef and the NED position is assigned to C.

```
# transfer GPS into nav (North East Down) frame
T_ned_ecef = numpy.array([[-sin(lat)*cos(lon), -sin(lat)*sin(lon), cos(lat)], \
    [-sin(lon), cos(lon), 0], \
    [-cos(lat)*cos(lon), -cos(lat)*sin(lon), -sin(lat)]])
C = numpy.dot(T_ned_ecef, (numpy.array([[Xe], [Ye], [Ze]]) - numpy.array([[Xo], [Yo], [Zo]])))
#Xned = Pned[0]
#Yned = Pned[1]
#Zned = Pned[2]
```

#### 5. Create combined rotation matrix from NED to camera frame

```
# transformation between body frame and nav frame
T_ned_body = numpy.array([[cos(pitch)*cos(yaw), -cos(roll)*sin(yaw)+sin(roll)*sin(pitch)*cos(yaw), sin(roll)*sin(yaw)+cos(roll)*sin(pitch)*cos(yaw)], \
    [cos(pitch)*sin(yaw), cos(roll)*cos(yaw)+cos(roll)*sin(pitch)*sin(yaw), -sin(roll)*cos(yaw)+cos(roll)*sin(pitch)*sin(yaw)], \
    [-sin(pitch), sin(roll)*cos(pitch), cos(roll)*cos(pitch)]]) #body frame - nav frame transformation

T_body_camera = numpy.array([[0, -1, 0], \
    [1, 0, 0], \
    [0, 0, 1]]) #camera orientation (parallel to aircraft z axis)

T_ned_camera = numpy.dot(T_ned_body, T_body_camera) #rotation between camera frame - mapping frame
```

T_body_camera represents a rotation of 90 degrees in the Z axis.

#### 6. Use the camera distortion parameters and target pixel location to calculate physical offsets from the camera location

```
## Automatic Georeference ##
A = self.intrinsicMatrix

T_camera_ned = numpy.linalg.inv(T_ned_camera)
#C = numpy.array([[Xned], [Yned], [Zned]]) #position information at exposure (nav frame)
P = numpy.array([[pu], [pv], [1]]) #image frame column vector
Z = 0 #height of object in image (assumed to be 0 for simplicity)
z = numpy.dot(numpy.dot(T_ned_camera, numpy.linalg.inv(A)), P)
s = (Z-C[2])/z[2]

Xm, Ym, Zm = C + s[0]*(numpy.dot(numpy.dot(T_ned_camera, numpy.linalg.inv(A)), P))

##Conversion from meters of displacement to lat/lon
#print "Xm:", Xm[0], "Ym:", Ym[0], "Zm:", Zm[0] #DEBUG
```

#### 7. Calculate the target coordinates based on the calculated offsets and return them

```
dLat = self.lat + Xm[0]/111111.1
dLon = self.lon + Ym[0]/(111111.1 * cos(radians(dLat)))
return dLat, dLon, Xm, Ym
```