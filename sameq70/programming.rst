
Programming ATMEL chip with atprogram
#######################################
August.20, 2018	Jack Lee

Modifying GPNVM
=============================
Oct.17, 2018

References:
* 9.1.5.11 GOVNM bits
* 20.3 section

Get info of Chipset
--------------------

::

	atprogram -t atmelice -i swd -d atsame70q21 info
	
Read out contents:

::

		Target voltage: 3.32 V
		
		Device information:
		
		Name:       atsame70q20
		JtagId:     N/A
		Revision:   A
		Chip ID:    0xa1020c00
		CPU arch.:  CORTEX-M7
		Series:     SAME70
		
		Security bit is set.
		
		Memory Information:
		
		Address Space    StartAddress            Size
		
		base                      0x0     0x100000000
		  PERIPHERALS      0x40000000      0x20000000
		  SYSTEM           0xe0000000      0x10000000
		  QSPIMEM          0x80000000      0x20000000
		  AXIMX            0xa0000000        0x100000
		  ITCM                    0x0        0x200000
		  IFLASH             0x400000        0x100000
		  IROM               0x800000          0x4000
		  DTCM             0x20000000         0x20000
		  IRAM             0x20400000         0x60000
		  EBI_CS0          0x60000000       0x1000000
		  EBI_CS1          0x61000000       0x1000000
		  EBI_CS2          0x62000000       0x1000000
		  EBI_CS3          0x63000000       0x1000000
		  SDRAM_CS         0x70000000      0x10000000
		
		fuses                     0x0             0x1
		
		lockbits                  0x0             0x8
		
		GPNVMBITS (0b0000000000000010 <-> 0x0002):
		   TCM_CONFIGURATION 0x0
		   BOOT_MODE     1
		   SECURITY_BIT  0
		
		LOCKBIT_WORD1 (0b00000000000000000000000000000000 <-> 0x00000000)
		LOCKBIT_WORD0 (0b00000000000000000000000000000000 <-> 0x00000000)	

Only when GPNVM is 0x0042, everything is OK;
	
Background of updating fuses
------------------------------
	
Setting GPNVM bits is done with the commands for setting "Fuse bytes". Here is an example that sets GPNVM BIT Boot_Mode to 1 and GPNVM bit6 to 1 and others to 0:


  atprogram -t edbg -i swd -d atsamd20j18 write -fs -o 0x804000 --values FFC7E0D85DFFFFFF
    Program 0xD8E0C7FF to USER_WORD_0 and 0xFFFFFF5D to USER_WORD_1 of an atsamd20j18 device. Mind the data byte order.

	atprogram -t atmelice -i swd -d atsame70q21 write -fs -o 0 --values 0x42
	
	D:\rtos\rtosLwip\an767>atprogram -t samice -i swd -d atsame70q20 write -fs --values 0100001000| 03000042 -v : write 0x03

Update: Please notice you don¡ät come arround of touching bit #6 ("Reserved") with this method. Atmel support confirmed to me it should always be set to 1 (I had some strange behavoiur when it was set to 0).

::

  atprogram -t samice -i SWD -d ATSAME70Q20 write -fs -o 0 --values 0x42

     -fs : write to fuses
     -o  : offset

  atprogram -t samice -i SWD -d ATSAME70Q20 read -fs -o 0


Value Format
---------------------------
 ``low/high/ext``
 
 GPNVM only 9 bits, but atprogram look it as 16-bit, 0xLLHH, LL is bit0-bit7, HH is bit8-bit15;


Tests and ``read -fs``
----------------------------

Step 1: erase chip, so not bootloader running

::
   atprogram -t samice -i swd -d atsame70q20 chiperase

   atprogram -t samice -i swd -d atsame70q20 info

Step 2: 

::

   atprogram -t samice -i swd -d atsame70q20 write -fs --values 0x4200 -o 0 -v
   atprogram -t samice -i swd -d atsame70q20 info

   atprogram -t samice -i swd -d atsame70q20 write -fs --values 0x0000 -o 0 -v
   atprogram -t samice -i swd -d atsame70q20 info
   
   atprogram -t samice -i swd -d atsame70q20 write -fs --values 0x0200 -o 0 -v
   atprogram -t samice -i swd -d atsame70q20 info


**Don't used ``read -fs``, just use ``info``:**

When GPNVM=0x0040, read out:

::

		:0100000040BF
		:00000001FF

When GPNVM=0x0002, read out:

::

		:0100000002FD
		:00000001FF



Help of atprogram

::

   atprogram help CMD


Hardware ERASE 
=================
When SECURITY bit of GPNVM is 1, JTAG can't connect to chip and update firmware. This is so-called **security**;

* **ERASE** pin, pin87, connect to R37; R38 connect to Power positive pin;
* connect R37 and R38, then every bits in GPNVM is cleared;
* ``atprogram`` program flash, and GPNVM is changed to 0x02 automatically; bootloader can boot at once;



Programming fails without reason
==================================

::

 atprogram -t samice -i swd -d atsame70q20 -l d:\access.log  program -f *.elf


Debugging
^^^^^^^^^^^
	
1. **SEGGER JLink to connect to device**:
	
	Jlink v618a, jink.exe
::	

		connect: --> ATSAME70Q20 --> SWD --> 4000Khz
		
OK. It means Jlink and its driver jlink64.sys,	JLinkCDC_x64.sys and libusb works fine;
|Otherwise, reinstall JLink driver;


2. **Debugging atprogram**:

::

  atprogram -t samice -i swd -d atsame70q20 -l d:\access.log -v info

Then check the log file and its alert: ``hil No device files were found for the specified device``

Start Atmel Studio, and in 'Tools/Device Programming' menu: 

 * Tool of SAM-ICE is found;
 * but 'device list' is null;


3. **Amtel Studio**:

In menu of 'Tools/Device Pack Manager' to restart the device installation;


4. **Programming device**:

Then, everything is OK!

**Note**: 

After 'Device Pack Manager' installs device files, never start Atmel Studio again!!!


