# spinal-docs
documentation for the spinal environment


# What is spinal?
spinal is an environment for programming step sequencers - currently it works exclusively with the [Elektron Analog Rytm](http://eu.elektron.se/drum-machines/analog-rytm/) drum computer. The spinal language is designed to let you rapidly express musical intentions in the sequencer grid.

Spinal is being implemented as a Mac application which interprets the language and communicates with the drum machine.

# Usage
Let's get going

## Setup
you need a Rytm drum computer and a Mac running the spinal application:
- Connect the drum machine to your Mac via USB.
- on the Rytm drum computer, make sure that USB-MIDI communication is enabled.
- Disable Overbridge-Mode on the Rytm drum computer.

test if your setup is good to go:
- Launch the app, and press CMD+1 on your computer keyboard. The active sequencer pattern of your drum machine appears in the grid display. Make changes to the sequence, and hit CMD+1 again. The changes you made should be reflected in the grid display.

## Hello World
the 'hello world' of step sequencer programming is probably a 4/4 kick drum. Start with an empty pattern on the drum machine.
Then type the following into the spinal interpreter, then hit Return:
```
trig bd 1^16'4
```
press play on your drum computer - you should now hear a 4/4 bass drum pattern.
the command above translates to: **trigger the bass-drum track in the step range 1 to 16, with an interval of 4 steps.**

try to change the command a bit, for example change the `4` to a different number such as `3`, hit Return again.
try this on different tracks, e.g. `trig sd 1^16'4` for the snare-drum track.
also try different ranges, such as `2^8'4`. the interval `'4` is optional - you can ommit this, e.g.: `1^16`. The interval is then `1`.

## events on a grid
