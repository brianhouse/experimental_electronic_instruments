## Using musical pitches

Though no music theory is required here, if you want to use musical notes (even-temperment), Pd can convert to MIDI pitch numbers to frequencies and back. MIDI pitches are just the notes of an even-tempered piano labeled from 0–127. `mtof` converts this number to a frequency, and `ftom` converts it back.

I also made an object called piano to help you determine those pitches, or just to connect and fool around with notes.

If you want to use just-intonation or microtonal systems ... you'll have to program it yourself.

<p align="center">
  <img src="media/06_15_musical_notes.png" width=600 /><br />
</p>
