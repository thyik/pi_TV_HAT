# Raspberry Pi TV HAT documentation

Getting [Started](https://www.raspberrypi.org/app/uploads/2018/10/Getting-started-with-the-Raspberry-Pi-TV-HAT.pdf)

***Steps to add new undefined frequency channel***
1. Enter the frequency of the channel (listed below for Singapore's MediaCorp FTA channels), 
2. Enter 8MHz as Bandwidth and click on Create. 
3. Repeat the steps to Add the frequencies for all the FTA channels.

## Singapore's MediaCorp Frequency Channel
* 538250 Hz 
  + Ch 5 (HD)
  + Suria (HD)
* 554000 Hz 
  + Ch 8 (HD)
  + Vasantham (HD)
* 570000 Hz 
  + Channel News Asia (HD)
  + Ch U (SD)
  + okto* (HD)
* 586000 Hz 
  + Ch U (HD)

## References
[raspberry pi pinout](https://pinout.xyz/#)

***TV HAT pinout (using SPI - Serial Peripheral Interface)***
* 5V - pin#2, pin#4;
* 3V3 - pin#1, pin#17 (not connected);
* GND - pin#6, pin#9, pin#14, pin#20, pin#25, pin#30, pin#34 pin,#39;
* SPI MOSI - pin#19;
* SPI MISO - pin#21;
* SPI SCLK - pin#23;
* SPI SC0 - pin#24;
* ID_SD - pin#27;
* ID_SC - pin#28.

[Sony CXD2880](https://elinux.org/images/2/2f/ELCE2018-poster-Sony-rpi-cxd2880.pdf)

Connecting Multiple Devices [SPI] (http://www.learningaboutelectronics.com/Articles/Multiple-SPI-devices-to-an-arduino-microcontroller.php)