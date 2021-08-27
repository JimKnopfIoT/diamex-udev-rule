# Diamex udev rule
Udev rule for Diamex Prog-S2 programmer to prevent powermanagement issues.

I recently had some issues with a Diamex Prog-S2 programmer when i tried to programm an atmega2560 for my ChipTesterPro from 8bit-museum.de.
I use the programmer on a ASUS ROG G752VS laptop and i was able to use it on a Windows VM but it fails on my Arch Linux system.
The Programmer was recognized by the system. lsusb showed me a correct 

    Bus 001 Device 005: ID 16c0:2aa9 Van Ooijen Technische Informatica PROG-S2

and the device was present as /dev/ttyACM0. But when i tried to programm the atmega2560 with avrdude using this command:

        avrdude -c stk500 -P /dev/ttyACM0 -v -p atmega2560 -U flash:w:Chip-TesterPro-FW-v0.19.hex:i

i had an anoying error:

    avrdude: stk500_command(): command failed
    avrdude: stk500_getparm(): failed to get parameter 0x9a
  
I found a solution using tlp to blacklist the device (edit /etc/tlp.conf, blacklist the USB device, enable tlp, start tlp with "tlp start").
But since I didn't use tlp before, i didn't want to use it in the future, just to be able to use the programmer. I was looking for another solution.

The better way is, just to add a udev rule and add the (widly unknown) option ATTRS{power/control}=="on" to it.

The whole line should look like:

        SUBSYSTEM=="tty", ATTRS{idVendor}=="16c0", ATTRS{idProduct}=="2aa9", ATTRS{power/control}=="on" GROUP="adbusers", MODE="0666", SYMLINK+="diamex"

It's possible that you have to change SUBSYSTEM or GROUP option to match your settings.


