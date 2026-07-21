# Synthesis




## Oscillators

All synthesis starts with oscillators. `osc~` is a perfect sine wave that oscillates between -1 and 1. However, there are other shapes that we can use. For example, replace `osc~` with `saw~`, `sqr~`, or `tri~` in your patch and play with the results (these are also externals we installed—look inside those files to see how they are made).

### Additive Synthesis

Simple oscillators can be added together, thus becoming the building blocks of much more complex waveforms.

Using the `+~` object (notice the tilde, which indicates that we are adding audio signals, not numbers), multiple oscillators can be combined. Because this also increases the amplitude of the waveform, we need to divide by the number of signals we are adding.

<p align="center">
  <img src="media/06_11_additive_synthesis_.png" width=600 /><br />
</p>

Try making two oscillators with very close frequencies—you'll hear "beating", which is another implicit oscillation at a frequency that is the difference between the two initial frequencies. Note that dragging on the number boxes in playback mode will increase or decrease by an integer value, but holding shift down will move the decimals.

### AM Synthesis and LFOs

Similarly, multiplication creates even wilder waveforms by combining two or more simple oscillators—essentially, one oscillator modulates the amplitude (volume) of the other. Traditionally, `phasor~` is used as a modulator. It is similar to `saw~`, but instead moving between the full signal range of -1 to 1, it oscillates from 0 to 1. When multiplied with the signal from `osc~`, this becomes an amplitude adjustment—from zero volume to full (1) volume.

<p align="center">
  <img src="media/06_11_am_synthesis.png" width=600 /><br />
</p>

If you set the `phasor~` down close to 20 Hz, you'll notice you get a pulsation effect. To smooth it out and make it into a "tremolo" instead, we can use an `cyc~` as a modulator instead of `phasor~`. Like `phasor~`, `cyc~`, moves between 0 and 1 instead of -1 and 1, but it does it with a sine shape instead of a sawtooth:

<p align="center">
  <img src="media/06_11_tremolo_.png" width=600 /><br />
</p>

Using `cyc~` in this way is known as a Low-Frequency Oscillator, or LFO. Even though it cannot be heard directly, LFOs can change the character of other sounds. They can also be used to control larger changes in a patch over time.


### FM Synthesis

While AM synthesis changes the amplitude of another signal, Frequency Modulation (FM) synthesis changes the frequency instead. Thus far, we have used static values for the frequencies of our oscillators, other than varying them manually with a Number box or slider. However, we can have another oscillator drive this change automatically instead.

<p align="center">
  <img src="media/06_12_fm_synthesis_.png" width=600 /><br />
</p>

Here, the frequency that we give to `osc~` is determined by another `osc~`. This second oscillator moves between -1 and 1 at 1000 times a second, which when multiplied by 2000 is between -2000 and 2000 at 1000 times a second. The frequency we give to the first oscillator thus becomes a center frequency around which the waveform rapidly oscillates. Varying these parameters results in complex waveforms.


## Filters and Subtractive Synthesis

There's another important "oscillator" that we've left out: `noise~`. Unlike the others, `noise~` produces random energy across the audio spectrum, aka "white noise":

<p align="center">
  <img src="media/06_14_noise.png" width=600 /><br />
</p>

`noise~` opens up the possibility of approaching synthesis from the opposite direction: instead of adding oscillators together to create complex sounds, what if we start from a maximally complex sound and _subtract_ frequencies to get what we want?

In order to do that, we'll need a **filter**. We've used filters a bit already in Audacity. Here, we are going to use an object called `vcf~` (voltage-controlled filter, in a nod to the electronics equivalent).

`vcf~` takes two parameters: a center frequency, and a "Q" or "resonance" value. When you send `noise~` (or any other audio signal) through `vcf~`, it filters out everything except the frequencies within a range of the center frequency as determined by the Q.

<p align="center">
  <img src="media/06_14_filters.png" width=600 /><br />
</p>

(In this example, I've created two sliders, one with a range of 50–5000, and one with a (reversed!) range of 10–1, which I've set in the properties)

Note that `vcf~` has two outlets: using the rightmost outlet instead will create a "low-pass" filter, which filters out all frequencies except those that are below the center frequency (now a "cutoff" frequency).


## Building complex oscillating systems

Note that this becomes a branching process: any number box can be replaced with another oscillator of some kind, whether it serves as a frequency modulator, an amplitude modulator, or to modulate the frequency of a filter. Combined, you can produce some dynamic sounds.

<p align="center">
  <img src="media/06_13_complex_.png" width=600 /><br />
</p>

Notice how the operator objects multiply or add to LFOs by a given value, which changes their range.



## Distortion

There's an easy way to generate (intentional) distortion in Pd. Rather than overloading your output, you can hook any audio signal into the `clip~` object, with the creation arguments set to -1 and 1 (or something like -.9 and .9 if you want to see what's happening a little more easily). This will constrain the signal to the normal range. But if you also add an amplitude multiplier just before `clip~`, it will function as a gain, and the higher you set it above 1, the more signal will get chopped off by `clip~`, generating distortion without a volume increase.

This tells us something about the nature of distortion: it "squares" off smooth audio signals, which creates richer harmonics, just like a square wave has a richer sound than a sine oscillator.

<p align="center">
  <img src="media/09_10_distortion.png" width=600 /><br />
</p>



## Using musical pitches

Though no music theory is required here, if you want to use musical notes (even-temperment), Pd can convert to MIDI pitch numbers to frequencies and back. MIDI pitches are just the notes of an even-tempered piano labeled from 0–127. `mtof` converts this number to a frequency, and `ftom` converts it back.

I also made an object called piano to help you determine those pitches, or just to connect and fool around with notes.

If you want to use just-intonation or microtonal systems ... you'll have to program it yourself.

<p align="center">
  <img src="media/06_15_musical_notes.png" width=600 /><br />
</p>



## Saving and loading

**SAVE YOUR WORK** Pd does nothing for you in the way of automatically saving things, and it is prone to crash (never set a font to size 0, for instance).

...but note that the numbers in your boxes aren't saved with the patch! If you find a combination you like, supply them as default values for each of the oscillators, or put them in messages that you can click or bang to restore the values.

One very helpful object for this is called `loadbang`—it sends a bang when the patch is loaded. Attach this to a message to set a value.

<p align="center">
  <img src="media/06_14_loadbang.png" width=600 /><br />
</p>



## Getting help

Command/Control-click on any object, and in addition to setting certain properties, you can pull up a help window that describes how to use an object and gives an example of how to connect it.

Additionally, under the "Help" menu, you can access the official Pd documentation, which is also available online here: [https://puredata.info/docs/manuals/pd/](https://puredata.info/docs/manuals/pd/)

Much of what I've written here is based on the FLOSS Manuals entry for Pd: [http://write.flossmanuals.net/pure-data/introduction2/](http://write.flossmanuals.net/pure-data/introduction2/)
