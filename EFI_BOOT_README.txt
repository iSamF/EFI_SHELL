EFI BOOTABLE USB KEY FOR DEBUG - Public
Rev 1.0
7/6/15
By Sam Fleming

INTRODUCTION - PURPOSE
======================
The purpose of the files in this EFI_BOOT_DEBUG.ZIP file are to create a single USB key that is useful for system debug in just about any S/W environment.  Once the files are unzipped onto a USB key, the USB key itself can be used to boot a system into the EFI shell.  The EFI shell files are located on the key itself!  The EFI shell environment can then be used to perform most basic H/W accesses (memory, IO, PCI).  

There are additional tools contained on the key that are useful in other operating system environments:
	- RW Everything (Windows)
	- samtool (Linux)

Installation is simple; just unzip everything onto a USB key.  
	File = EFI_BOOT_DEBUG.ZIP

Details on the various tools and programs contained on the key are provided below:


==============================================================================
EFI SHELL
=========
Booting:
--------
Booting to the EFI Shell program that is contained on the USB key is easy:
	1)  Enter the system's BIOS setup menu to boot directly to the key.  Make sure to choose the UEFI boot option for the USB key!  This will boot the system into the EFI shell.  Example:
	Boot Manager Menu => UEFI SMI Corporation USB DISK AA09012700007503
	2)  Once in the EFI shell, you will still need to "mount" the USB key in order to access the files on the key.  There should be a list of all available media (drives, USB key, etc) displayed after booting.  Find the USB key and simply type in the name of the media followed by a ":".  Example:
		Shell> fs0:
	3)  A list of the files is done by typing "ls".  Example:
		fs0:\ ls
	4)  "cd directory-name" to move around directories.
	5)  "cd .."  to move up a directory level.

Basic EFI SHell Debug Commands:
--------------------------------
*  Help on the primary Debug Command, "mm":
		help mm -b
*  Accessing Memory Example (Memory Read, four bytes from 0xFFFFFFF0; the Boot Vector):
		mm fffffff0 -w 4 -mem
*  Accessing IO Example (IO Write of one byte from 0x12 to 0x80; Port 0x80 Display):
		mm 80 12 -w 1 -io
*  Accessing PCI Registers Example (PCIe Read of 4 bytes from Segment=0, Bus=0, Device=0, Register 4):
		mm 00000000004 -w 4 -pcie
        (ssbbddffrrr) - segment, bus, function, register


==============================================================================
RW Everything - Windows Tool
=============================
This tool can access just about any register in a system that is booted to Windows.  The key includes both 32 and 64 bit versions of the RW Everything Tool. 

To execute the tool, just double-click the Rw.exe icon in the 		
	/RW_Everything/x64 	or 
	/RW_Everything/x32 
directory.

The tool is pretty straight forward to use.  There are icons at the top of the screen that are used to access PCI, memory-mapped, IO mapped, and MSR type registers.

The latest version can be downloaded here:  http://rweverything.com/


==============================================================================
samtool - Linux Tool
====================
This program can be used on most linux builds in order to read and write to H/W registers (memory, IO, PCI, MSRs).

1)  On a linux box, copy the files in the /samtool directory onto the Linux box.
2)  Execute the following command.
	 > chmod 755 samtool
3)  Run the tool
	 > sudo ./samtool ?							(this brings up the basic help menu)
4)  Usage examples:
	> sudo ./samtool mem 0xFFFFFFF0 d 0x10 (Reading from memory 0xFFFFFFF0)
	> sudo ./samtool io 0x80					(Reading from I/O Location 0x80)
	> sudo ./samtool pci 0x00:0x00.0x00 	(Reading from PCI device: bus 0,
														 device 0, function 0)
	> sudo ./samtool msr 0x10					(Reading from Time Stamp Counter MSR)

Peruse the help menus for more detailed examples.

The latest version can be downloaded here:  https://github.com/Intel-ADSC/samtool


==============================================================================
CONTENTS OF BOOTABLE EFI SHELL USB KEY (EXECUTABLE FILES):
==========================================================
/efi/boot
	bootia32.efi
	bootx64.efi
	(Choose to boot from usb key (UEFI Choice), system will boot to efishell)
/RW_Everything
	/x32
	(rw.exe to run program)
	/x64
	(rw.exe to run program)
/samtool
	(sudo ./samtool on linux machine)