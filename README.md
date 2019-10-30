# Building-Gstreamer-Raspberry-Pi-With-SRT-Support
This sh script and some tips you can find in the README file will help you obtaining a running installation of gstreamer with SRT support on you Raspberry PI (Raspbian 10 Buster)

## Why?
I struggled over a month and wasted a considerable amount of time trying to get a working version of gstreamer (at this point in time the latest release is the 1.16). I followed different guides and approaches (both official or user-written) but none of them worked standalone. 

This guide is intended for very beginners like me and for myself because maybe I will need gstreamer with SRT support again and I don't wont to forget how to do it.

## Contributing
I'm putting this a the beginning because there are 99% chance I will not need gstreamer in my life for a lot of time but it would be graat that people facing new problems (with new Raspbian releases or new gstreamer releases) will maintain this guide updated => feel free to commit and fork!

## Let's start

### Setting up the RPI
Please run `sudo raspi-config` and go into 
```diff
- 5 Interfacing Options
```
and make sure to enable the camera. A reboot is recommended right after.

### SRT 
The project can be found here: [SRT Project from GitHub](https://github.com/Haivision/srt).
```bash
sudo apt-get install tclsh pkg-config cmake libssl-dev build-essential
git clone https://github.com/Haivision/srt
cd srt
./configure
make
sudo make install
```

### Gstreamer
Download and run the .sh script you can find in this repo and everything should run smoothly. It will take a lot of time but don't worry :) Just don't waste your time in front of your PI because it will seriously take a lot of time!

At the moment of writing the latest gStreamer release is 1.16. You can change the release version you want to build just changing it on line 4 of the .sh script.

Super noob tip: after downloading the sh file make it executable using
```bash
chmod u+x gstreamer_build.sh
```
**Please note: This will not build anything under gst-libav because I'm getting an error and I really don't need it so i just skipped it**

### Raspicam Src
I found [this plugin for natively using the raspicam inside a gstreamer pipeline](https://github.com/thaytan/gst-rpicamsrc) extremely usefull.

Here's how to install it:
```bash
git clone https://github.com/thaytan/gst-rpicamsrc
cd gst-rpicamsrc
./autogen.sh --prefix=/usr --libdir=/usr/local/bin
make
sudo make install
```
Please not the choice of libdir: we're placing them in the exact same folder of where we placed gstreamer libraries.

## Usage example
On the RPI:
```bash
gst-launch-1.0 -v rpicamsrc preview=true sensor-mode=5 bitrate=8000000! video/x-h264,width=1640,height=922,framerate=40/1,profile=baseline ! mpegtsmux ! srtsink uri=srt://:8888
```
On the receiver you can either:
### Use VLC
You can find the stream at srt://**YOUR RPI IP ADDRESS**:8888
### Use a gStreamer Pipeline
```bash
gst-launch-1.0 srtsrc uri=srt://192.168.1.55:8888 ! decodebin ! autovideosink sync=false
``` 
