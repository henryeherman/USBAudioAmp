USB Audio Amplifier
===================
The USB Audio amplififer is based upon the reference design for the PCM2906C.  The output from the amplifier is fed in to an STA540 Power amplifer, the design of which is shamelessly borrowed from the `Sparkfun <http://www.sparkfun.com>`_ `audio amplifier kit <http://www.sparkfun.com/products/9612>`_.  The stereo input of the usb codec chip is driven by two instrumentation amplifiers that provide selectable gain of 5, 50 100.5 and 505 or 14dB, 34dB, 40dB and 54dB respectively.  The purpose is further discussed below.

The designs files were all created in altium, but a pdf and gerber files are provided so the design can be reused by those interested.

The schematic is broken into four separate schematics for easier digestion.  The four section are described as follows:

Power Supply
------------
Power enters the widget through the VPWR terminal block.
Pheonix 2 pin 5.08mm spacing pluggable screw terminals were selected.
They might be more expensive than standard 2pin terminal, but they are AWESOME and worth it!
Nearly any two pin 5.08 spacing connector can be substituted.
A fuse holder in provided to protect the device against shorts and over current conditions.
The STA540 power amplifier (U8), when powered by 18Volts and configured to drive two loads can drive 38 Watts per channel, for a total of 38 W x 2 or 78 Watts.  Using P = VxI,  at 18 Volts this translates to a max current of 4.3 amps.  You may use a 5 Amp fuse.  Q1 is a power pfet and provides reverse voltage protection.  Still try not to reverse the polarity of the power supply!
The STA540 has a thermal cutoff to prevent overheating. Its still best not to short the outputs of the AMP!
The maximum voltage rating for the STA540 is 22 volts, BUT you should not put more than 18V into the board.
A transient voltage suppressor is in parallel and provides suppression of voltage greater than 20V. You stil should not put a voltage of greater than 18V into the board as the TVS can only dissipate 1.5W of power continuously. If you do, you will see magic blue smoke!
The intention was to run this device off of 12 Volt battery (think automobile battery). 
The minimum voltage for board funcioning is 8 Volts as this in the minumum voltage for correct function of the STA540 (U8) power amplifier.

Two LM317 adjustable voltage regulators (VR1 and VR2) provide 3.3V and 5V rails respectively.  These sources pass through 2 PI filters to further reduce the power supply noise (assuming you have power supply noise).  CON1 (a three pin locking header) provides access to the power rails should yo have additional devices to power.  Power consumption on these rails is fairly low (<500mA) and the LM317 regulators are capable of delivering 1.5A, take care not to exceed this value.
LED D3 and D4 are status indicators for the two regulated power rails (3.3V and 5V).

USB Codec
---------
The PCM2906 is a very *nifty* ic.  It is a **full** stereo sound card in an chip!  It is a USB device the abides by the audio class USB specification.  
The great part about the chip is that it works on **ALL** major OS systems; Windows, MACOSX, Linux, etc.  Additionally, no drivers need to be installed.  
The PCM2906 (U9) requires a 12Mz crystal.  Should you want to have external volume controls, pins 1,2 and 3 of CON3 provide access to the HID pins which can be used to control your system volume.  Just use a momentary switch that pulls to ground. CON2 provides access to the S/PIDF input and output.  The PCM2906 may be powered from the USB bus or with external power. Jumper J1 allows selection of the power source.


Amplifier Output
----------------
The real interesting part about the widget is the combination of the PCM2906 and the STA540 on a single device, making it a *BOOM* box on a board.  Combine it with any 12-18V power supply, a laptop or single board computer (think rasberry pi) and a pair of 4 Ohm to 8 Ohm 40 Watt speakers and you have yourself a party.  The STA540 (U8) provides gain of 20dB this translates to a factor of 10x.  The PCM2906 output voltage swing is   0.6 * 3.3V or 1.97Vpp. Two non inverting gain stages (U6 and U7) are provides to add additional gain to the outputs.  By default the installed feedback network provide voltage gain of 1.5x or 3.5dB. The sliding switch S1 allows a user to put the STA540 into standby mode.  The D2 led will be on when it is in standby.  Two Pheonix pluggable screw terminals OUTL and OUTR provide access to the output signals from the amplifier.  The DAC ouput offers a resolution of 8, 16 bits.


Amplifer Input
---------------
*TODO* Test Inut Amplifiers
The input amplifiers are as of yet untested. 

Two instrumentation amplifiers (U2 and U3) provide selectable gain for very weak signals.  
Dip switch array SW1 allows selecting gains of 5, 50 100.5 and 505 or 14dB, 34dB, 40dB and 54dB respectively.
U5 prodives a low impedance reference voltage for the two instrumentation amplifiers.  The signals from the instrumentation amplifiers are fed in to the ADC inputs of the PCM2906.  The ADC input resolution offers a resolution of 8 or 16 bits.

Purpose
-------
The intention is to use directly couple a singal into an aqueous medium using one *"widget"* and use another *"widget"* to amplify the signal and receive it.  Using GNURadio it maybe possible to explore underwater electrostatics as a communication channel.
