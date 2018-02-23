![FM Composer logo](http://fmcomposer.org/img/icon256.png)

http://fmcomposer.org

FM Composer is a music and sound creation tool, featuring a custom FM synthesizer engine and a tracker-like interface.
Released with 150+ FM instruments and drums, ranging from synth to acoustic sounds, covering the whole MIDI instrument set.

# Features
- 6 operator, 24 voice polyphony FM sound engine
- Tracker-style sequencer, max 254 patterns of 1-255 rows.
- Lots of effects available : vibrato, tremolo, arpeggio, pitch slides, real time FM parameter modification, loop points...
- MIDI integration : MIDI file import with partial XG/GS support, MIDI keyboard support
- Wave and MP3 exports
- Piano roll view

# Why
Modern world lacks modern trackers. And there is very few custom sound engines in the VST market, most of them are emulating old existing hardware.
This is a attempt to make a good yet minimalistic FM synthesizer, with a very graphical, easy to use interface.
This is also an attempt to provide real time sound synthesis for video games. Since the FM engine is a separate module with very few dependencies, it can be intergrated into other projects. Playback with real time modification of the song is possible for interactive content.

# Technical overview
GUI is written in C++ and using the SFML library to get low level access to the display, keyboard and mouse input. Each GUI element has its class. They are used by Views which are the actual program screens (Pattern screen, Instrument screen, Settings etc.). Views, for most of them, contain everything they need for providing the features to the user. This results in quite big classes, which are split in several files for readability.

The audio engine is written in pure C and was optimized for lowest CPU usage possible (cache-friendly structures, loops on small data sets, no function calls in critical parts, taking advantage of unsigned int wrapping for phase accumulators and so on). A part of the optimization relies on the use of FISTP instruction instead of the standard C ftol() for int/float conversions. This is done through the /QiFist commandline on Visual Studio. Maybe even better optimizations could be made with SSE or other tricks I'm not aware of.

FM Composer uses its own binary format, FMCI for storing instruments and .FMCS for the songs . They use LZ4 compression (https://github.com/lz4/lz4). I'll write a document describing how they work, if you need them *now* just tell me.

# What needs to be done

## Builds for other OS
I chose only cross platform libraries for this project, and added #defines in my code when I called OS-specific functions. It should be *relatively* easy to make it work on Linux and MacOS. However I lack time for this stuff. If someone want to have fun with that, any help is welcome !

## ... (to be defined)

# Compiling
The project compiles fine under Visual Studio 2013. You'll need the following additional libraries :
- SFML
- PortAudio
- LAME

For the release I modified slightly the SFML library to support file drop, get a crispier font rendering and support a missing keyboard key. You should still be able to compile with the official version if you comment the lines relative to file drop (the other changes i did doesn't impact the API)
