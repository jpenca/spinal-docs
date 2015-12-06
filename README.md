# spinal-docs
documentation for the spinal environment


# What is spinal?
spinal is an environment for programming step sequencers - currently it works exclusively with the [Elektron Analog Rytm](http://eu.elektron.se/drum-machines/analog-rytm/) drum computer. The spinal language is designed to let you rapidly express musical intentions in the sequencer grid.

Spinal is being implemented as a Mac application which interprets the language and communicates with the drum machine.

# Usage
Let's get going

## Setup
you need a Rytm drum computer and a Mac running the spinal application:
- Connect the Drum machine to your Mac via USB.
- on the Rytm drum computer, make sure that USB-MIDI communication is enabled.
- Disable Overbridge-Mode on the Rytm drum computer.

test if your setup is good to go:
- Launch the app, and press CMD+1 on your keyboard. The active sequencer pattern of your drum machine appears in the grid display. Make changes to the sequence, and hit CMD+1 again. The changes you made should be reflected in the grid display.

## The Language
