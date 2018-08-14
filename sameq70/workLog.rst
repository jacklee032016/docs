==========================
BISST (Built-In Self Test)
==========================
April, 2018	Jack Lee


Aug.14th, Tuesday 2018

When stack size of mac task is assigned, the OS can't be used?

MAC ISR:
	
- Interrupt Status: IQRs
	- RX_Complete:
	- RX_Overrun:
	- HREST: HRESP not OK, DMA sees HRESP not ok;
	
	- TX_Complete:
	- TX_UR: transmit UnderRun, eg. failed
	- TX_TFC: failed because of AHB error;

- Receiving (register) Status: 
  -	OverRun: not receiving status at end of frame or HRESP not OK (HNO)
  
