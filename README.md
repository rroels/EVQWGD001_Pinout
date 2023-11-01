# EVQWGD001 Rotary Encoder Pinout

> [!NOTE]
> Most vendors provide an incorrect pinout and as a result a lot of examples and projects have it wrong. However, even if it's wired "wrong" it will still work fine, as most libraries are able to deal with the resulting signals. 

TL;DR, this is the correct pinout:

<img src="images/pinout.jpg" width="300">

Continue reading if you want to know why. 

## Context

I wanted to use a EVQWGD001 rotary encoder in a project. However, it was not very clear how to use it. When searching for information I found conflicting info.

* https://teletype.in/@gleb.sexy/big-l
* https://hackaday.io/page/11326-drawing-again
* AliExpress vendor pages
* ...

I encountered different documentation, each with a order of the pins, and even on the vendor page on AliExpress it would have two different pinouts in the same product description. 

Even when looking at existing projects (e.g. custom keyboard PCBs), I would find them wired up in different ways. 

This left me with quite some questions:

* What is the correct pinout of the EVQWGD001 rotary encoder?
* How come people are using different wiring, yet it seems to work fine either way?

## Determining the pinout of the EVQWGD001

### Setup

The goal of the experiment is to use an oscilloscope (Analog Discovery 3) to visualise the three relevant pins, try different combinations, and figure out the correct pinout. 

As the pins of the EVQWGD001 didn't play well with breadboards and dupont cables, I built a quick and dirty jig to hold the EVQWGD001. It also allowed me to easily rewire the pins and connect the oscilloscope to various points in the circuit. 

I wanted to avoid using a microcontroller to keep things simple, so I added two pull-up resistors that can be connected to another pins of my choice. 

<img src="images/jig1.jpg" width="300">

<img src="images/jig2.jpg" width="300">

### Measurements

I was then able to wire and visualise the various possible pinouts. Basically, either the first pin is the common pin, the second pin is the common pin or the third pin is the common pin. 

For each of these three scenarios, I made small recording on the oscilloscope which contained one scroll up and one scroll down. This is what it looks like:

<hr>

Assumption: the first pin is the common pin

<img src="images/common_first.png" width="500">

<hr>

Assumption: the second pin is the common pin

<img src="images/common_mid.png" width="500">

<hr>

Assumption: the third pin is the common pin

<img src="images/common_last.png" width="500">

### Conclusion

As you can see, in the first two scenarios where we assume either the first or second pin is the common pin, it doesn't look like the signal one would expect from a rotary encoder. One of the channel's square waves is always aligned with the start or end of the other channel's square waves. 

However, they are supposed to overlap, like this (source: [ArduinoGetStarted.com](https://arduinogetstarted.com/tutorials/arduino-rotary-encoder):

<img src="images/phase.webp" width="300">

We only saw similar results in scenario 3, where we assumed that the third pin is the common pin:

<img src="images/common_last2.png" width="500">

Thus, we can conclude that the third pin is the common pin, and the first and second pins are the "A" and "B" channel respectively. 

## Sources

* [https://howtomechatronics.com/tutorials/arduino/rotary-encoder-works-use-arduino/](https://howtomechatronics.com/tutorials/arduino/rotary-encoder-works-use-arduino/)
* [https://www.mathertel.de/Arduino/RotaryEncoderLibrary.aspx](https://www.mathertel.de/Arduino/RotaryEncoderLibrary.aspx)
* [https://circuitdigest.com/microcontroller-projects/rotary-encoder-module-interfacing-with-arduino](https://circuitdigest.com/microcontroller-projects/rotary-encoder-module-interfacing-with-arduino)
* [https://arduinogetstarted.com/tutorials/arduino-rotary-encoder](https://arduinogetstarted.com/tutorials/arduino-rotary-encoder)