# Getting started with the Daisy Seed

## Microcontrollers

The Daisy Seed is a microcontroller designed for audio.

A microcontroller is, in essence, a computer. The primary difference is that a computer is a general purpose machine with an operating system (like MacOS or Windows) that runs multiple programs, but a microcontroller only runs one program, its "firmware". The Daisy Seed is particularly awesome because we can use Pd patches as firmware.

What's the advantage of using a microcontroller over a computer? It can be integrated into an electrical circuit that includes other hardware components, like sensors and amplifiers. As such, rather than being contained within a sleek box with a monitor and keyboard, a microcontroller has its parts exposed so that circuits can be built around it. As artists and musicians, we can build microcontrollers into our own custom interfaces that combine basic parts into forms that go well beyond what is possible with pre-made systems.


<p align="center">
    <img src="media/daisy.png.webp" width="400" />
</p>


## Circuits

According to the dictionary, in the general sense the word circuit means "a roughly circular line, route, or movement that starts and finishes at the same place." That applies in the electrical sense, too. A circuit is a loop, or rather, it's typically a whole knot of loops, in which electrical current is flowing from "power" back to "ground" and making something happen along the way. Between power and ground, we use positive (+) and negative (-) to indicate the direction of the flow. 

When it comes to audio, we can also think of the flow of current in terms of an audio signal. For example, an amplifier takes an audio signal, boosts it with additional current, and then sends it out to a speaker, which transduces that current into physical motion in the air.

When we're working with the Daisy Seed, we're going to be making circuits that run from power to ground through its various "pins" and external components like switches and LEDs.


## Pins

So what are pins? They are the connection points on the seed, the legs to which we can attach electrical components to make circuits. The diagram below is a "pinout" diagram that shows which is which. There are pins to connect stereo audio output (L OUT, R OUT) and input (L IN, R IN), a power source (VIN), output to + (VOUT), and connections to - (GND). The rest of the pins are general purpose pins for buttons, switches, etc.


<p align="center">
    <img src="media/daisy_pinout.jpg" width="600" />
</p>

To begin, we're going to set things up for prototyping on a breadboard.


## Breadboards

While we can make permanent circuits by soldering on perf boards, it's much easier to experiment using breadboards.

Breadboards are awesome because they let us work through our circuit ideas quickly and reward experimentation. Wires can sometimes pop loose, but that's a small price to pay for not having to undo soldered connections to change something.

The following image shows how breadboards create hidden connections between their holes—you can connect components together simply by plugging them into adjacent holes, according to this scheme:

<p align="center">
    <img src="media/breadboard.png" width="800" />
</p>


As a result:

<p align="center">
    <img src="media/connections.png" width="800" />
</p>


### Power Rails

Those long strips of connected holes on the top and bottom of the breadboard are called "power rails." We'll set things up so that + runs through the red strips and - runs through the blue/black ones. This gives us lots of points to connect to power and ground.


## Shorting

There is basically only one rule. And that is that when we create a circuit, it must include some kind of resistance to limit the amount of current that can flow. Otherwise ... BOOM. Fried chip. 

**Do not connect any pin to ground (-) other than GND without resistance in-between**

We'll learn what constitutes resistance as we go.


## Setup

Put the seed on the breadboard like in the image below. The numbers and letters on the breadboard don't matter, so don't get confused when we talk about the pins on the seed—the ones in the diagram above are what we're referring to.

<p align="center">
    <img src="media/setup_bb.png" width="1000" />
</p>


Notice below how both ground pins are connected to - on the power rail. VOUT feeds + to the rail, and then we have two wires that run across the board so that both rails are connected to each other.

As a convention, I use red wires to connect to + on the power rail, and black for -. All other wires are some other color. This helps to keep things straight when we're debugging circuits. 

For now, VIN isn't connected to anything—our input source will be the USB jack.

This will always be our the basic starting point when wiring things up with the seed.



## Hello World

### Simple interface circuit

Starting with our basic setup, we can begin to attach other elements. To begin with, let's add a button:



A button (or "momentary switch") has two sides, each with two pins. When you press the button, everything is connected together. Notice that one side is connected to +, and the other side is connected to - _through a resistor_.

A resistor is a component whose job it is to limit current. Depending on the situation, we'll use resistors with different values. In this case, it's 10kΩ.

Finally, we connect a wire between a pin on the seed and the ground side of the button. If the button isn't pressed, the pin will show that it's connected to - (via the resistor). If the button _is_ pressed, it will show that it is connected to +. 

In this case, I used pin 10. No relation to the breadboard number which happens to be right next to it, you have to count the pins on the seed itself and follow the pinout diagram.

So how do we use this?


### Pd patch


In Pd, open a new patch. Create a simple oscillator of some kind, and have it turn on and off with a toggle.




In Pd, open a patch, and choose "Compile" under the main menu. You should be prompted to install or update your "toolchain" if you haven't already. Go ahead and do this.









