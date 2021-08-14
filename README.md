# Dell-Precision-E7530-Prototype-OpenCore
Install macOS on your Dell Precision 7530 workstations.

Supported OS: **macOS 12 DP4**

This repo utilizes **OpenCore MOD** for Windows and other OS compability.  

## Requirement
If you have a prototype "Precision E7530" laptop.

### What you should do if you're looking to install on a production 7\*30 or 7\*40?

The DSDT.aml includes three things: (1) fixes for the prototype ACPI; (2) Dell EV3 MUX patch; (3) I2C patch.

Although this is *not being tested*, you may still try deleting the DSDT.aml and adding the [EV3 patch](https://github.com/nnkn/hackintosh-dell-precision-7540-oc/blob/main/EFI/OC/ACPI/SSDT-EV3-BAN-PEGP-VINI.aml) and the [I2C patches](https://github.com/nnkn/hackintosh-dell-precision-7540-oc/blob/main/EFI/OC/ACPI/SSDT-I2C.aml) from nnkn's 7540 repo. I do suggest you to use mine as a base repo as the config.plist there is troublesome and outdated, unless you want to prepare everything from scratch.

## Post-installation tweaks
A few required or otherwise useful steps to take on a running macOS system on your laptop:

### Using your own serial number

Just generate a new set with OpenCore Configurator and you're good to go

### Fixing Sleep

In order to achieve a stable sleep, you're [responsible of disabling the following:](https://github.com/dortania/OpenCore-Post-Install/blob/master/universal/sleep.md)
```
sudo pmset autopoweroff 0
sudo pmset powernap 0
sudo pmset standby 0
sudo pmset proximitywake 0
sudo pmset tcpkeepalive 0
```

This will do 5 things for us:

1. Disables autopoweroff: This is a form of hibernation
2. Disables powernap: Used to periodically wake the machine for network, and updates(but not the display)
3. Disables standby: Used as a time period between sleep and going into hibernation
4. Disables wake from iPhone/Watch: Specifically when your iPhone or Apple Watch come near, the machine will wake
5. Disables TCP Keep Alive mechanism to prevent wake ups every 2 hours

Also make sure you disable the two BT power pins on the back if you're using DW1820a.

### Checking your PCI memory address

You have to compare your current DSDT with the patched DSDT, and change the memory address for certain built-in PCI devices to work. This is a _must_, because we use a DSDT injection here.

### Keyboard f-key

Instead of using an ACPI patch, Karabiner-Elements can help us achieve a software-based solution to emulate function keys as a real Mac keyboard. To achieve this, Karabiner-Elements is required to be installed. Under `Keyboard (Apple)`, we define `delete-forward` to `fn` and `keypad_num_lock` to `delete_forward`. Without sacrificing any key being used (`keypad_num_lock` does nothing on macOS), this best simulates a Magic Keyboard layout.

## Reference Specs

CPU: QNVH i7 8750H Engineering Sample (2.0, 2.9, 3.6; Overclocked to 2.0, 3.6, 3.6)

GPU: Coffee Lake UHD 630; AMD WX 4150 (fake-id to WX 4100, WX 3200 also supported)

MLB: Precision E7530, BIOS version 99.0.0, CFG Lock disabled by default

RAM: 3200 16G Dual Channel

SSD: Doesn't really matter

## What works

- Full hardware acceleration
- Audio (Speaker, Microphone, 3.5mm)
- Broadcom Wireless with AWDL
- Battery reading and charging recognition
- USB ports (Type-A and C)
- CPU Temperature and voltage/wattage reading
- Mini DP and HDMI output
- Sleep
- [SD card reader](https://github.com/0xFireWolf/RealtekCardReader/issues/3)
- Everything else

## What's untested

- Hibernation
- Thunderbolt 3

## What's not yet working

- N/A

## What will never* work

- TrackPad physical buttons except bottom left key (WIP: [VoodooI2C](https://github.com/VoodooI2C/VoodooI2C/pull/445))
- ALPS U1 Dual Pointing StickPointer Support ([See here for details](https://github.com/blankmac/AlpsT4USB/issues/8))
- Fingerprint
- Optimus. Currently only iGPU for internal screen, and only dGPU for external screens.
- TB3 Hot-Plug for Prototype NVM

_* Not unless someone decides to make custom kexts for these, of course._

## Credits

[nnkn's Precision 7540 repo](https://github.com/nnkn/hackintosh-dell-precision-7540-oc) and [OSXLatitude.com](https://osxlatitude.com/forums/topic/16159-solved-precision-7530-prototype-smbios-cannot-be-properly-injected-i2c-not-working-mux-control-in-bios-reset-after-restart/) for inspiring DDGPU fix and I2C fix

The /r/Hackintosh Discord and [Kishor Prins](https://github.com/VoodooI2C/VoodooI2C/issues/463) for providing DSDT.aml patches and patching ideas

And everyone else for creating kexts and kernel patches being used in this repo

### For 11.3+ Support

[u/JohnnyCesh](https://www.reddit.com/r/hackintosh/comments/nzsyqo/inspiron_15_r_se_7520_big_sur_115_beta_2_success/) for 11.3+ Brightness control fix. Could be useful for other MUX-active dual GPU laptop systems.

[Syncretic](https://forums.macrumors.com/threads/latebloom-an-experimental-workaround-for-the-11-3-race-condition.2303986/) for 11.3+ PCI race condition workaround. This fixes (1) Airport Card not working after cold or hot reboot and (2) sometimes not loading the OS due to PCI allocation problems. Could be useful for other systems with a lot of PCI devices.
