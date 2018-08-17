==========================
Work Log 
==========================
April, 2018	Jack Lee




**********************
Button Reboot/Factory
**********************

Default, GPIO is in low level, when press it, level from low to high, a rise-edge IRQ is emitted;
when it is unpressed, level from high to low, a fall-edge IRQ is emmited;
Set frequency of debouncing filter is 2 Hz, filting the glitch and douncing;

*. Set Rise-Edge interrupt

*. In ISR of Rise-Edge IRQ:

 * Configure fall-edge again;

 * Record current timestamp;

*. In ISR of Fall-Edge IRQ:

 * Comparing the delay with the threshold of factory reset (3 seconds)

 * Reboot or factory configure based on the result of comparison;


TC, Timer Counter
^^^^^^^^^^^^^^^^^^

Bug of Timer, count slow half than real time, so change the counter value in TC_RC (Register C):
divided by 2 more;


Aug.14th, Tuesday 2018

When stack size of mac task is assigned, the OS can't be used?
When timer task is assigned priority level of 4, OS can't be used?

Ideally, tftp/http send data in maxumum speed of 33 IRQs per second;

*********
MAC ISR:
*********

- Interrupt Status: IQRs
	- RX_Complete:
	- RX_Overrun:
	- HREST: HRESP not OK, DMA sees HRESP not ok;
	
	- TX_Complete:
	- TX_UR: transmit UnderRun, eg. failed
	- TX_TFC: failed because of AHB error;

- Receiving (register) Status: 
  -	OverRun: not receiving status at end of frame or HRESP not OK (HNO)
  
