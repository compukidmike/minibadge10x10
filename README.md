# Saintcon Minibadge 10x10 Board
Welcome to your new favorite soldering project! 1600+ solder joints!

## Assembly
Aside from the obvious set of 200 8-Pin headers (they go on the side with the shiny squares!), there are a few through-hole parts you'll need to solder. They all go on the side with the shiny squares! The button and power switch don't have a set orientation. The screw terminal block is designed to have the wire openings facing the edge of the board so that you can actually put wires into it.

Starting with the 2023 board, you no longer need to solder the USB connector! It has been moved to the bottom of the board and comes pre-soldered with the rest of the surface mount parts.

## Solder Jumpers
There are 10 solder jumpers on the back of the board. They are ONLY for if the CLK chips are missing (U112-U116). They will allow you to tie the CLK pins to EITHER 3V3 or GND. If you solder these jumpers with the CLK chips on the board, you WILL damage the chips and possibly start a fire. You have been warned!

## Board Files
The KiCad project and gerbers are provided here. A couple notes:

- You'll need a programmer for the PIC microcontroller. The programming port says TPI, which is an Atmel thing. The original design used an ATTiny but parts availability forced a change to a PIC. The programming header wasn't changed, so you'll need to connect your programmer to the proper pins.

- Soldering the CLK driver chips (A3910) isn't for the faint of heart. The best way to do it is with a solder paste stencil and reflow oven. If you don't have those, hand soldering is _possible_ but not easy.
