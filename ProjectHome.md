## Introduction ##

The Arduino Due uses an ARM Cortex M3 processor made by Atmel (SAM3X8E).  This microcontroller is fairly different from the normal AVR microcontrollers used for most Arduinos.  Alex Leone made an Arduino library for handling [Texas Instrument's TLC5940 16 Channel LED PWM driver](http://www.ti.com/product/tlc5940) with the AVR microcontrollers - https://code.google.com/p/tlc5940arduino/.

However, running the TLC5940 well requires delving into lower-level parts of the hardware -- things like timer counters and interrupts -- and these are different on the ARM microcontroller.  I've started a TLC5940 library that makes use of the features offered by the ARM processor.  At least for now, this library doesn't offer all the fancy features that Alex Leone's library (e.g. animations, fades, PROGMEM storage, etc.) because I didn't need those features.  I have my own libraries for describing and running lightshows.

## Initial Setup ##

This library is slightly annoying to use at present since, unlike most libraries, you actually have to copy the files into your sketch's folder.  Unless I write a general purpose library providing hooks to the Arduino Due's timer counter interrupts, the least annoying way to define the interrupt needed by this library in a way that can be configured is to use **#define** statements (since the name of the interrupt handler must be known at compile time).  Without editing the Arduino makefiles, the simplest way to ensure that **#define** values are seen across all the needed code files is to **#include** a configuration file in the various code files.  But this config file needs to be able to be tailored differently for different sketches, which leads to different copies of this library in each sketch that uses it.  /sigh

Anyway, enough with the ranting and onto the setup!

  * Copy all the files in this library into your sketch's folder.  (You don't need to include those in the **example** folder.)

  * Edit the parameters in **due\_tlc5940\_config.h**.

  * **#include "due\_tlc5940.h"** in your sketch.


---


## Usage ##

### Initialization: ###
Call **initTLC5940()** somewhere early in your sketch (in your **setup** function, for example).

### Grayscale Values: ###
You can manipulate grayscale values for TLC5940 output channels with the following functions.  The grayscale values range from 0 (completely off) through 4095 (completely on).
  * **setGSData (_outputChannel_, _value_)**
  * **setAllGSData (_value_)**
  * **getGSData(_outputChannel_)**

Grayscale data is not sent to the TLC5940 until you call **sendGSData()**.

### Dot-Correction: ###
The following functions are used for dot-correction values.  The dot-correction values range from 0 (no output is allowed on that channel) through 63 (maxmimum output is allowed).
  * **setDCData(_outputChannel_, _value_)**
  * **getDCData(_outputChannel_)**

Send the dot-correction data to the TLC5940 with **sendDCData()**.


---
