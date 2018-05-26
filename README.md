nanoKontroller
==============

A very simplistic Python app to turn the controllers on a Korg nanoKONTROL2 device into user actions on Linux.

It is very specificially hard-coded for my setup but may be of use for others to modify.

Features
========

* Maps bottom-left player controls to keyboard media keys so they will control virtually any audio/video app
* Maps the first slider and M button to control soundcard volume and toggle output mute
* Maps the second slider and M button to control a seconardy soundcard volume toggle *input* mute, intended for use with a headset for VoIP calls. Audio volume here goes up to 150% to boost audio on quiet calls.

Requires
========

* PulseAudio for audio control
* MIDO for MIDI control
* evdev for keyboard events
* GObject for notification messages
