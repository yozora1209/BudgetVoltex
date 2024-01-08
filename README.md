# BudgetVoltex
A sound voltex controller on the budget, for less than $50!
[picture] 
# About
This is a little project I made inspired by https://github.com/felixha00/lowcost-voltex and this repo is roughly documenting my journey throughout the building of the controller aswell as providing an updated (at least as of time of writing) tutorial on how you could build one yourself!
This controller uses a concept called a "handwired keyboard," handwired keyboards are a style of keyboards and a hobby in itself, but in essence, it is basically a bunch of switches connected together in rows and columns that are controlled by an Arduino Pro Micro, to which can be flashed with manually written firmware made and compiled on [QMK MSYS](https://msys.qmk.fm) and then flashed with [QMK Toolbox](https://github.com/qmk/qmk_toolbox).
# Materials
* Any housing to hold the switches and knobs (preferably wood because it's a cheap material)
* An Arduino Pro Micro with the AtMega32u4 chip (Any chinese knockoff will work)
* EC11 Rotary Encoders
* Mechanical Keyboard Switches x7
* 1N4148 Diodes x7
* A Soldering Iron
* Some Solder
* Wires (copper or aluminum)
  
Of course, prices may vary depending on geographical location, availability and how much you already own, but at least for me, who lives in Brazil, it was times cheaper than [$170 + $130 shipping](https://imgur.com/ir808ol), thanks, gamo2!

# Firmware and preparations
First, make sure you're running a system compatible with QMK and set it up by following their guide: https://docs.qmk.fm/#/newbs_getting_started

Next, we need to make sure to know which bootloader your arduino is running, most of the knockoffs will run the caterina bootloader, which is the bootloader already preset in the .hex firmware in this repository, to figure this out: plug in your arduino, open [QMK Toolbox](https://github.com/qmk/qmk_toolbox) and with a pair of tweezers, short the [RST (Reset) and GND (Ground) pins](https://imgur.com/uM4fLvM) once, it should say "[bootloader] device connected" written in yellow on QMK Toolbox, if it's written "Caterina Device connected," you can download the ``sdvx_kb.hex`` file and skip the next session, if it says "Atmel_DFU device connected" or any other bootloader, then you'll need to change 2 lines on the source code of the firmware and build it yourself, see next session.

# Modifying the bootloader and building the firmware
First, download sdvx_kb.zip and extract it to C:\Users\[user]\qmk_firmware\keyboards, open the sdvx_kb folder and open the file info.json on a text editor, I use Notepad++ to modify the file. Now on line 39 you should see ``"development_board": "promicro",`` and you need to replace that line with 2 other lines, replace it with ``"processor": "atmega32u4",`` then press enter to make a new line and type in ``"bootloader": [name of bootloader],``. If you have a different bootloader you might want to check your processor aswell and replace the parameter accordingly.

Now you need to compile, to do that, open QMK MSYS, you should have it installed if you followed the aforementioned QMK setup guide, in the console, type ``qmk compile -kb sdvx_kb -km default``, this will compile the keyboard in the folder ``sdvx_kb`` with the keymap ``default``, if all goes well you should get a message like this: `` * The firmware size is fine - 16780/28672 (58%, 11892 bytes free)`` and a bunch of lines with ``[OK]`` in your logs.
If you get any errors in the compiler, check **how old this repository is**, the code format CAN and PROBABLY WILL change, in that case, you'll have to recode the entire firmware yourself. If this repository is recent enough you don't have any errors related to the code, ask the [QMK Discord server](https://discord.gg/qmk) for assistance on your error.

# Flashing the bootloader
WARNING!! If flashing that firmware fries your board or the code doesn't work, I AM NOT RESPONSIBLE FOR ANY DAMAGES, this firmware was coded for an atmega32u4 arduino pro micro device running the Caterina bootloader on Jan 2024 and it works fine on mine, it'll be your fault and your fault only for trusting me and flashing this firmware, should you have modified it or not. If you have any questions or concerns, ask the QMK Discord server.

You can do this before or after wiring (see [Wiring](https://github.com/M4th3wIsntHere/BudgetVoltex/blob/main/README.md#wiring)), it realistically doesn't matter, it should be a bit easier to do it before wiring because you might take up the GND pin used to enter bootloader mode.

First, plug in your Pro Micro and open QMK Toolbox, then on QMK Toolbox, on the top right of the screen press the open button and navigate to the path where ``sdvx_kb_default.hex`` is located, it should be either to where you downloaded it from this repo or in the ``.build`` folder inside the ``qmk_firmware`` folder if you built it yourself, select the file.

Now you have to enter bootloader mode, do like I mentioned before and with a pair of tweezers short the [RST (Reset) and GND (Ground) pins](https://imgur.com/uM4fLvM) once, it'll enter bootloader mode for 1 second, now once it disconnects, do it again, it'll enter bootloader mode for 8 seconds, that is the amount of time you have to press the "Flash" button on QMK Toolbox, once flashing starts the arduino LED will blink red, when it's done it'll stop blinking, check your logs and see if the flashing was successful, if it was you can now disconnect your arduino and proceed to the Wiring section.

# Wiring
Wiring consists of bridging pins and connecting them to the mainboard using copper/aluminum wires, the arduino will then read which key is being pressed because the firmware defines which button is pressed by what switch in what column and row.
To briefly explain columns and rows, columns are the vertical ordering in which switches are positioned and they should be bridged as such and rows are the same thing but horizontal.

Now for you to start wiring, place all your switches in your board, presumably you're gonna have 7 switches (from top to bottom left to right: START, BT-A, BT-B, BT-C, BT-D, FX-A and FX-B), to make it easier to understand, I've made an inaccurate ilustration of how the wiring should go on about, here's the subtitle:

Big Black Sqaure: the board your switches go to
Red Square: Right Laser encoder (it's flipped in the image because you're gonna work on the bottom of the board, therefore, everything's mirrored)
Blue Square: Left Laser encoder
Dark-ish blue square: The Arduino
Black Squares: Switches
Yellow dots inside the black square: Switch Pins
Red lines in the switch pin: 1N4148 Diode (The diode **MUST BE SOLDERED TO THE BOTTOM PIN** and the polarity **MUST BE CORRECT IN A WAY THE PART WITH THE RED END IS SOLDERED INTO THE PIN AND PART WITH THE BLACK END IS SOLDERED INTO THE ROW CABLE**)
Randomly colored lines: Wires

Note about the wires represented in the image: Each wire has a different color for easy comprehension, wires that cross eachother MUST NOT BE SOLDERED TEOGETHER, you HAVE to solder the wires to the correct pins and a wire of a single color that goes through multiple switches' pins should bridge the switches together, zoom into the end of the wire in the arduino (dark blue-ish square) to see some code (GND or a letter followed by a number), that is the pin you have to solder the cable to, please use this image as a guide for pins: https://imgur.com/biEyi6y (remove P before the code to get the actual pin, ex: PF4 is F4). The wire position is also not accurate in the image because it's meant to be understandable rather than accurate, pay attention to what pin your're soldering to what wire.

And you're done! Make sure you didn't short anything both in the board and in the wires because that could either fry the arduino or disrupt inputs and proceed to the next session.

# Assembling
Assembling varies from build to build, if your controller perhaps runs on the lid of a box that can be closed, then just finish assembling it and you'repretty much done, test if everything works correctly and if it doesn't, again, ask on the QMK discord server.
