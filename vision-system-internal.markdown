[Odroid XU4](odroid-xu4)

[Vision system communication](vision-system-communication)

[Point Grey Chameleon3](point-grey-chameleon3)

[GUI Programming](gui-programming)

[Georeferencing](georeferencing)

## Camera Calibration

    mkdir debug
    python camera_calibrate.py --debug="debug" --square_size=0.0268 "*.jpg"

## Ground Station GUI

### Dependencies

`sudo apt-get install python-pyexiv2 python-wxgtk3.0 python-matplotlib python-pygame libblas-dev liblapack-dev libatlas-base-dev gfortran libzbar-dev g++ qt4-designer pyqt4-dev-tools`

`sudo pip install setuptools numpy simplecv scipy zbar statsmodels qimage2ndarray rpyc pyperclip`

`sh opencv_2_4_9.sh`

## Converting Point Grey RAW RGB8 format

### Method 1: ImageMagick

#### Dependencies

imagemagick (apt)

#### Procedure

`convert -size 2448x2048 -depth 8 rgb:<infile> <outfile>.jpg`

### Method 2: Wand (ImageMagick in Python)

#### Dependencies

libmagickwand-dev (apt)  
wand (pip)

#### Procedure

Read the image bytes into an array `buf` using `read(size)`. Then create the Image object.

`from wand.image import Image`  
`img = Image(blob=buf, width=2448, height=2048, depth=8, format='RGB')`

Convert it to JPG and save the file.

`img.format = 'jpeg'`  
`img.save(filename='myimage.jpg')`

### Method 3: Imagej

#### Dependencies

imagej (apt)

#### Procedure

Create a macro and save it as <name>.ijm:

```
filelist = getArgument();
file = split(filelist,':');
print("0: ", file[0]);
print("1: ", file[1]);

in = "open=/home/pi/Documents/" + file[0] + " image=[24-bit RGB] width=2448 height=2048 offset=0 number=1 gap=0";
out = "/home/pi/Documents/" + file[1];
run("Raw...", in);
saveAs("Jpeg", out);
```

Execute with:
`imagej -b <name> <infile> <outfile>`

### Method 4: RAW + Compression

#### Dependencies

pigz (apt)

#### Procedure

`tar cf - myrawfile | pigz > myrawfile.tar.gz`