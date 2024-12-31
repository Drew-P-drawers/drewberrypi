# Motion IP Camera to Monitor 3D Prints

I got this working using [Motion](https://motion-project.github.io/) to make sure my 3D prints weren't running amuck on my old Creality Ender 3 PRO. It's got some issues.

![gif](/assets/images/ip-webcam-3d-printer.gif)

I initially discovered and set up motion using this guide:
https://pimylifeup.com/raspberry-pi-webcam-server/

Unfortunately, the guide was a bit out of date. I shouldn't have just blindly followed it, and instead spent a long night reading through the official [Motion documentation](https://motion-project.github.io/motion_guide.html) because I was only getting around 1 frame per second on the live feed. I also was only seeing a grey box for the longest time.

**RTFM**

I'll update the guide with all of my learnings, frustrations, etc. (At some point... MAYBE)

Quick setup based on what I did for 1080p, high framerate, and a stream I can view on any of my local network devices from the logitech c920 webcam on my Raspberry Pi 5.
This will also have Motion run as a service, meaning it will start automatically whenever the Raspberry Pi 5 is booted on. (Don't worry, you can turn it off.) 

"When running as a service, Motion runs as the user motion and this user is automatically added to the user group of video. By only be included in this user group, when Motion is run, it will only have limited permissions. It is NOT recommended that this user get added to the sudo group. Instead, only add this user to groups that are specifically required." - [https://motion-project.github.io/4.6.0/motion_config.html](https://motion-project.github.io/4.6.0/motion_config.html)

## Guide

### Install motion and required libraries
If you just want to copy and paste, and have similar equipment to mine, this should work for you. Why it works? I'll update that later haha. 

But enjoy.

Anyways, I always run the update commands before installing anything new for peace of mind.

```
sudo apt update
sudo apt upgrade
```

Install required libraries for motion

```
sudo apt install autoconf automake build-essential pkgconf libtool git libzip-dev libjpeg-dev gettext libmicrohttpd-dev libavformat-dev libavcodec-dev libavutil-dev libswscale-dev libavdevice-dev default-libmysqlclient-dev libpq-dev libsqlite3-dev libwebp-dev libcamera-v4l2
```

Install motion

```
sudo wget https://github.com/Motion-Project/motion/releases/download/release-4.6.0/$(lsb_release -cs)_motion_4.6.0-1_$(dpkg --print-architecture).deb

sudo dpkg -i $(lsb_release -cs)_motion_4.6.0-1_$(dpkg --print-architecture).deb
```

Command to edit the config

```
sudo nano /etc/motion/motion.conf
```
After reading through the documentation, this is how I configured motion using the `motion.conf` file. 

Parameters I changed:

```
daemon on > off
width 640 > 1920
height 340 > 1080
pause on
minimum_motion_frames 4 > 1
framerate 30 > 100
webcontrol_localhost on > off
stream_localhost on > off
stream_maxrate 1 > 50
movie_output on > off
```

The biggest helpers were `stream_maxrate`, `pause`, and `movie_output`

Turning `movie_output` off made it so I could see the livestream, instead of just a grey box.
  My general thinking, I was sitting in front of the camera, moving. So detecting motion as soon as I turned it on, made it so that the device was "busy" and couldn't kickstart the livestream process. That's my assumption anyways

Turning up `stream_maxrate` enabled a better FPS on the livestream. 1FPS is poop.

I didn't need it to be a motion detecting camera that logged videos everytime it saw something. Your uses may vary, but I preferred saving cpu resources by using `pause` to disable the feature.

If you just want to overwrite the default configuration, you can find mine with these updated parameters here:
[configuration file](assets/motion.conf).

Once you have my version downloaded, navigate to wherever you saved it using the CLI. (maybe `/home/pi/Downloads`)

```
cd /home/pi/Downloads
```
This will most likely work. If not, try adding `sudo` before the command to elevate the permissions.

```
mv motion.conf -force /etc/motion/
```
You can find the preview of my updated `motion.conf` at the bottom of this guide, so you can see the context. (Or visit the file silly)

You should probably reboot your device at this point. 

```
sudo reboot
```

You'll notice the led on the camera turn on, that means it should be working. Time to find out.

## Find the local stream

If you take a look at the configuration file, we'll be streaming to port 8081

```
# The port number for the live stream.
stream_port 8081
```

But you need to figure out what to do with it. Simply put, motion will be generating a hosted local live stream that you can access by slapping your local ip address with the host number.

Let's find the IP address of your raspberry pi.

```
ifconfig
```

Should be under `wlan0` if you're using wifi and it's followed by `inet`.

Like I said, slap the ip address and port together with a `:` in your browser. Should work on any device in your network, including your phone.

```
http://192.***.*.***:8081/
```

Something else I discovered, you can visit port 8080 to control the camera if you'd like.

```
http://192.***.*.***:8080/
```

Enjoy!

---
**NOTE**

I use [Termius](https://termius.com/) to SSH into my Raspberry Pi in case I need to change anything from my phone.

---

## Useful Motion commands

To start Motion:

```
sudo systemctl start motion
```

To stop Motion:

```
sudo systemctl stop motion
```

To restart Motion:

```
sudo systemctl restart motion
```

If you don't want Motion to start everytime you boot your device, gotta edit the config.

```
daemon on
```

## TODO: Create a cloudflare tunnel to access it from anywhere
Will probably follow a guide like this: https://pimylifeup.com/raspberry-pi-cloudflare-tunnel/

### Preview of updated motion.conf

```
# Rename this distribution example file to motion.conf
#
# This config file was generated by motion 4.6.0
# Documentation:  /usr/share/doc/motion/motion_guide.html
#
# This file contains only the basic configuration options to get a
# system working.  There are many more options available.  Please
# consult the documentation for the complete list of all options.
#

############################################################
# System control configuration parameters
############################################################

# Start in daemon (background) mode and release terminal.
daemon off

# Start in Setup-Mode, daemon disabled.
setup_mode off

# File to store the process ID.
; pid_file value

# File to write logs messages into.  If not defined stderr and syslog is used.
; log_file value

# Level of log messages [1..9] (EMG, ALR, CRT, ERR, WRN, NTC, INF, DBG, ALL).
log_level 6

# Target directory for pictures, snapshots and movies
; target_dir value

# Video device (e.g. /dev/video0) to be used for capturing.
video_device /dev/video0

# Parameters to control video device.  See motion_guide.html
; video_params value

# The full URL of the network camera stream.
; netcam_url value

# Name of mmal camera (e.g. vc.ril.camera for pi camera).
; mmalcam_name value

# Camera control parameters (see raspivid/raspistill tool documentation)
; mmalcam_params value

############################################################
# Image Processing configuration parameters
############################################################

# Image width in pixels.
width 1920

# Image height in pixels.
height 1080

# Maximum number of frames to be captured per second.
framerate 100

# Text to be overlayed in the lower left corner of images
text_left CAMERA1

# Text to be overlayed in the lower right corner of images.
text_right %Y-%m-%d\n%T-%q

############################################################
# Motion detection configuration parameters
############################################################

# When Motion is started, pause the motion detection.
pause on

# Always save pictures and movies even if there was no motion.
emulate_motion off

# Threshold for number of changed pixels that triggers motion.
threshold 100

# Noise threshold for the motion detection.
; noise_level 4

# Despeckle the image using (E/e)rode or (D/d)ilate or (l)abel.
despeckle_filter

# Number of images that must contain motion to trigger an event.
minimum_motion_frames 1

# Gap in seconds of no motion detected that triggers the end of an event.
event_gap 60

# The number of pre-captured (buffered) pictures from before motion.
pre_capture 0

# Number of frames to capture after motion is no longer detected.
post_capture 0

############################################################
# Script execution configuration parameters
############################################################

# Command to be executed when an event starts.
; on_event_start value

# Command to be executed when an event ends.
; on_event_end value

# Command to be executed when a movie file is closed.
; on_movie_end value

############################################################
# Picture output configuration parameters
############################################################

# Output pictures when motion is detected
picture_output off

# File name(without extension) for pictures relative to target directory
picture_filename %Y%m%d%H%M%S-%q

############################################################
# Movie output configuration parameters
############################################################

# Create movies of motion events.
movie_output off

# Maximum length of movie in seconds.
movie_max_time 60

# The encoding quality of the movie. (0=use bitrate. 1=worst quality, 100=best)
movie_quality 45

# Container/Codec to used for the movie. See motion_guide.html
movie_codec mkv

# File name(without extension) for movies relative to target directory
movie_filename %t-%v-%Y%m%d%H%M%S

############################################################
# Webcontrol configuration parameters
############################################################

# Port number used for the webcontrol.
webcontrol_port 8080

# Restrict webcontrol connections to the localhost.
webcontrol_localhost off

# Type of configuration options to allow via the webcontrol.
webcontrol_parms 4

############################################################
# Live stream configuration parameters
############################################################

# The port number for the live stream.
stream_port 8081

# Restrict stream connections to the localhost.
stream_localhost off

# This parameter wasn't including in the regular config file, but was listed in the documentation. Took a long time figuring out why the video feed was only about 1fps
stream_maxrate 50

##############################################################
# Camera config files - One for each camera.
##############################################################
; camera /usr/etc/motion/camera1.conf
; camera /usr/etc/motion/camera2.conf
; camera /usr/etc/motion/camera3.conf
; camera /usr/etc/motion/camera4.conf

##############################################################
# Directory to read '.conf' files for cameras.
##############################################################
; camera_dir /usr/etc/motion/conf.d
```

