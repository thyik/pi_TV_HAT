# Building OpenMAX encoder for transcoding

What you'll need

First we need to install all of the dependencies required to compile FFMpeg, as well as the standard tools for compiling programs (gcc, automake, etc.)
```
sudo apt-get update
sudo apt-get install autoconf automake build-essential libass-dev libfreetype6-dev \
libsdl1.2-dev libtheora-dev libtool libva-dev libvdpau-dev libvorbis-dev libxcb1-dev libxcb-shm0-dev \
libxcb-xfixes0-dev pkg-config texinfo zlib1g-dev

```

Once that's done, we're ready to pull the latest FFMpeg from the git repository:

```
cd ~
git clone https://github.com/ffmpeg/FFMpeg --depth 1
```


```
cd ~/FFMpeg
./configure --enable-gpl --enable-nonfree --enable-mmal --enable-omx --enable-omx-rpi

```
Once that's done, you should now have the FFMpeg sources in your ~/FFMpeg folder.


Now we're ready to compile.

```
make -j4 
```
The -j4 tells the compiler to use all 4 cores of the RPi2/RPi3 which speeds up compilation considerably. With an RPi2, expect to wait about 15-20 minutes for the compile to complete. With an RPi3, the process will be quicker.


Once the process is done, you should have a working version of FFMpeg, complete with OMX encoding support. Double check that it's enabled properly by typing:

```
./ffmpeg -encoders | grep h264_omx
```

If it worked, you should see:

```
V..... h264_omx             OpenMAX IL H.264 video encoder (codec h264)

```

At this point you can install FFMpeg on your system if you would like by typing:

```
make install
```

Here's an example command line, for those of you not familiar with FFMpeg encoding:

```
./ffmpeg -c:v h264_mmal -i <inputfile.mp4> -c:v h264_omx -c:a copy -b:v 1500k <outputfile.mp4>
```

The first `-c:v h264_mmal` tells FFMpeg to use h264_mmal hardware accelerated decoder

The second `-c:v h264_omx` tells FFMpeg to use the h264_omx encoder

The `-c:a` copy tells FFMpeg to simply copy any audio tracks without transcoding

The `-b:v` tells FFMpeg to set the average video bitrate target to 1500Kbit. You can set this to whatever you want to get the desired file size.

You can type ffmpeg -h full to get a complete list of commands, it's quite extensive. You can also check the ffmpeg man page.

When you run the command, open another window and run top, you'll see that the CPU usage is very low, around 45% or so, telling you that the RPi is using hardware acceleration.

Some things to keep in mind:
1. This encoder is VERY basic, it does not include all of the bells and whistles that libx264 has, you're basically able to scale the video and lower or increase the bitrate, that's pretty much it.
2. To my knowledge, there's no GUI program that supports this feature, so you're stuck encoding on the command line.
3. The use of ANY kind of scaling or filters will drastically slow down the encode because it uses the RPi's CPU.

## Setup Tvhendend transcoding

1. Install Raspbian Jessie (not Raspbian Stretch!) and update the system.
2. Install Tvheadend [v4.3](https://tvheadend.org/projects/tvheadend/wiki/AptRepositories#fn3) (unstable) as described here:
3. Configure Tvheadend, and test viewing your air TV channels using default MPEG-TS Pass-through "pass" stream profile.
4. Download and compile [OpenMAX-enabled FFmpeg](https://www.reddit.com/r/raspberry_pi/comments/5677qw/hardware_accelerated_x264_encoding_with_ffmpeg):
5. (not necessary!). Run the following command and make sure you can see the test picture. This means that hardware acceleration works properly: 
```
$ gst-launch-1.0 -v videotestsrc ! omxh264enc ! h264parse ! omx264dec ! glimagesink
```

6. Create the new streaming profile in Tvheadend web interface (Configuration->Stream->Stream Profiles) using "MPEG-TS Spawn/ built-in" type with the command line: 

```
$ ffmpeg -i - -f mpegts -c:a copy -c:v h264_omx -b:v 368k pipe:1
```

This will generate the stream with the bitrate about 500-600 kbit/s instead of several Mbit/s using GPU hardware transcoding.
During the transcoding CPU usage will not be higher than 40-60% comparing to 150% in case if you are using any other software transcoding. CPU temperature will not raise above 60-65°C.


## To build in background

1. instal Tmux `sudo apt-get install tmux`
2. run `tmux`
3. execute `make -j4`
4. press `Ctrl-b` then `d` to detach
5. to attach again, type `tmux attach`
