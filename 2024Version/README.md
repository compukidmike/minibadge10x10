# Saintcon Minibadge 10x10 Board
Welcome to your new favorite soldering project! 1600+ solder joints!

# This is for the 2024 Version
There are a lot of changes in the 2024 version and a lot of this does not apply to previous versions.

## Assembly
Aside from the obvious set of 200 8-Pin headers (they go on the side with the shiny squares!), there are a few through-hole parts you'll need to solder. They all go on the side with the shiny squares! The power switch doesn't have a set orientation. The CLK SPEED switch does have an orientation but the pin arrangement will only allow you to insert it one way. The screw terminal block is designed to have the wire openings facing the edge of the board so that you can actually put wires into it, but some people have put it the other way to fit tight cases.

New for this year, it has a USB-C port! Also, you don't need to solder the USB connector! It has been moved to the bottom of the board and comes pre-soldered with the rest of the surface mount parts.

## Solder Jumpers
The solder jumpers on the back have been removed. The new CLK driver chips are easy to hand solder so there wasn't a reason to keep them.

## CLK Driver
The CLK driver chip is different this year. Instead of the tiny A3910, the TPM8837C is an SOIC package that is hand solderable. This greatly simplifies assembly if you choose to make one of these boards on your own. It also makes it easier to visually verify that the chips are soldered properly and will work.

## CLK Generator
The CLK generator is very different this year. Instead of a microcontroller, we're using a 14-bit Binary Ripple Counter with Oscillator. This eliminates the need to program the chip (and have a programmer for it). It's also a cheaper solution and one that is easily modifiable without changing code. 

The 74LV4060D is an interesting chip. At its heart is a simple 14-bit pulse counter. Whenever a pulse comes in, it adds 1 to its counter. The cool part is that most of those 14 bits are connected to external pins. That means that as it counts, the pins represent the number in binary. By choosing a specific frequency to put into it, we can divide that frequency by any power of 2, up to 2<sup>14</sup>. This is all great, but where do we get that frequency from? Well, this chip has another trick up its sleeve. It has a built-in oscillator circuit. So if we connect a capacitor and a couple resistors, it uses them to generate a frequency that is dependent on the value of those parts. So by carefully selecting values, we can get it to generate and divide a frequency of our choosing! 

I chose to generate a frequency of 512Hz and since that's a power of 2, I can get a nice 1Hz pulse out of it. This is fed into the CLK driver chip mentioned above, and now we can run the CLK pins for our minibadges! But wait, those CLK driver chips have 2 inputs to control the 2 outputs since they're meant to run motors in FWD/REV (and they also support brake/coast if the inputs are both the same). That's where the inverter chip comes in. Since we don't have a microcontroller with multiple outputs, we need to take our CLK pulse and provide an inverted signal to the other input on the CLK driver chip. Since the driver chip has 2 outputs, this enables us to drive two columns of CLK pins with a single driver chip.

Side Note: It is a bit strange to have the 74LV4060D chip generate a 512Hz frequency. It's quite low compared to how these chips are normally used. The main reason I chose it is that RC oscillators are very sensitive to changes in part values. Resistor and capacitor values can change a bit with temperature, which will make the frequency shift. We can't really fix that, but one thing we can do is to choose a large-ish capacitor value which will make it more immune to capacitance changes from nearby objects, like your body (this becomes especially important when using this circuit on things like minibadge expansion boards that will be worn on a lanyard). Larger capacitor values result in a lower frequency which works out for our use case.

## USB Load Switch
We added some circuit protection for the USB power circuit. The AP22818BKAWT is a Load Switch chip that provides overcurrent and short circuit protection. It allows 2 amps of current before it trips. This should prevent most mishaps when there's a short circuit somewhere on the board (or on one of those 1600+ solder joints). It also provides some protection for whatever USB device is providing power to the board.

## Board Files
The KiCad project and gerbers are provided here.
