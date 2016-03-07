Board: Raspberry Pi 2 Model B (for 2016+)

Image conversion:

Point Grey RAW format

sudo apt-get install imagej

Create a macro and save it as rgb2jpg.ijm:

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
imagej -b rgb2jpg infile outfile