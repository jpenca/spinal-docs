# spinal-docs

![alt text][ffwd-gif]



# What is spinal?
spinal is an environment for programming step sequencers - currently it works exclusively with the [Elektron Analog Rytm](http://eu.elektron.se/drum-machines/analog-rytm/) drum computer. The spinal language is designed from scratch to let you rapidly express musical intentions in a sequencer grid.

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
Then type the following into the spinal interpreter, then hit CMD+Return:
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

Everywhere in spinal where a numerical value is used, you can use a range-loop expression.


#### Range
To specify a numerical range, use the `^` operator:
`1^16` means `1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16`.

#### Interval
To specify an interval within a range, do it like this:

`1^16'4` means `1,5,9,13`.

You can chain multiple intervals like this:

`1^16'4'3'2` means `1,5,8,10,14`.

Intervals will take turns. After the last interval is used, the range-loop will wrap to the first interval again.

Intervals can also be negative numbers or 0:

`1^16'3'-1'0` means `1,4,3,3,6,5,5,8,7,7,10,9,9,12,11,11,14,13,13,16,15,15`

#### Array
You can specify a chain of numerical values directly, without the range operator:

`1'2'3'5` means `1,2,3,5`.

`1` means `1`.

`2` means `2`.

etc...

####Wrapping
A range-loop will wrap around once it reaches its end. In certain cases, this is an indicator for the function to return, e.g. in the `trig` function, if the step range wraps, the function will break. But in most other cases, the wrapping behaviour is used to create looping number sequences, for example looping MIDI note values.

`1^3` loops as such: `1,2,3,1,2,3,1,.....`

`1^4'3` loops like this: `1,3,2,1,3,2,1,.....`

#### Arithmetic
You can do arithmetic with range-loops. Arithmetic operators are: `=,-,*,/`

`2*1^3` means `2,4,6`

`1+2` means `3`

Operator precedence (order of operation) is currently conceptionally buggy. If in doubt, use parentheses.

#### Parentheses
You can use parentheses to indicate your preferred order of arithmetic operations.
Example:

`(1+2)*3` means `9`

`1+2*3` means `7`


#### Nesting
It is fun to nest range-loops. This can be used to create complex sequences.

`1'2'3` means `1,2,3`

`1'(2'4)'3` means `1'2'3'1'4'3`

The `2'4` above is its own range-loop, and it is nested within the `1'...'3` loop.
Fun!


## The Trig Command
The most powerful command in Spinal is the TRIG command. It's for programming events into the sequencer.
It has the following syntax:

`trig <track(s)> <step(s)> <optional:p-locks>`
	
`<track(s)>` determines in which track(s) note events should be placed. valid values are integer numbers from 0 to 11. 

There are macros `BD, SD RS, CP, CP, BT, LT, MT, HT, CH, OH, CY, CB` corresponding to these numbers and you can simply use these.
If you want to trigger notes in more than one track, use multiple tracks in a range-loop, such as `BD'SD`, which is the same as `0'1`.
This means that the tracks will be alternating between the BD and SD track when generating notes.

`<step(s)>` determines in which step(s) note events should be placed. valid values are floating point numbers from 1 to 64. Floating-point values will translate to trig microtiming. For example, `1^16'1.5` will put down note events every 1.5 steps.
	

The `<tracks>` and `<steps>` work together in parallel and - with a bit of practise - let you create very complex patterns with a great deal of expression and control.

to go even further, the optional P-LOCKS argument(s) lets you program sequencer automation. The syntax for this is:

`parameter-name:parameter-value`, for example: `fil.frq:20'60'90` - this will set p-locks for the filter cutoff frequency and alternate between the provided values 20, 60 and 90.

You can chain as many p-locks in one command as you like. You can use complex range-loops for the values.

a simple example:

`trig bd'sd 1^16
fil.frq:40'80
smp.smp:20^28
smp.tun:0'2'(5'7)'1`

you can also split the command over multiple lines in the editor for a better overview:

    trig bd'sd 1^16
    fil.frq:40'80
    smp.smp:20^28
    smp.tun:0'2'(5'7)'1`

see below for a list of parameter names.


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
There are a total of 72 available slots for parameter locks available in the memory of a sequencer pattern. This means you can automate 72 unique parameters across all 12 sequencer tracks. But if a paremeter is locked for a track, any step in this track can have a lock. Keep this in mind! When using p-locks on several tracks, it is easy to hit the memory ceiling.

#### Synth Parameters
Note: Synth Parameters differ between tracks. Not all parameters are available on all tracks.

| Arg Name            | Parameter                 | min         | max       | unit                                                   |
| ------------------- | ------------------------- | -----------:| ---------:| -----------------------------------------              |
| syn.0: or syn.tun:  | Synth Parameter 0         | 0           | 127       | integer                                                |
| syn.1:              | Synth Parameter 1         | 0           | 127       | integer                                                |
| syn.2:              | Synth Parameter 2         | 0           | 127       | integer                                                |
| syn.3: or syn.dec:  | Synth Parameter 3         | 0           | 127       | integer                                                |
| syn.4:              | Synth Parameter 4         | 0           | 127       | integer                                                |
| syn.5:              | Synth Parameter 5         | 0           | 127       | integer                                                |
| syn.6:              | Synth Parameter 6         | 0           | 127       | integer                                                |
| syn.7: or syn.lev:  | Synth Parameter 7         | 0           | 127       | integer                                                |

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
| fil.rel:            | Filter Envelope Release   | 0           | 127       | integer                                                |
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



## Built-in Numeric Functions

Spinal comes with a set of built-in functions which produce numbers that you can use for your sequencer programming adventures.
The syntax for using a function is: 

`function-name[<argument>,<argument>,<argument>,....]`
	
arguments are parameters for the function. Some of them are optional, and some functions work differently depending on how many arguments you put in.

You can use functions everywhere where you can use numbers and range-loops.

For example:

`trig randi[bd,cp] 1^16` - This will pick tracks `bd'sd'rs'cp` at random, and put down note events in the step range 1 to 16.

`trig lt 1^16 fil.frq:64+cyc[16]*32` - This will put down notes, and create a stepping sweep on the filter cutoff using p-locks, essentially giving you another LFO for free.

###List of Functions

#### RANDI - Random Integers
`randi[]` - random integer 0 to 1

`randi[<max>]` - random integer 0-max, e.g: `randi[3]` will spill numbers between 0 to 3.
	
`randi[<min>,<max>]` - random integer min-max, e.g: `randi[-3,3]` will spill numbers between -3 and 3.
	

#### RANDF - Random Fractional Numbers

`randf[]` - random float 0.0 to 1.0

`randf[<max>]` - random float 0.0 to max, e.g: `randf[3.5]` will spill numbers between 0.0 and 3.5.
	
`randf[<min>,<max>]` - random float min-max, e.g: `randf[-3.5,3.5]` will spill numbers between -3.5 to 3.5.

#### PICK - Pick a random element from a range-loop

`pick[<some range-loop>]` - chooses an element from the provided loop at random. Example: `pick[1'10'100]` spills out either 1, 10, or 100.
	

#### CHANCE - probability

`chance[<probability>]` - lets you specify a probability from 0.0 (not gonna happen) to 1.0 (definitely gonna happen). The output is either 0 or 1. 
	
This is very useful for setting any of the Rytm's TRIG FLAGS, such as whether or not to enable RETRIG for a certain step:

`trig bd 1^16 rt:chance[.5]` - 50% chance for each of the trigs to have RETRIG enabled.
	
	
#### Oscillator functions

Oscillators can be very useful for parameter lock programming. When used with p-locks, they are similar to the Rytm's built-in LFO in HOLD mode.
Experiment with adding or multiplying different oscillators!

#### CYC - sine oscillator

`cyc[<steps>]` - creates a sine wave in the range -1 to 1 which repeats every given steps. For example: `cyc[16]` will repeat every 16 steps.
	
`cyc[<steps>,<phase>]` - lets you specify a phase offset (0.0 - 1.0)
	
example:

    trig bd 1^16
	fil.frq:64+cyc[16]*32
	
this creates a sweep on the filter cutoff frequency. the sweep loops every 16 steps, and is thus nicely synced to the sequence.

    trig bd 1^16
	fil.frq:64+cyc[16]*cyc[4]*32
	
this creates a more complex LFO shape by multiplying two different sine oscillators with each other.

#### TRI - triangle oscillator

`tri[<steps>]` - creates a triangle wave in the range -1 to 1 which repeats every given steps. For example: `tri[16]` will repeat every 16 steps.
	
`tri[<steps>,<phase>]` - lets you specify a phase offset (0.0 - 1.0)
	
	
	
#### PUL - pulse oscillator

`pul[<steps>]` - creates a pulse wave in the range -1 to 1 which repeats every given steps. For example: `pul[16]` will repeat every 16 steps.
	
`pul[<steps>,<phase>]` - lets you specify a phase offset (0.0 - 1.0)
	
`pul[<steps>,<phase>,<pulsewidth>]` - lets you specify a pulsewidth (0.0 - 1.0)
	
	

	
	
	
	
	





[ffwd-gif]: https://raw.githubusercontent.com/jpenca/spinal-docs/master/spinal.gif "commands in this gif are randomly generated from a Max/MSP patch and sent over via UDP"
