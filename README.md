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

## Trig Properties
These parameters are available for every sequencer trig. There is no limit to how many of these can be enabled in a pattern.
(That is, any step can have any of these enabled. There are 64x12=768 maximum individual steps in a pattern.)

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
There are a total of 72 available slots for parameter locks available in the memory of a sequencer pattern. This means you can automate 72 unique parameters across all 12 sequencer tracks. Keep this in mind! When using p-locks on several tracks, it is easy to hit the memory ceiling.

#### Synth Parameters
Note: Synth Parameters differ between tracks. Not all parameters are available on all tracks.

| Arg Name            | Parameter                 | min         | max       | unit                                                   |
| ------------------- | ------------------------- | -----------:| ---------:| -----------------------------------------              |
| syn.0:              | Synth Parameter 0         | 0           | 127       | integer                                                |
| syn.1:              | Synth Parameter 1         | 0           | 127       | integer                                                |
| syn.2:              | Synth Parameter 2         | 0           | 127       | integer                                                |
| syn.3:              | Synth Parameter 3         | 0           | 127       | integer                                                |
| syn.4:              | Synth Parameter 4         | 0           | 127       | integer                                                |
| syn.5:              | Synth Parameter 5         | 0           | 127       | integer                                                |
| syn.6:              | Synth Parameter 6         | 0           | 127       | integer                                                |
| syn.7:              | Synth Parameter 7         | 0           | 127       | integer                                                |

#### Sample Parameters
| Arg Name            | Parameter                 | min         | max       | unit                                                   |
| ------------------- | ------------------------- | -----------:| ---------:| -----------------------------------------              |
| smp.tun:            | Sample Tuning             | -24         | 24        | float semitones (uses finetune parameter)              |
| smp.fin:            | Sample Fine Tuning        | -64         | 63        | float                                                  |
| smp.brr:            | Sample Bitrate Reduction  | 0           | 127       | integer                                                |
| smp.smp:            | Sample Selection          | 0           | 127       | integer (0 == no sample)                               |
| smp.sta:            | Sample Start Point        | 0           | 120       | integer                                                |
| smp.end:            | Sample End Point          | 0           | 120       | integer                                                |
| smp.lop:            | Sample Loop Flag          | 0           | 1         | integer                                                |
| smp.lev:            | Sample Playback Volume    | 0           | 127       | integer                                                |

#### Filter Parameters
| Arg Name            | Parameter                 | min         | max       | unit                                                   |
| ------------------- | ------------------------- | -----------:| ---------:| -----------------------------------------              |
| fil.atk:            | Filter Envelope Attack    | 0           | 127       | integer                                                |
| fil.dec:            | Filter Envelope Decay     | 0           | 127       | integer                                                |
| fil.sus:            | Filter Envelope Sustain   | 0           | 127       | integer                                                |
| fil.rel:            | Filter Envelope Release   | 0           | 127       | integer (0 == no sample)                               |
| fil.frq:            | Filter Cutoff Frequency   | 0           | 127       | integer semitones                                      |
| fil.res:            | Filter Resonance          | 0           | 127       | integer                                                |
| fil.typ:            | Filter Type               | 0           | 6         | integer                                                |
| fil.env:            | Filter Env Mod Depth      | -64         | 63        | integer                                                |

#### Amp Parameters
| Arg Name            | Parameter                 | min         | max       | unit                                                   |
| ------------------- | ------------------------- | -----------:| ---------:| -----------------------------------------              |
| amp.atk:            | Amp Envelope Attack       | 0           | 127       | integer                                                |
| amp.hld:            | Amp Envelope Hold         | 0           | 127       | integer                                                |
| amp.dec:            | Amp Envelope Decay        | 0           | 127       | integer                                                |
| amp.ovr:            | Amp Overdrive Amount      | 0           | 127       | integer                                                |
| amp.del:            | Amp Delay Send            | 0           | 127       | integer                                                |
| amp.rev:            | Amp Reverb Send           | 0           | 127       | integer                                                |
| amp.pan:            | Amp Panning               | -63         | 64        | integer                                                |
| amp.vol:            | Amp Volume                | 0           | 127       | integer                                                |
| amp.acc:            | Amp Accent Amount         | 0           | 127       | integer                                                |

#### LFO Parameters
| Arg Name            | Parameter                 | min         | max       | unit                                                   |
| ------------------- | ------------------------- | -----------:| ---------:| -----------------------------------------              |
| lfo.spd:            | LFO Speed                 | -64         | 63        | integer                                                |
| lfo.mul:            | LFO Speed Multiplier      | 0           | 23        | integer                                                |
| lfo.fad:            | LFO Fade In/Out           | -64         | 63        | integer                                                |
| lfo.dst:            | LFO Destination           | syn.lev     | amp.rev   | parameter name                                         |
| lfo.wav:            | LFO Waveform              | 0           | 6         | integer 0 == TRI, ...,  6 == RND                       |
| lfo.sph:            | LFO Start Phase           | 0           | 127       | integer                                                |
| lfo.mod:            | LFO Mode                  | 0           | 4         | integer 0 == FREE, 4 == HALF                           |
| lfo.dep:            | LFO Modulation Depth      | -128        | 128       | float                                                  |
