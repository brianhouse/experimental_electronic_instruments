# Sampling

Thus far with Pd, we've used oscillators, filters, envelopes, and timing objects to create synthesized sonic systems that do not depend on any pre-recorded audio data.

However, Pd is also capable of loading audio data and using it like a signal. It can manipulate audio "samples" in both senses of the word: short snippets of audio clipped from recordings, and the individual units of digital data captured from an analog signal.


## Preparing a sample

To begin with, we'll need some audio to work with. We can use Audacity for this. After opening the application, use "File" -> "Import" -> "Audio..." and navigate to the audio you want to use. This could be something you recorded, of course, but this example will use a musical track. This is an .m4a file, but Audacity should be able to convert it.

<p align="center">
  <img src="media/audacity_import_1.png" width=600 /><br />
</p>
<p align="center">
  <img src="media/audacity_import.png" width=600 /><br />
</p>


<p align="center">
  <img src="media/audacity_imported.png" width=600 /><br />
</p>

This track is stereo, but for our purposes, we need mono. After selecting everything with "Select" -> "All", we can use "Tracks" -> "Mix" -> "Mix Stereo Down to Mono" to create a single channel file.

<p align="center">
  <img src="media/audacity_mono.png" width=600 /><br />
</p>

Now we can zoom into the track and select a few seconds that we want to work with. You can use "Edit" -> "Clip Boundaries" -> "Split" to isolate a section, and then delete the clips on the either side and move your clip to the beginning of the track.

<p align="center">
  <img src="media/audacity_split.png" width=600 /><br />
</p>

The last thing we need to do before exporting is make sure there is a clean beginning and end to the clip, ie, that the waveform starts and ends at 0. We do this by making a very **very** short fade in and fade out, using the option in the "Effects" menu. (Without this, the clip will create "clicks" when it loops in Pd ... but if your fades are too long, you'll hear a bump regardless)

<p align="center">
  <img src="media/audacity_fade.png" width=600 /><br />
</p>

Export the track as a WAV file, and save it in your Pd folder. **rename the track to something short without a space** -- this will make things easier later.

<p align="center">
  <img src="media/audacity_save.png" width=600 /><br />
</p>


## Manipulating samples in Pd

Pd stores samples (in both senses) in data structures called arrays. Create an array by using the object browser. Using the info pane, give it a unique name, and note that you can specify the default number of samples (in the data sense). To find this number, multiply the number of seconds you want times the sample rate (eg, 48000).

<p align="center">
  <img src="media/array.png" width=600 /><br />
</p>

Next, create a `ac/smpplay~` object with the name of the array as its argument. This object will read the contents of the array and play it as audio when it receives a "start" message. 

Finally, create a `ac/smpload` object, also with the name of the array as an argument. When it receives a bang, `ac/smpload` will prompt you for a wav file to load into the array. Do so now—you should see the waveform appear in array graphic, and you should be able to play it.


<p align="center">
  <img src="media/sampler.png" width=600 /><br />
</p>

Notice that putting a slider object below the waveform shows the position of playback as the sample plays.

There are also two sliders that we can add to select just a portion of the sample for playback, "loop start" and "loop stop", each of which is scaled 0 to 1 over the length of the sample:

<p align="center">
  <img src="media/loop.png" width=600 /><br />
</p>

Note that if we added messages and a `loadbang`, we could therefore save the loop points so that they load with the patch.

...or we could set the points dynamically. For example, we could use a random start and during triggered by a metro:

<p align="center">
  <img src="media/random_loop.png" width=600 /><br />
</p>


Finally, `smpplay~`'s first inlet determines the playback rate. This offers some interesting possibilities. To begin with, you might simply experiment with the sample played at a faster or slower rate. (also note the auto-load of the wav file in this example)

<p align="center">
  <img src="media/08_10_rate.png" width=600 /><br />
</p>

However, by adding our pitch math to the rate calculation, we can create a tuned sampler:

<p align="center">
  <img src="media/tuned.png" width=800 /><br />
</p>

You could also make a series of messages with different frequency and/or pitch values and bang those with a sequencer.


## Using samples as audio signals

The left outlet of `ac/smpplay~` is an audio signal, and rather than connect it directly to `ac/spkr~`, you can use it as an input to any kind of synthesis, whether AM, FM, subtractive, or whatever else.

For example, here's a low-pass filter with the rolloff frequency controlled with a slider (remember to make it logarithmic and set an appropriate range in the properties):

<p align="center">
  <img src="media/filter_play.png" width=600 /><br />
</p>

We can, however, also automate a filter change. The right outlet of `ac/smpplay~` gives a value from 0 to 1 corresponding to how far along in the loop playback has progressed. We can use this value to automatically update the cutoff frequency we give to `ac/lpf~`, therefore creating a filter sweep with every loop.

To do this, we're going to use `ac/map`. `ac/map` takes numbers within a given range and rescales them to another range. So for example, if we are getting a signal between 0 and 1 from `ac/smpplay~`'s right outlet, and we want our filter sweep to go from 50 Hz to 8kHz, we could use the arguments `ac/map 0 1 50 8000`.

<p align="center">
  <img src="media/sweep.png" width=600 /><br />
</p>

(Also, `ac/map~`, with the tilde, works similarly for signals instead of control messages.)

For another example, let's add some tremolo to the output. All this entails is multiplying the output of `ac/smpplay~` by an LFO:

<p align="center">
  <img src="media/play_tremolo.png" width=600 /><br />
</p>

Ultimately, you can think of sample arrays as another kind of oscillator, one that can add a lot of sonic texture and conceptual possibilities to your patches.
