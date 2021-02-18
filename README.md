Hi all,

First of all download longmynd from filipcrump site
Configure it in the way provided by author and compile it.
Download ffmpeg alternative from https://github.com/jocover/jetson-ffmpeg and compile it 


You may also find on the this repository my own compiled versions ready to use for Jetson Nano
I do not give cut my head (stake my live), but you may also needed some libraries,although I assume that it will not be needed.

So the easy way is to copy to your fresh Nvidia Jetson nano installation of Ubuntu files and subdirectories located in user-local directory.
I assume sudo will be available without a password.

Well, in user-local directory you may find two files: s1.sh and s2.sh wchich should be executable.

I would not treat it as a ready-made solution, but as a variation on the hardware and software capabilities of Jetson nano
You have a better solution - show me and I'll take a look.

For example, I'd like to see the gst-launch syntax in conjunction with the RTMP:// protocol, which includes the destination IP and port and it will not be UDP or RTSP://

If I have not this line of code, in the mean time I separated tuner (s1.sh) and codec (s2.sh).
In my case local Jetson nano IP was 192.168.0.120 so adapt please for your Jetson Nano address.

If you run:


```
sudo killall longmynd 
for be sure
```

then

```
./s1.sh
./s2.sh (you must change in it path accordin to yours)
# Note: in stream10.sh you should change IP to a good one (yours Jetson nano) 
```

after
```
screen -list
# then you will see two proces: wideo and tuner
```
if you wish to kill these processes you need to run:
screen -S wideo -X quit
screen -S tuner -X quit

Good Luck!
and
73


----------------------

Original from forked site:
# Longmynd

An Open Source Linux ATV Receiver.

Copyright 2019 Heather Lomond

## Dependencies

    sudo apt-get install libusb-1.0-0-dev libasound2-dev

To run longmynd without requiring root, unplug the minitiouner and then install the udev rules file with:

    sudo cp minitiouner.rules /etc/udev/rules.d/

## Compile

    make

## Run

Please refer to the longmynd manual page via:

```
man -l longmynd.1
```

## Standalone

If running longmynd standalone (i.e. not integrated with the Portsdown software), you must create the status FIFO and (if you plan to use it) the TS FIFO:

```
mkfifo longmynd_main_status
mkfifo longmynd_main_ts
```

The test harness `fake_read` or a similar process must be running to consume the output of the status FIFO:

```
./fake_read &
```

A video player (e.g. VLC) must be running to consume the output of the TS FIFO. 

## Output

    The status fifo is filled with status information as and when it becomes available.
    The format of the status information is:
    
         $n,m<cr>
     
    Where:
         n = identifier integer of Status message
         m = integer value associated with this status message
      
    And the values of n and m are defined as:
    
    ID  Meaning             Value and Units
    ==============================================================================================
    1   State               0: initialising
                            1: searching
                            2: found headers
                            3: locked on a DVB-S signal
                            4: locked on a DVB-S2 signal 
    2   LNA Gain            On devices that have LNA Amplifiers this represents the two gain 
                            sent as N, where n = (lna_gain<<5) | lna_vgo
                            Though not actually linear, n can be usefully treated as a single
                            byte representing the gain of the amplifier
    3   Puncture Rate       During a search this is the pucture rate that is being trialled
                            When locked this is the pucture rate detected in the stream
                            Sent as a single value, n, where the pucture rate is n/(n+1)
    4   I Symbol Power      Measure of the current power being seen in the I symbols
    5   Q Symbol Power      Measure of the current power being seen in the Q symbols
    6   Carrier Frequency   During a search this is the carrier frequency being trialled
                            When locked this is the Carrier Frequency detected in the stream
                            Sent in KHz
    7   I Constellation     Single signed byte representing the voltage of a sampled I point
    8   Q Constellation     Single signed byte representing the voltage of a sampled Q point
    9   Symbol Rate         During a search this is the symbol rate being trialled
                            When locked this is the symbol rate detected in the stream
    10  Viterbi Error Rate  Viterbi correction rate as a percentage * 100
    11  BER                 Bit Error Rate as a Percentage * 100
    12  MER                 Modulation Error Ratio in dB * 10
    13  Service Provider    TS Service Provider Name
    14  Service             TS Service Name
    15  Null Ratio          Ratio of Nulls in TS as percentage
    16  ES PID              Elementary Stream PID (repeated as pair with 17 for each ES)
    17  ES Type             Elementary Stream Type (repeated as pair with 16 for each ES)
    18  MODCOD              Received Modulation & Coding Rate. See MODCOD Lookup Table below
    19  Short Frames        1 if received signal is using Short Frames, 0 otherwise (DVB-S2 only)
    20  Pilot Symbols       1 if received signal is using Pilot Symbols, 0 otherwise (DVB-S2 only)
    21  LDPC Error Count    LDPC Corrected Errors in last frame (DVB-S2 only)
    22  BCH Error Count     BCH Corrected Errors in last frame (DVB-S2 only)
    23  BCH Uncorrected     1 if some BCH-detected errors were not able to be corrected, 0 otherwise (DVB-S2 only)
    24  LNB Voltage Enabled 1 if LNB Voltage Supply is enabled, 0 otherwise (LNB Voltage Supply requires add-on board)
    25  LNB H Polarisation  1 if LNB Voltage Supply is configured for Horizontal Polarisation (18V), 0 otherwise (LNB Voltage Supply requires add-on board)


### MODCOD Lookup

### DVB-S
```
0: QPSK 1/2
1: QPSK 2/3
2: QPSK 3/4
3: QPSK 5/6
4: QPSK 6/7
5: QPSK 7/8
```

#### DVB-S2
```
0: DummyPL
1: QPSK 1/4
2: QPSK 1/3
3: QPSK 2/5
4: QPSK 1/2
5: QPSK 3/5
6: QPSK 2/3
7: QPSK 3/4
8: QPSK 4/5
9: QPSK 5/6
10: QPSK 8/9
11: QPSK 9/10
12: 8PSK 3/5
13: 8PSK 2/3
14: 8PSK 3/4
15: 8PSK 5/6
16: 8PSK 8/9
17: 8PSK 9/10
18: 16APSK 2/3
19: 16APSK 3/4
20: 16APSK 4/5
21: 16APSK 5/6
22: 16APSK 8/9
23: 16APSK 9/10
24: 32APSK 3/4
25: 32APSK 4/5
26: 32APSK 5/6
27: 32APSK 8/9
28: 32APSK 9/10
```

## License

    Longmynd is free software: you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation, either version 3 of the License, or
    (at your option) any later version.
    Longmynd is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.
    You should have received a copy of the GNU General Public License
    along with longmynd.  If not, see <https://www.gnu.org/licenses/>.
