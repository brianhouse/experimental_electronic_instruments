# Envelopes

But sound events in the physical world don't just sound continuously, they have a shape to them. There might be an initial strike, tone, or impact, followed quieter section as a vibrating body comes to rest. The timbre might also start off with one character and transition into another. So let's see what we can do with **envelopes**.

## Envelopes

The most typical configuration for an envelope, which you'll see all over the place on synthesizers and software instruments, is a four-stage function known as an ADSR:

<p align="center">
  <img src="media/adsr_diagram.png" width=600 /><br />
</p>

- **(A)ttack**: the length of time it takes the sound to reach its loudest point after it is triggered
- **(D)ecay**: the length of time after the attack it takes the sound to reach its sustain amplitude
- **(S)ustain**: the amplitude that the signal is held at after the initial dcay
- **(R)elease**: the length of time it takes the sound to fade to silence after it is released

You can think of triggering a sound and subsequently releasing it as analogous to pressing down an organ key, holding it for an arbitrary length of time, and then letting it go. 

In Pd, we can define an envelopes with the `ac/eg~` object (**e**nvelope **g**enerator):

<p align="center">
  <img src="media/eg.png" width=800 /><br />
</p>

Note that `ac/adsr~` takes a `1` to trigger the envelope and a `0` to release it. On its own, this does nothing—it just gives you the envelope as a signal to work with.


## Amplitude envelopes

But by multiplying some oscillating signal with the `ac/adsr~` signal, we can produce discrete sound events with very different characters. Experiment with how each parameter changes the sound.

<p align="center">
  <img src="media/eg_mult.png" width=800 /><br />
</p>



## Filter envelopes

Another common use for an envelope is to control the rolloff frequency of a filter. In this example, we're rescaling the 0-1 range of the envelope to a frequency range of our choosing. 

<p align="center">
  <img src="media/eg_filter.png" width=800 /><br />
</p>
