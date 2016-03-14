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