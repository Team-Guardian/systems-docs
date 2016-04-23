Command arguments
- --pixelformat
   - RGB8
   - YUV444
- --width
- --height
- --imageformat
   - jpeg
   - raw
- --imagegrab
   - save
   - stdout
- --regulargrabmode
   - stdin
   - gpio
- --irgrabmode
   - stdin
   - gpio
   - software
- example: --pixelformat RGB8 --width 2448 --height 2048 --imageformat jpeg --imagegrab save --regulargrabmode gpio --irgrabmode software

Input commands
- trigger/triggerir will initiate a software trigger which will grab an image and push it into the image     buffer
- grab/grabir will retrieve an image and save as jpeg or send to stdout based on command argument
- exposure/exposureir x.xx will set the exposure time to x.xx ms
- compensation/compensationir will set the exposure compensation to x.xx%
- exit will stop camera streaming, disconnect them and exit the program

Stdout output format after every grab
- ImageName,exposuretime,exposurecomp,wb