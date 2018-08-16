=================================
Performance Testing and Debugging 
=================================

Aug.15th, Wednesday 2018


-------------------------------------
What is the limitation of performance
-------------------------------------

- Ideally, tftp/http send data in maxumum speed of 33 IRQs per second;
- ttl is about 0.5 ms for ICMP response;

This is the limitation;

------------------------------
Debugging and Testing
------------------------------

- IP layer also drops a lot of packets, about 30% packets;

  - Disable promiscuous mode and enable multicast mode in MAC controller;
  - boardcast mode must be enabled in MAC controller;

- Disable ETH and ARP flags in net_if initialization;

- Disable TCP_LOCK mode;

- from **sys_timeouts_mbox_fetch()** --> **sys_arch_mbox_fetch(,,0)**, so avoid busy-waiting;

-------------------------------
LwIP Task/Process Architecture
-------------------------------


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
2 Tasks/Threads for PCB protocol stack
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- MAC/NIC Thread

	- Receive packets from hardware, handle mutex access with IRQ service routing
	
	  - ISR is registered in CPU IRQ table;
	  
	- Send packets to LwIP tcpip.c (tcpip thread);
	
- tcpip thread

  - handle TCP connections;
  - handle APIs based on MSG, eg. upper layer protocols;
  - timeout mechanism of TCP is busy-waiting: **sys_timeouts_mbox_fetch()**;
  
    - Timer task of FreeRTOS is not used in LwIP, because the sys_arch port of amtel is not implemented;
  
  - When TCP_LOCK is defined:
  
    - ICMP/UDP/ARP protocols are handled by MAC/NIC task;
    
  - otherwise:
  
    - TCP_INPUT mailbox are used, and packets are sent to mailbox;
    - ICMP/UDP/ARM protocols are handled by tcpip task;
    
  - mutex clock is not defined by Amtel version sys_arch;
  - so only mailbox can be used;



OS emulation layer

* timer: only for TCP, one-shot timers;
* process synchronization: semaphore;
* message passing: mail box:
  - post: not block, enqueued;
  - fetch: another process;

OS+No Net Connection --> remove tcpip thread?

  
^^^^^^^^^^^^^^^^
mailbox for TCP  
^^^^^^^^^^^^^^^^

When mailbox of TCP_MSG is used, every msg for one pbuf must be stored in mailbox.
For FreeRTOS, mailbox is a queue, os queue length must be defined;

So for every pbuf, one msg and one position in queue must be used. so these 2 parameters 
of **TCP_MSG_INPUT** and **MAILBOX_SIZE** should be same;


- **TCPIP_MBOX_SIZE**: for tcp msg input;
- **DEFAULT_ACCEPTMBOX**: for API based on msg;
- **DEFAULT_TCP_RECVMBOX**: for upper layer APIs;


When stack size of mac task is assigned, the OS can't be used?

When timer task is assigned priority level of 4, OS can't be used?

Ideally, tftp/http send data in maxumum speed of 33 IRQs per second;

------------------------
MAC Controller and ISR
------------------------

Conculusion

 - The necklace of performance is MCU and software of protocol stack;
 - ICMP is simplest and quickly responded protocol, which can be used for performance testing;
 - MAC controller is much faster than MCU;
 - Minimize the number of RXed packets in MAC controller is key of performance improving;

^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Debugging and Configuration
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

 - Only the nessary IRQs should be active, eg. only RX IRQs are enabled;
 - TX IRQs are disabled: TX IRQs are not for transmission, and they are not emitted;
 
 - When number of RX buffers reach 7, RX overrun seldom happens;
 - Enable boardcast, and mulitcast on MAC controller;
 - Disable promiscuous mode, which make a lot of useless packets are received, and degrade the performance greatly.
 - Every RX IRQ must be cleared in Receiving Status Register after it happens;

^^^^^^^^^^^^^^^
MAC controller
^^^^^^^^^^^^^^^

- Interrupt Status (register): IQRs
	- RX_Complete:
	- RX_Overrun:
	- HREST: HRESP not OK, DMA sees HRESP not ok;
	
	- TX_Complete:
	- TX_UR: transmit UnderRun, eg. failed
	- TX_TFC: failed because of AHB error;

- Receiving Status (register): 
 - OverRun: not receiving status at end of frame or HRESP not OK (HNO)
  
