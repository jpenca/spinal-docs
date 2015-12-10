# spinal-docs
documentation for the spinal environment


# What is spinal?
spinal is an environment for programming step sequencers - currently it works exclusively with the [Elektron Analog Rytm](http://eu.elektron.se/drum-machines/analog-rytm/) drum computer. The spinal language is designed to let you rapidly express musical intentions in the sequencer grid.

Spinal is being implemented as a Mac application which interprets the language and communicates with the drum machine.

# Usage
Follow to tutorial below to get started.

## Setup
You need a Rytm drum computer and a Mac running the spinal application:
- Connect the drum machine to your Mac via USB.
- on the Rytm drum computer, make sure that USB-MIDI communication is enabled.
- Disable Overbridge-Mode on the Rytm drum computer.

Test if your setup is good to go:
- Launch the app, and press CMD+1 on your computer keyboard. The active sequencer pattern of your drum machine appears in the grid display. Make changes to the sequence, and hit CMD+1 again. The changes you made should be reflected in the grid display.

## Hello World
The 'hello world' of step sequencer programming is probably a 4/4 kick drum. Start with an empty pattern on the drum machine.
Then type the following into the spinal interpreter, then hit Return:
```
trig bd 1^16'4
```
press play on your drum computer - you should now hear a 4/4 bass drum pattern.
the command above translates to: **trigger the bass-drum track in the step range 1 to 16, with an interval of 4 steps.**

If you like, try to change the command a bit, for example change the `4` to a different number such as `3`, hit Return again.
try different tracks, e.g. `trig sd 1^16'4` for the snare-drum track.
also try different ranges, such as `2^8'4`.

The interval `'4` is optional. For example, try: `1^16`. The interval then simply becomes `1`.

- After each change, listen closely to the pattern you created.
- Look at the sequencer and how the trigs are placed.

Play around with this, and you will master the spinal language quickly.

## Ranges & Loops
The main design goal of spinal is to give you the ability to express yourself in terms of sequences that your drum machine can understand.
The basic building block for that is the range-loop expression - a powerful tool for creating intricate sequences.
**It is essential that you learn how to use the range-loop.** Once you know how it works, you have learned most of the spinal language already.

## Trig Parameters
These parameters are available for every sequencer trig. There is no limit to how many of these can be locked on specific steps.

| Arg Name            | Parameter                 | min         | max       | unit                                     |
| ------------------- | ------------------------- | -----------:| ---------:| ---------------------------------------- |
| not:                | Trig note                 | -24         | 24        | MIDI note semitones (integer)            |
| vel:                | Trig velocity             |  1          | 127       | MIDI velocity (int)                      |
| len:                | Trig length               |  0          | 127       | step length (float),  e.g. 1 = 1/16 note |
| trc:                | Trig condition            |  0          | 64        | integer or macro e.g. `prb.75`,`mod.1.3` |
| syn:                | Synth Enable Flag         |  0          | 1         | integer                                  |
| smp:                | Sample Enable Flag        |  0          | 1         | integer                                  |
| env:                | Envelope Restart Flag     |  0          | 1         | integer                                  |
| lfo:                | LFO Restart Flag          |  0          | 1         | integer                                  |
| snd:                | Sound Lock                |  0          | 127       | integer (0 == no lock)                   |
| mit:                | Micro-Timing              |  -23        | 23        | integer                                  |
| mut:                | Trig Mute Flag            |  0          | 1         | integer                                  |
| acc:                | Trig Accent Flag          |  0          | 1         | integer                                  |
| swi:                | Trig Swing Flag           |  0          | 1         | integer                                  |
| sli:                | Trig Parameter Slide Flag |  0          | 1         | integer                                  |
|  rt:                | Retrig Enable Flag        |  0          | 1         | integer                                  |
| rtr:                | Retrig Rate               |  0          | 1         | float e.g. 0.5, 1/32, 1/64, ...          |
| rtv:                | Retrig Velocity Ramp      |  -128       | 128       | integer                                  |
| rtl:                | Retrig Length             |  0          | 127       | step length (float), 1 = 1/16 note       |


## Parameter Locks
There are a total of 72 available parameter locks available in a sequencer pattern. This means you can automate 72 unique parameters across all 12 sequencer tracks. Keep this in mind! When using p-locks in all tracks, it is easy to fill up the lock memory.

| Arg Name            | Parameter                 | min         | max       | unit                                                   |
| ------------------- | ------------------------- | -----------:| ---------:| -----------------------------------------              |
| syn.0:              | Synth Parameter 0         | 0           | 127       | integer (some synth params unavailable on some tracks) |
| syn.1:              | Synth Parameter 1         | 0           | 127       | integer                                                |
| syn.2:              | Synth Parameter 2         | 0           | 127       | integer                                                |
| syn.3:              | Synth Parameter 3         | 0           | 127       | integer                                                |
| syn.4:              | Synth Parameter 4         | 0           | 127       | integer                                                |
| syn.5:              | Synth Parameter 5         | 0           | 127       | integer                                                |
| syn.6:              | Synth Parameter 6         | 0           | 127       | integer                                                |
| syn.7:              | Synth Parameter 7         | 0           | 127       | integer                                                |
| smp.tun:            | Sample Tuning             | -24         | 24        | float semitones (uses finetune parameter)              |
| smp.fin:            | Sample Fine Tuning        | -64         | 63        | float                                                  |
| smp.brr:            | Sample Bitrate Reduction  | 0           | 127       | integer                                                |
| smp.smp:            | Sample Selection          | 0           | 127       | integer (0 == no sample)                               |
| smp.sta:            | Sample Start Point        | 0           | 120       | integer                                                |
| smp.end:            | Sample End Point          | 0           | 120       | integer                                                |
| smp.lop:            | Sample Loop Flag          | 0           | 1         | integer                                                |
| smp.lev:            | Sample PLayback Volume    | 0           | 127       | integer                                                |




