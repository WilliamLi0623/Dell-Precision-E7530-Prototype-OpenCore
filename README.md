# Dell-Precision-E7530-Prototype-OpenCore
Install _cursed_ macOS **only** on your Prototype Dell Precision 7530 workstations.

## Requirement
If you have a cursed "Precision E7530" laptop.

**What you should do if you don't?**

Scroll to bottom, go to the nnkn repo, and use their EFI instead. Regular 7x30 and 7x40 EFI should be interchangable and compatible.

## Post-installation tweaks
A few required or otherwise useful steps to take on a running macOS system on your laptop:

**Using your own serial number**

Just generate a new set with OpenCore Configurator and you're good to go

**Disabling Power-related Settings**

In order to achieve a stable sleep, you're responsible of disabling the following:
- Power Nap (if available)
- Wake for Internet Access
- tcpkeepalive (hint: `sudo pmset -a tcpkeepalive 0`)

**Check your PCI memory address**

You have to compare your current DSDT with the patched DSDT, and change the memory address for certain built-in PCI devices to work. This is a _must_, because we use a DSDT injection here.

## What works

- Full hardware acceleration
- Audio (Speaker, Microphone, 3.5mm)
- Broadcom Wireless with AWDL
- Battery reading and charging recognition
- USB ports (Type-A and C)
- CPU Temperature and voltage/wattage reading
- Mini DP and HDMI output
- Keyboard and F-key (F11/ F12 as brightness keys)
- Sleep

## What's untested

- Hibernation
- Thunderbolt 3 (hot-plug not supported)

## What's not yet working

- N/A

## What will never* work

- TrackPoint and all physical buttons except bottom left key (WIP: VoodooI2C)
- SDHC (WIP: RealtekPCIeCardReader)
- Fingerprint

_* Not unless someone decides to make custom kexts for these, of course._

## Credits

https://github.com/nnkn/hackintosh-dell-precision-7540-oc and OSXLatitude.com for inspiring DDGPU fix and I2C fix

The /r/Hackintosh Discord for providing DSDT.aml patches and patching ideas
