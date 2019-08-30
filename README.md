# Building-Gstreamer-Raspberry-Pi-With-SRT-Support
This sh script and some tips you can find in the README file will help you obtaining a running installation of gstreamer with SRT support on you Raspberry PI (Raspbian 10 Buster)

## Why?
I struggled over a month and wasted a considerable amount of time trying to get a working version of gstreamer (at this point in time the latest release is the 1.16). I followed different guides and approaches (both official or user-written) but none of them worked standalone. 

This guide is intended for very beginners like me and for myself because maybe I will need gstreamer with SRT support again and I don't wont to foret how to do it.

## Contributing
I'm putting this a the beginning because there are 99% chance I will not need gstreamer in my life for a lot of time but it would be graat that people facing new problems (with new Raspbian releases or new gstreamer releases) will maintain this guide updated => feel free to commit and fork!

## Let's start

### SRT 
First of all we need to download, compile and install SRT libraries.

### Gstreamer

### Raspicam Src
I found [This plugin for natively using the raspicam inside a gstreamer pipeline](https://github.com/thaytan/gst-rpicamsrc) extremely usefull.

Here's how to install it:
```bash
git clone https://github.com/thaytan/gst-rpicamsrc
cd gst-rpicamsrc
./autogen.sh --prefix=/usr --libdir=/usr/local/bin
make
sudo make install
```

### Usage example
```bash
gst-launch-1.0 -v rpicamsrc preview=true sensor-mode=5 ! video/x-h264,width=1640,height=922,framerate=40/1 ! mpegtsmux ! srtsink uri=srt://:8888
```
