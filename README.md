# NXT-Python

NXT-Python is a package for controlling a LEGO NXT robot using the 
Python programming language. It can communicate using either USB or 
Bluetooth. It is available under the Gnu GPL v3 license. It is based on 
NXT_Python, where releases halted in May 2007.

*Note: I, Eelviny am not the original owner of this repo, nor do I claim to be. I'm just an NXT lover and Python tinkerer nobly saving this abandoned project from the death of Google Code... Who knows, maybe I'll try and improve on it. If Marcus or others wish to have this repo, feel free to message me.*

## Requirements:

* Python 2.6 or greater, but not 3.x (http://www.python.org)
And at least one comm library:
* Bluetooth communications:
  *Linux/Windows: [PyBluez](https://github.com/karulis/pybluez)
  *Mac: [LightBlue](http://lightblue.sourceforge.net/)
* USB communications:
  *PyUSB (http://sourceforge.net/projects/pyusb/)
* Fantom communications (tested on Mac OSX):
  *Pyfantom (http://pyfantom.ni.fr.eu.org/)

## Installation (see the [wiki](https://github.com/Eelviny/nxt-python/wiki/Installation) page):

* Untar/unzip source package.
* In package directory, run "python setup.py install" (as root), or if under windows, double-click install.bat.
* To use USB on Linux as non-superuser, at a root terminal type:
```
groupadd lego
usermod -a -G lego [username]
echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="0694", GROUP="lego", MODE="0660"' > /etc/udev/rules.d/70-lego.rules
```

# Getting Started:

Take a look at the examples directory. Feel free to copy that code 
into your scripts and don't be afraid to experiment! If you are having 
trouble with something, you may find the solution in the docstrings (for 
example, help('nxt.sensor.Ultrasonic')) or even in the source code 
(especially for digital sensors).

## Notes/FAQ:
(I have tried to put the most important stuff first, but it would be a good idea to read the whole section. In any case, read it all the way through before asking for help. Thanks!)

# About v2
This version is part of the 2.x series of releases. Programs 
designed for NXT_Python or for the 1.x series of nxt-python will not 
work with this version. If you are trying to get an old program to work, 
it most likely needs a 1.x series release, which can be downloaded from 
the nxt-python [releases](https://github.com/Eelviny/nxt-python/releases) page. New projects should use a 
2.x series release (hint: this is one!) due to the new features and API 
improvements. Converting old projects is somewhat difficult and not 
officially supported, though as always you're welcome to ask for help.

# Problems and Their Solutions
Support for a number of sensors has not been tested at all, due to 
lack of hardware. I have started a project to test this code, but the 
going is slow and I still can't test everything. If you have a problem 
with a digital sensor, see the troubleshooting guide below and don't 
forget to report your trouble!

The Synchronized Motor support has not been extensively tested for 
accuracy. It seems to mostly work well but the accuracy of the braking 
function and the closeness of the two motors to each other have not been 
given a formal scientific assessment.

NXT-Python has not been tested and may not work with custom nxt 
firmware versions (if you don't know what that means, you don't need to 
worry about it). However, if the firmware supports the standard LEGO 
USB/BT communications protocol, everything should more or less work. 
NXT-Python has been tested with bricks using LEGO firmware version up to 
1.29 and is compatible with protocol version 1.124 (used by most if not 
all of the official firmwares). It has also been reported working with 
LeJOS.

Specific Stability Status:
*nxt.brick, nxt.telegram, nxt.direct, and nxt.system have been redone somewhat as of v2.2.0 but appear to work well.
*USB Communication System (nxt.usbsock)
  *On Linux: Very stable and extensively tested.
  *On Windows: Somewhat tested; seems to work pretty well.
  *On Mac: Some users having problems.
*BlueTooth Communication System (nxt.bluesock, nxt.lightblueglue)
  *On Linux: Stable; well tested with both pybluez and lightblue.
  *On Windows: Stable; working last I checked.
  *On Mac: Some users having problems.
*Internet Communications System (nxt.ipsock) seems to work for the most part. Occasionally has hiccups.
*Fantom Communications System (nxt.fantomsock)
  *On Linux: N/A (Fantom driver not supported)
  *On Windows: Not tested.
  *On Mac: Tested, USB interface working, Bluetooth not working.
*nxt.locator: Tested working with revamped logic and new code in v2.2.0.
*nxt.motor: Stable except for Synchronized Motor support, which is experimental at this stage and has not been extensively tested.
*nxt.sensor: Code not specific to a particular sensor is well-tested and working great. More than half of the sensor classes were last reported working; the rest have not to my knowlege been tested and were written blindly from the manuacturers' specifications.
*nxt.error: If there's a problem with this one, I'm gonna cry.

## Contact:
NXT-Python's Head Developer:
Marcus Wanner (marcus@wanners.net)
The support and development mailing list:
http://groups.google.com/group/nxt-python
Report bugs and suggest new features at:
https://github.com/Eelviny/nxt-python/issues

## Thanks to:
Doug Lau for writing NXT_Python, our starting point.
rhn for creating what would become v2, making lots of smaller changes, and reviewing tons of code.
mindsensors.com (esp. Ryan Kneip) for helping out with the code for a lot of their sensors, expanding the sensors covered by the type checking database, and providing hardware for testing.
HiTechnic for providing identification information for their sensors. I note that they have now included this information in their website. ;)
Linus Atorf, Samuel Leeman-Munk, melducky, Simon Levy, Steve Castellotti, Paulo Vieira, zonedabone, migpics, TC Wan, jerradgenson, henryacev, Paul Hollensen, and anyone else I forgot for various fixes and additions.
All our users for their interest and support!

## Troubleshooting Digital Sensors (don't read unless you have problems):
If you are getting errors, strange behavor, or incorrect values from a digital
sensor, chances are that there is a bug in our code. Follow these instructions
to try and find out what's wrong:
1. Test the sensor with a different access library to make sure it's working
right.
2. Check your code again. There are some weird "features" in the interfaces
of some of the sensors; make sure you are doing things right.
3. Locate the sensor class's source code in nxt-python. It should be
somewhere in nxt/sensor/<manufacturer>.py, under the heading "class SensorName(
BaseDigitalSensor):". Read any comments for instructions on certain things.

If you get to here and are still having a problem, you can either go ahead and
report it now or continue to try and find and fix the problem and then report
it (or not report it at all, but that wouldn't be very nice...).
Python experience required beyond this point.

4. Get the sensor's specifications from the manufacturer's website. Make
sure it includes a table of I2C registers and instructions for using them.
5. Pick one of the following depending on what the problem is:

####Errors:
Cause: We screwed up.
Solution: Check the line mentioned in the error for incorrect syntax or
other problem. A bit of python experience and maybe some googling is needed
here.

####Strange Behavior (in sensors with modes/commands):
Cause: nxt-python's command enumerations are incorrect.
Solution: Verify them using the sensor's specs, and correct any problems.
See "Incorrect Values" for more.

####Incorrect Values:
Cause: nxt-python is processing the value wrong.
Solution: Check what goes on in the sampling method against what the spec
says should be done. If there is an inconsistency, try to fix it.

Cause: nxt-python has an incorrect register number or type in I2C_ADDRESS.
Solution: Verify the address (the number) and the string (the struct format
string). To verify the address, use the spec. To verify the struct format, you
will need to read this: <http://docs.python.org/library/struct.html#format-
strings> or have experience with struct.

Read the spec for the sensor to determine how the given value should be read,
then start at the sample method and read through it, checking for problems as
you go. If it seems right, go back to the I2C_ADDRESS chunk (near the top of the
class) and make sure that the correct struct format string is being used. The
most common problem here is values that are off by plus or minus 128 or 32768
because of an incorrect signed/unsigned setting. This can be fixed by switching
the case (as in upper or lower) of the letter in the string. Other problems
could include the wrong size (B, H, or L) being used, or, in the two latter
ones, the wrong byte order (< or >). As always, common sense required.
