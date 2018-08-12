=====================
UART/USART and stdio
=====================
Feb.24th, 2018 		Jack Lee			

----------------------------------												
STD IO based on UART/USART in BSP
----------------------------------
* Lowest layer: UART/USART driver;
* Second Layer: serial.h, encapsulated details of UART/USART
		
* Application Layer: stdio_serial.h
	* provides initializing interface	for UART or USART;
	* Abstract access to serial port with: stdio_base and 2 function pointers: ptr_put, ptr_get;
		* Defaultly, RTOS access 
				
* Standard C libary:
	* read.c and write.c: implement standard C fucntion of read(int fd, )	and write(int fd);
				
* LibC or no libc		
	* No matter with or without libc, read()/write() is interface with the std io functions;
			
	* When no libc
		 * stdio.c: 
				* when libc is not used, this file implements 'xxprintf' to output string only;
				* No input from this file;
	* When Libc:					
		* When nano C library is used, this file is not needed, same functions are provided by nano C;

^^^^^^^
Notes:
^^^^^^^
* bootloader without libc support, only a tiny stdio, so some formats in printf() can't be used. 
* For example "%lu"	and "%ld" are not parsed by stdio, so "%u" and "%d" only be used;