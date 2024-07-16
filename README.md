# GhostTree permanent install at Exploration Place 2024      

## Overview
 - main code is `effectTry2.pd`
 - Code creates sound effect from mic input and sends amplitude data to independent LED instance
- Amplitude data sent via TCP on port 9990 in the form `amplitude XX;<CR>` where `XX` is a float
  - range of amplitude calculated in code to always be between 0 and 100 regardless of start parameters
   
## Requirements 
 - Pd >= 0.54-1 
 - iemlib
 - else >= 1.0-0 rc-11

## Start Parameters
- **sensitivity** (int 0-80 default 35): amp variation needed to detect activity. Higher = less sensitive
 - **windowSize** (int power of 2 default 1024): number of samples averaged to calculate amplitude. Freq of amp set will then be freq*2/windowSize per sec
   - *freq is set by jack audio, currently at 12000Hz for the install*
 - **wet** (float 0-1 default 0.5): amplitude of signal fed back into input (echo)
 - **micNoisegate** (float 0-80 default 60): noise level below which LEDs are uneffected
 - **exponent** (float 0-10 default 2): power exponent applied to the amplitude for the LEDs

## Execution
 - Command: `Pd -nogui -send "sensitivity [X]" -send "windowsSize [Y]" -send "wet [Z]" -send "micNoisegate [A]" -send "exponent [B]" -path [IEMLIB PATH] -stderr effectTry2.pd`
 - Example: `Pd -nogui -send "sensitivity 35" -send "windowSize 1024" -send "wet 0.6" -send "micNoisegate 60" -send "exponent 2" -path [IEMLIB PATH] -stderr effectTry2.pd`

## Debugging
It's possible to run the Pd patch with the gui displayed on the host machine. Steps:
 1. run Pd: `Pd -guicmd "" -send "sensitivity [X]" -send "windowSize [Y]" -send "wet [Z]" -send "micNoisegate [A]" -send "exponent [B]" -path [IEMLIB PATH] -stderr effectTry2.pd`
 1. find out port Pd GUI will use: `netstat -ltnp`
 1. in a new terminal on the host machine, log in forwarding the Pd GUI port: `ssh user@machine -L [HOST PORT OF YOUR CHOICE]:localhost:[PD PORT SHOWN BY NETSTAT]`
 1. run Pd GUI on host machine: `pd-gui [HOST PORT OF YOUR CHOICE]`

### Debugging gotchas
 1. If you save the patch, it will save on the remote machine (GOOD!)
 1. If you try to load a patch, it will try to load it on the host machine (BAD!)

