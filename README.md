nanoKontroller
==============

A very simplistic Python app to turn the controllers on a Korg nanoKONTROL2 device into user actions on Linux.

Config is controlled via a mapping file that ties inputs on the device to various events.

LED control will not work until the magic control sequence has been sent to the device. Use the Windows control software to switch to this mode.

Features
========

* Maps any button to a keypress on the host keyboard via evdev events
* Maps sliders or knobs to the volume of any audio device, including support for volume limiting or overdrive
* Maps any button to the mute control for any audio device
* Maps any button to an external shell command that includes the device key and value

Requires
========

* PulseAudio for audio control
* MIDO for MIDI control
* evdev for keyboard events

Config
======
By default config should be in ```~/.config/nanoKontroller.ini``` but this can be overridden by the ```-c```/```--config``` command line argument.

The ```[audioinputs]``` section defines audio devices to use as inputs, such as headsets or microphones. Similarly, ```[audiooutputs]``` defines audio devices to use as outputs, such as headsets or speakers. To help finding these names there is a ```--list-devices``` command line option to print them to the console.

The ```[keymap]``` section defines what to do with each button on the device.

Bare options such as ```KEY_PLAYPAUSE``` are taken directly from evdev as a fake keypress to generate.

Options prefixed with ```mute``` such as ```mute/mainOutput``` are used to tie a button to mute an audio device. In this case, the device ```mainOutput``` which maps to ```alsa_output.usb-0d8c_Generic_USB_Audio_Device-00.iec958-stereo```

Options prefixed with ```volume``` such as ```volume/mainOutput``` are used to tie a slider or knob to the volume of an audio device, in this case the device ```mainOutput```. An optional third argument can be supplied like ```volume/mainOutput/80``` to limit the maximum volume to 80% or ```volume/mainOutput/150``` to boost the maximum volume to 150%.

Options prefixed with ```exec``` are used to tie a button to executing a shell command. The values ```{NK_KEY_ID}``` and ```{NK_KEY_VALUE}``` are substituted by the device key ID number and the value (0 for button release, 127 for button press).


Example Config
==============
```
[audioinputs]
headsetInput = alsa_input.usb-Plantronics_SupraPlus_Wideband_USB-00.analog-mono

[audiooutputs]
headsetOutput = alsa_output.usb-Plantronics_SupraPlus_Wideband_USB-00.analog-stereo
mainOutput = alsa_output.usb-0d8c_Generic_USB_Audio_Device-00.iec958-stereo

[keymap]
PLAY = KEY_PLAYPAUSE
PREV = KEY_PREVIOUSSONG
NEXT = KEY_NEXTSONG
STOP = KEY_STOPCD
RECORD = KEY_RECORD
PARAM1_SLIDER = volume/mainOutput       
PARAM1_MUTE = mute/mainOutput     
PARAM2_SLIDER = volume/headsetOutput/150
PARAM2_MUTE = mute/headsetInput     
PARAM3_SOLO = exec/echo "Key {NK_KEY_ID}, value {NK_KEY_VALUE}"
