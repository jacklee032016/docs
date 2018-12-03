
Scheduler and Performance of RTOS
#####################################
Dec.2nd, 2018	Jack Lee

Background
=================

Parameters
-----------
* Task priority of key tasks(**IRQ**, **MAC**, **IP**, **HTTP|UDP|Client**);
* time slice of RTOS;
* period of **TCP task**'s timers;

  * consume most of CPU processing time, and make **MAC task** not receiving packet, **HTTP task** count to close;
* Polling count of **HTTP Server**;

Time Slice:
------------

* 1000Hz: schedule 1000 times/second, 1ms period for every scheduling;
* 100Hz: schedule 100 times/second, 10ms period for every scheduling;

Maybe 50Hz, is good option for SAME70Q21

Notes
------------------
* When tasks are in different priorities, time slice does not matter the performance;

  * except higher task block itself, lower task can not execute;
  * time slice should be little longer in order to minimize the time spent on scheduler;
* When tasks are in same priority, time slice determine the performance

Optimizing for UDP command task
------------------------------------

* Time slice: 1000Hz;
* Task priorities: **MAC**:4; **TCP**:3; **UDP**:4;

Before Optimizing
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
* Time slice: 100Hz; same priorities for only 2 task: **MAC**:4 and **TCP**:3;
* UDP response is too slow: **TCP task** do not has enough CPU time;

* **MAC task**: 4, so **TCP task** can't get enough CPU time, and **MAC** congested if packets bursted;


Problemtically existed schema
----------------------------------
* Time slice: 1000Hz;
* Task priorities: **MAC**:5; **TCP**:6; **HTTP**:7;
* 

Problems
^^^^^^^^^^^^^^^^^^
After 3-way handshake, TCP is established, **TCP** and **HTTP** tasks are scheduled, then **MAC task** can not be scheduled, 
so no further packets are received, then HTTP connection are closed by count to zero;

Before 3-way handshake, **MAC task** receives packets because high priority tasks are always blocked;


Scheduling Schemas
=======================

MAC highest priority
----------------------
* It is easy to consume all of **PBUF_RAM** by **MAC task**;
* So it is not a suitable option;


Same peiorities for all key tasks
------------------------------------
* schedules every tasks in time slice one by one;
* task can block itself;
* If packets arrive burstly, memory can be used up quickly;


**MAC task** : lowest priority
----------------------------
**TCP** and **HTTP** tasks maybe been starved???

**HTTP task** should be high priority than **TCP task**, because it is feed by **TCP task** and send back data in MAC layer directly;

If no data or message, **HTTP task** always waits;

So, the parameters can be tuned are time slice and time of **TCP task**'s timer:

* First, enlarge the time to about 500 ms for slow timer;
* Secondly, change time slice from 1000Hz to 100Hz or 50Hz;

* Test and try different parameters;

