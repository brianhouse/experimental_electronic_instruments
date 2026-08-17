

# Sequencing (and Spatialization)

Now that we know how to create rich sonic material with Pd, we're going to explore how we can structure those sounds over time.


## Delay

Envelopes give us the notion of an "event" in time that can be triggered. As we've already seen, in Pd, a "bang" is what makes things happen. There's a lot we can do with bangs, but to start off, we can delay them. Like this:

<p align="center">
  <img src="media/del.png" width=400 /><br />
</p>

The number in `del` tells us how long the delay will be. In this case, it's 1000 milliseconds—or one second. 

Remember that there are two types of signals in Pd. So far, we've mainly been working with audio signals—the stuff that comes out of oscillators. But we've sent a few messages and used loadbang too—these aren't audio, they're "control" signals that tell Pd what to do. Audio objects are, of course, concerned with time, in the sense of building up audio waveforms. But `del` is the first object to demonstrate that control signals have a different notion of time, which functions to cue different things to happen when we want them to.


## Pulsation

Perhaps the most important object for sequencing is `metro` aka "metronome". `metro` outputs a bang every N milliseconds. To start a `metro`, we have to send it a 1, and to stop it, we send a 0. An easy way to do this is with a toggle object (both bangs and toggles are available from the object menu, or by typing "bng" or "tgl" respectively in an object box):

<p align="center">
  <img src="media/metro.png" width=400 /><br />
</p>


## Stepping

To take things a step further, we can add `ac/step`. This object will count the number of bangs it receives, up to but not including the parameter given as a default value or sent to its right inlet.

<p align="center">
  <img src="media/step.png" width=400 /><br />
</p>

A more interesting display than a number box is HRadio, which is available from the object menu. Since my step counter goes to 10, I've had to go into the properties on the HRadio and increase the number of cells from the default 8.

<p align="center">
  <img src="media/step_radio.png" width=400 /><br />
</p>

Next, we add a `select` object. `select` compares an input to its inlet with the numbers or symbols given to it as parameters. If the input matches one of them, the corresponding outlet gets a bang. (Note that the final outlet on `select` is just the input if the input doesn't match any of the specified options).

Since our `ac/step` has 10 steps, it outputs values from 0 to 9. `select` takes that number, and bangs the appropriate step.

<p align="center">
  <img src="media/select.png" width=600 /><br />
</p>

From here, we could connect those bangs to envelope generators to play sounds.

Here's a simple drum machine to show how that might look:

<p align="center">
  <img src="media/drum_machine.png" width=800 /><br />
</p>

Alternately, a sequencer could control the number values of oscillating frequencies. If we want those to align with standard musical tuning (12 TET), we can use the `ac/ptof` object to output a frequency value based on a note name:

<p align="center">
  <img src="media/pitch.png" width=800 /><br />
</p>


Also note that `ac/step` outputs a bang from its right outlet every time it has finished the count and is starting over. This means you can chain step counters together to have different levels of repeating events:

<p align="center">
  <img src="media/measures.png" width=400 /><br />
</p>


## Random and comparison operators

Beyond `metro`, `ac/step`, and `select`, a very useful object for sequencing is `random`. When `random` receives a bang in its left inlet, it outputs a random number less than the default parameter or a message sent to its right inlet.

Combined with `select`, this is an easy way to choose between several options on a bang. For example, this plays one of six pitches with each bang:

<p align="center">
  <img src="media/random.png" width=600 /><br />
</p>

`random` can also be used to introduce probabilistic events. For example, maybe a certain sound is only triggered 50% of the time:

<p align="center">
  <img src="media/random_2.png" width=400 /><br />
</p>

Note that this example uses another new object, `<` which outputs a 1 if it receives a number lesser than its argument. We can then select on the output of `<` to get a bang that only fires half of the time. If `random` is generating 0-99, then this value is the probability as a percentage.

Other comparison operator objects are: `<`, `<=`, `>=`, and `==` (note the double).

## Spatialization

We've already seen how to apply amplitude envelopes to sound in Pd as well as to "mix" sounds by dividing or multiplying their output by a number to decrease or increase their  amplitude.

However, note that the `ac/output~` has two inlets, one each for the right and left channel of a stereo signal. By connecting signals only to one or the other, we can pan them hard left or hard right.

...but for more detailed control, we can use `ac/pan~`. This object takes either a continually varying signal (aka an LFO) or a control value, and routes a mono input to stereo output, adjusting the balance between channels accordingly.

<p align="center">
  <img src="media/pan_1.png" width=500 /><br />
</p>

<p align="center">
  <img src="media/pan_2.png" width=500 /><br />
</p>


### Reverb

Look at `freeverb~`'s help file for details on how to use it. But at it's most basic, you simply hook the left channel to the left inlet, the right channel to the right inlet, stereo output, and away you go:

<p align="center">
  <img src="media/07_13_freeverb.png" width=600 /><br />
</p>

I recommend using only one `freeverb~` object as the very last object before `dac~` or `output~`. More than one will be a burden on your CPU, and with just one you will have a coherent sense of spatialization.
