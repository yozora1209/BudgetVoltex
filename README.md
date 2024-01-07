# BudgetVoltex
A sound voltex controller on the budget, for less than $50!
[picture] 
# About
This is a little project I made inspired by https://github.com/felixha00/lowcost-voltex and this repo is roughly documenting my journey throughout the building of the controller aswell as providing an updated (at least as of time of writing) tutorial on how you could build one yourself!
This controller uses a concept called a "handwired keyboard," handwired keyboards are a style of keyboards and a hobby in itself, but in essence, it is basically a bunch of switches connected together in rows and columns that are controlled by an Arduino Pro Micro, to which can be flashed with manually written firmware made and compiled on [QMK MSYS](https://msys.qmk.fm) and then flashed with [QMK Toolbox](https://github.com/qmk/qmk_toolbox).
# Materials
* Any housing to hold the switches and knobs (preferably wood because it's a cheap material)
* An Arduino Pro Micro (Any chinese knockoff will work)
* EC11 Rotary Encoders
* Mechanical Keyboard Switches
* 1N4148 Diodes x7
* A Soldering Iron
* Wires for soldering
  
Of course, prices may vary depending on geographical location, availability and how much you already own, but at least for me, who lives in Brazil, it was times cheaper than [$170 + $130 shipping](https://imgur.com/ir808ol), thanks, gamo2!

# Firmware and preparations
First, make sure you're running a system compatible with QMK and set it up by following their guide: https://docs.qmk.fm/#/newbs_getting_started

Next, we need to make sure to know which bootloader your arduino is running, most of the knockoffs will run the caterina bootloader, which is the bootloader already preset in the .hex firmware in this repository, to figure this out: plug in your arduino, open [QMK Toolbox](https://github.com/qmk/qmk_toolbox) and with a pair of tweezers, short the [RST (Reset) and GND (Ground) pins](https://imgur.com/uM4fLvM) once, it should say "[bootloader] device connected" written in yellow on QMK Toolbox, if it's written "Caterina Device connected," you can skip the next session, if it says "Atmel_DFU device connected" then you'll need to change 2 lines on the source code of the firmware and build it yourself, see next session
# Modifying the source
