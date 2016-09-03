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

Build the Earth spheroid and calculate the radius of curvature

```
a = 6378137 #semi-major earth axis
b = 6356752.3142 #semi-minor earth axis
f = (a-b)/a #flattening factor
e2 = 2*f - f*f #first eccentricity, squared
M = (a*(1-e2))/sqrt(1-e2*sin(lat)**2)**3 #meridian radius of curvature
N = a / sqrt(1-e2*sin(lat)**2) #prime vertical radius of curvature
```

Convert from geodetic frame to ECEF (Earth-centered, Earth-fixed)

```
#calculate Earth Centered Earth Fixed plane coordinates
Xe = (N + alt)*cos(lat)*cos(lon)
Ye = (N + alt)*cos(lat)*sin(lon)
Ze = (N*(1-(e2))+alt)*sin(lat)
```

Convert to ECEF at ground level

```
#set up origin, 0 altitude directly below the aircraft
Xo = (N + 0)*cos(lat)*cos(lon)
Yo = (N + 0)*cos(lat)*sin(lon)
Zo = (N*(1-(e2))+0)*sin(lat)
```
Note that the above calculations erroneously use 0 as the altitude, which will produce an ECEF result for sea level. Instead of 0 altitude, the site elevation should be used.


Convert from ECEF to NED using the formula:

P_n = R_n/e (P_e - P_eo)

i.e. position in NED is given by performing a rotation from ECEF to NED on the difference between the ECEF position and the ECEF ground (origin) position.

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
