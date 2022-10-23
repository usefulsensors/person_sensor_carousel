# Person Sensor Screen Locker
Orient a carousel to the nearest face using a Person Sensor.

## Introduction

The [Person Sensor](https://usfl.ink/ps) from [Useful Sensors](https://usefulsensors.com)
is a small, low-cost hardware module that detects nearby peoples’ faces, and
returns information about how many there are, where they are relative to the
device, and performs facial recognition. It is designed to be used as an input
to a larger system, and this example shows how to use it to control a
servo-driven carousel so that it always points towards the nearest face.

## BoM

To build this project you'll need:

 - [Raspberry Pi Pico H](https://www.adafruit.com/product/5056).
 - [Person Sensor from Useful Sensors](https://usfl.ink/ps).
 - [Qwiic connector cable with female jumpers](https://www.sparkfun.com/products/17261).
 - [LynxMotion Base Rotate Kit](https://www.robotshop.com/en/lynxmotion-base-rotate-kit-no-servo.html).
 - [Hitec HS-422 Servo Motor](https://www.robotshop.com/en/hitec-hs-422-servo-motor.html).
 - [3 Male to female jumper wires](https://www.sparkfun.com/products/9385).
 - Small Phillips screwdriver.
 - Blu Tack or something similar to hold the sensor in place.

No soldering is required. You should be able to use any servo and housing as
alternatives to the exact models listed here, these were just chosen because
they are easy to assemble. You can also use a regular Pico and solder on the
headers yourself if you don't have a Pico H.

## Assembling

### Carousel

- Unscrew the default horn from the servo if one is attached.
- Screw the servo into the carousel base.
- Place the ball bearing into the depressions in the top of the base.
- Connect the rotating plate onto the servo shaft, and screw it in.
- Plug the three M/F jumper wires into the servo cable.
- Wire them into the right pins on the Pico:
    - Black (GND) should go to pin 3.
    - Red (5V) should go to pin 40.
    - Yellow (DATA) should go to pin 1.

Plug one end of the Qwiic cable into the Person Sensor, and the other into the
Trinkey. They each only have a single port, and Qwiic connectors can only be
attached one way, so it should hopefully be straightforward.

## Install CircuitPython

Hold down the `bootsel` button and plug the Trinkey into a USB port of your
desktop or laptop machine. You should see a drive called `RPI-RP2` appear in
your file system.

There's a [step by step guide to installing CircuitPython on a Trinkey](https://learn.adafruit.com/adafruit-trinkey-qt2040/circuitpython)
but the summary is that you download [CircuitPython for the Trinkey](https://circuitpython.org/board/adafruit_qt2040_trinkey/),
and copy it onto the `RPI-RP2` drive. Once the copying has completed, you should
see a new `CIRCUITPYTHON` drive appear instead.

## Install Libraries

The application works by emulating a keyboard and sending keypresses to the main
machine to lock the screen, or minimize the main window. We need the [adafruit_hid library](https://docs.circuitpython.org/projects/hid/en/latest/)
to do the emulation, so the first step is to download a big bundle of all the
CircuitPython libraries from [circuitpython.org/libraries](https://circuitpython.org/libraries). You'll need to find the right bundle for your CircuitPython version.

Once you have that downloaded, unpack the bundle on your local machine. In the
file viewer, go to the `lib` folder within the unpacked bundle and copy the
`adafruit_hid` directory into the `lib` folder on the `CIRCUITPYTHON` drive.
This should install the library we need to emulate a keyboard.

## Customize for your OS

If you're on Windows, the code in this repository should work with no changes.
On MacOS or Linux you'll need to modify what keys are sent to cause the screen
to lock. If you look at the end of `code.py`, you should see commented-out
options for the different operating systems.

## Install the Code

Now your Trinkey is set up, copy the `code.py` file from this repository into
the `CIRCUITPYTHON` drive. You should notice that the green LED on the Person
Sensor lights up when it sees your face. If you point it away from yourself for
more than five seconds, you should find that the screen locks! It will also
attempt to minimize the current window if it detects somebody looking over your
shoulder for more than a second.

The sensor needs to be the right way up, with the connector at the top, and
pointing towards you. If you have a long enough Qwiic cable, you can try
mounting it on the top of your laptop screen.

## Next Steps

The timings for the screen locking and window minimization are controlled by the
`MAIN_FACE_TIMEOUT_SECONDS` and `LOOKIE_LOO_TIMEOUT_SECONDS` values in the code.
You can also change how large the face needs to be to be counted with the
`MAIN_FACE_MIN_WIDTH` and `MAIN_FACE_MIN_HEIGHT` variables.

This project is intended as an example of what's possible when you can access
ML capabilities as simply as any other hardware component. There's also an
Arduino version available at [github.com/robotastic/useful-autolock](https://github.com/robotastic/useful-autolock),
and you can find out more about the Person Sensor from our [full developer guide](https://usfl.ink/ps_dev).
We're looking forward to seeing what the creative minds of the maker community
come up with, so please let us know how you get on!