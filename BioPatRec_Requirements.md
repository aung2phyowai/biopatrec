# Requirements #

  * Matlab R2011b
    * Data Acquisition Toolbox for recordings
    * Statistics Toolbox for [Discriminant Analysis](Discriminant_Analysis.md)
    * Instrument Control Toolbox for the [VRE](VRE.md) (tcpip function)

BioPatRec was initially developed in Matlab `R2009`, however the latest version was only tested in `R2011b`. Some parts of BioPatRec might work in previous versions, but the VRE and the data acquisition routines need `R2011b`. So far, everything seems to be working with `R2014a`.

**NOTE: There is a bug with Matlab 2014b where the progress bar doesn't display properly anymore (thank you Matlab), but it works fine in R2014a.**

## Running on Mac ##

Everything seems to work currently when running a virtual machine with windows.


## Running on Linux ##

Everything seems to work currently when running a virtual machine with windows, even the data acquisition and VRE.

## Data Acquisition ##

The DAQ routines currently work with the Matlab's "Session-based Interface" (SBI). The "legacy" routines can be made available on request.

We have used the simplest National Instruments card (USB-6009) for most of our experiments. We expect any NI card to work seemly, however you won’t know until you try it. In theory, the SBI should allow you to use a wide variety of DAQ cards.

If you are planning to use the USB-6009 card, you will need the drivers [NI-DAQmx](http://joule.ni.com/nidu/cds/view/p/id/2604/lang/en)

Note: Other devices using SCI have been used as well. These routines and the "legacy" routines for MATLAB can be made available upon request.

## Virtual Reality Enviroment ##

In order to run the Virtual Reality Environment, you must use a machine running Windows. The software has been tested and checked to run successfully on Windows 7 64-bit machines.

In order to run the software, the computer must be compatible with OpenGL rendering. Any NVidia and ATI card can be used, and most SiS, Intel and S3 cards are compatible. To make sure if it is compatible, check with your graphic card vendor to make sure it supports **OpenGL 1.2.1**.

Other hardware, such as CPU, has not been thoroughly tested in order to say the level of CPU required to run this application. The system has been tested and works with:
  * Intel Core i7 2,8 GHz
  * Intel Core i5 2,4 GHz
  * Intel Core i3 2,1 GHz

without showing any signs of reduced performance.

Using hardware with non-adequate performance will result in slower performance in rendering and may provide latency in all systems associated with the VRE.