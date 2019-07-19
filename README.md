Teletext for Raspberry Pi
-------------------------

This software generates a teletext signal in software. No hardware
mods are needed.

This support both PAL (Teletext) and NTSC (CEA608 format captions).

(The demo mode should no longer crash TVs!)

Usage:

Have a Raspberry Pi connected to a TV by composite video.
It doesn't matter if you are running X or not. Dispmanx will draw over
anything else.



Derail

Here is how for RPI 1-3: https://www.raspberrypi.org/forums/viewtopic.php?t=165943
```
Dear Ceci123,
The Raspberry Pi Model B+ and up, including the Raspberry Pi 3, all have the RCA jack unified
with the 3.5 mm audio jack, you'll need a hybrid cable which has all three of the RCA output
jacks, (yellow, white and red).

After connecting it to a TV (CRT), you'll notice that it isn't enough, so you'll need to tweak
it by editing the config.txt. There are three things you need to modify.

1. #sdtv_mode=0
Search for that line in /boot/config.txt. Remove the hash symbol (uncomment it) and change the
0 after the = symbol to what your CRT TV's mode is from this list.

sdtv_mode=0 Normal NTSC
sdtv_mode=1 Japanese version of NTSC – no pedestal
sdtv_mode=2 Normal PAL
sdtv_mode=3 Brazilian version of PAL – 525/60 rather than 625/50, different subcarrier

2. hdmi_ignore_hotplug=1
Add the line in the brackets here (hdmi_ignore_hotplug=1) to the bottom of /boot/config.txt

3.hdmi_force_hotplug=1
Search for that line in /boot/config.txt . What you've got to do is to add a hash symbol in front
of it, or comment it like this: #hdmi_force_hotplug=1.

And you are done!
Hope this helps you!
Some info from this was forked from ( Link #1 below ) and ( Link #1 below. ) Disclaimer:
I don't own these articles, therefore give credit to the original authors.
Thank you so much!
Hope you appreciate it,
matteopio
```

https://bhavyanshu.me/tutorials/force-raspberry-pi-output-to-composite-video-instead-of-hdmi/03/03/2014
http://www.instructables.com/id/Raspberry-Pi-2-Quick-n-Easy-RCA/

Derail done



Build the programs:

    make

Twiddle the registers:

    sudo ./tvctl on

Ensure you see the message "Teletext output is now on."

If you're connected via PAL, run the demo:

    ./teletext

and press the text button on your TV remote.

If you're connected using NTSC, run the demo:

    ./cea608

and enable the TV's closed caption controls to show CC1.

Detailed Usage
--------------

    tvctl on|off

This tool prepares the composite out for teletext transmission by
shifting the output picture into the VBI area. It will check the
registers are in a known state before doing anything. "on" and
"off" commands will have no effect if the state is already on or
off, or if the registers are in an unknown state.

    teletext <->

Running with no arguments will show a demo. Running "teletext -"
will read packets from stdin and display them. You can therefore
pipe packets from another tool, possibly over the network with ssh
or netcat.

The packet format is 42 byte raw binary packets, without the clock
run-in. Each packet received will be transmitted once, so you must
send packets endlessly. See

http://www.etsi.org/deliver/etsi_i_ets/300700_300799/300706/01_60/ets_300706e01p.pdf

for details of the teletext protocol.

    cea608 <->

Running with no arguments will show a demo.  Running "cea608 -"
will read data from stdin and display.  The data is in binary format,
with each field represented as two bytes of parity-encoded data.
This is the "raw" format some capture tools can output.  Data is
output at 59.97 fields per second.  See

https://en.wikipedia.org/wiki/EIA-608

for details on the protocol and links to the specifications.
