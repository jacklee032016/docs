
Programming ATMEL chip with atprogram
#######################################
August.20, 2018	Jack Lee


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


