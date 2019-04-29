Eggduino
====

Arduino Firmware for Eggbot / Spherebot with Inkscape-Integration

Version 1.6a
tested with Inkscape Portable 0.91, Eggbot Extension and patched eggbot.py

Regards: Eggduino-Firmware by Joachim Cerny, 2015

Thanks for the nice libs ACCELSTEPPER and SERIALCOMMAND, which made this project much easier. Thanks to the Eggbot-Team for such a funny and enjoyable concept! Thanks to my wife and my daughter for their patience. :-)

Features:

- Implemented Eggbot-Protocol-Version 2.1.0
- Turn-on homing: switch-on position of pen will be taken as reference point.
- No collision-detection!!
- Supported Servos: At least one type ;-) I use Arduino Servo-Lib with TG9e- standard servo.
- Full Arduino-Compatible. I used an Arduino Uno
- Button-support (3 buttons)

Tested and fully functional with Inkscape.

Installation:

- Upload Eggduino.ino with Arduino-IDE or similar tool to your Arudino (i.e. Uno)
- Disable Autoreset on Arduinoboard (there are several ways to do this... Which one does not matter...)
- Install Inkscape Tools wit Eggbot extension. Detailed instructions: (You yust need to complete Steps 1 and 2)
http://wiki.evilmadscientist.com/Installing_software

- Because of an bug in the Eggbot-extension (Function findEiBotBoards()), the Eggduino cannot be detected by default.
	Hopefully, the guys will fix this later on. But we can fix it on our own.
    It is quiete easy:
	
        - Go to your Inkscape-Installationfolder and navigate to subfolder .\App\Inkscape\share\extensions
		- open File "eggbot.py" in texteditor and search for line:
			"Try any devices which seem to have EBB boards attached"
                - comment that block with "#" like this:
                		# Try any devices which seem to have EBB boards attached
				# for strComPort in eggbot_scan.findEiBotBoards():
				#	serialPort = self.testSerialPort( strComPort )
				#	if serialPort:
				#		self.svgSerialPort = strComPort
				#		return serialPort
		- In my version lines 1355-1360

# My Config

## Inkscape Extensions 
In `ebb_serial.py` add `port[1].startswith("Arduino Uno") or` on line 50:
```python
49		for port in comPortsList:
50		 	if port[1].startswith("Arduino Uno") or port[1].startswith("EiBotBoard"):
51				EBBport = port[0] 	#Success; EBB found by name match.
52				break	#stop searching-- we are done.
```

Extension are located on `/Applications/Inkscape.app/Contents/Resources/share/inkscape/extensions/` on MacOS.

## Stepper Driver Tuning

Stepper drivers need to be tuned to output the right amount of current to the stepper motors so that they don't get too hot. 

* A4988: `Imax = 2.4 x Vref` (with Rcs = 0,05 Ohm)
* DRV8825: `Imax = 2 x Vref`

My stepper are rated at 1.3A an i use A4988 drivers, so `Vref` should be `1.3 / 2.4 = 0.52V`.

More info: https://forum.arduino.cc/index.php?topic=415724.0