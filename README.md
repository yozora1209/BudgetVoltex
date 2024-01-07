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
* Mechanical Keyboard Switches
* 1N4148 Diodes x7
* A Soldering Iron
* Wires for soldering
  
Of course, prices may vary depending on geographical location, availability and how much you already own, but at least for me, who lives in Brazil, it was times cheaper than [$170 + $130 shipping](https://imgur.com/ir808ol), thanks, gamo2!

# Firmware and preparations
First, make sure you're running a system compatible with QMK and set it up by following their guide: https://docs.qmk.fm/#/newbs_getting_started

Next, we need to make sure to know which bootloader your arduino is running, most of the knockoffs will run the caterina bootloader, which is the bootloader already preset in the .hex firmware in this repository, to figure this out: plug in your arduino, open [QMK Toolbox](https://github.com/qmk/qmk_toolbox) and with a pair of tweezers, short the [RST (Reset) and GND (Ground) pins](https://imgur.com/uM4fLvM) once, it should say "[bootloader] device connected" written in yellow on QMK Toolbox, if it's written "Caterina Device connected," you can skip the next session, if it says "Atmel_DFU device connected" or any other bootloader, then you'll need to change 2 lines on the source code of the firmware and build it yourself, see next session.
# Modifying the bootloader settings in the firmware
First, download sdvx_kb.zip and extract it to C:\Users\[user]\qmk_firmware\keyboards, open the sdvx_kb folder and open the file info.json on a text editor, I use Notepad++ to modify the file. Now on line 39 you should see ``"development_board": "promicro",`` and you need to replace that line with 2 other lines, replace it with ``"processor": "atmega32u4"`` then press enter to make a new line and type in ``"bootloader": [name of bootloader]``.
Now you need to compile, to do that, open QMK MSYS, you should have it installed if you followed the aforementioned QMK setup guide, in the console, type ``qmk compile -kb sdvx_kb -km default``, this will compile the keyboard in the folder ``sdvx_kb`` with the keymap ``default``, if all goes well you should get a message like this: `` * The firmware size is fine - 16780/28672 (58%, 11892 bytes free)`` and a bunch of lines with ``[OK]`` in your logs.
If you get any errors in the compiler, check **how old this repository is**, the code format CAN and PROBABLY WILL change, in that case, you'll have to recode the entire firmware yourself. If this repository is recent enough you don't have any errors related to the code, ask the [QMK Discord server](https://discord.gg/qmk) for assistance on your error.

# Flashing the bootloader
WARNING!! If flashing that firmware fries your board or the code doesn't work, I AM NOT RESPONSIBLE FOR ANY DAMAGES, this firmware was coded for an atmega32u4 arduino pro micro device running the Caterina bootloader on Jan 2024 and it works fine on mine, it'll be your fault and your fault only for trusting me and flashing this firmware, should you have modified it or not, if you have any questions or concerns, ask the QMK Discord server.

You can do this before or after wiring (see [Wiring]()), it realistically doesn't matter, it should be a bit easier to do it before wiring because you might take up the GND pin used to enter bootloader mode.
First, plug in your Pro Micro and open QMK Toolbox, then on QMK Toolbox, on the top right of the screen, press the open button

# Wiring
