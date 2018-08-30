
Timers in LwIP Protocol  
########################################
Aug.29, 2018	Jack Lee


Timeout Theory
===================
Port library and its mailbox provide the timeout machanism of LwIP:

# **sys_arch_mbox_fetch()** which can define timeout to fetch msg from mailbox;
# **tcp_ip thread** will fetch new message(pbuf) from its mbox;
# If support timeout and timers, tcp_ip thread will fetch message with **sys_timeouts_mbox_fetch()**
 * **sys_timeouts_mbox_fetch()** is defined in timeout.c of Lwip, which can call all timers of protocol stack and user defined timers;
# Otherwise, tcp_ip thread will fetch message with **sys_mbox_fetch()**
 * **sys_mbox_fetch()** is a macro of **sys_arch_mbox_fetch()**

Notes: 
----------
Difference between sys_arch_xxx() (in sys_arch.c) and its lwip version of sys_xxx() (in sys.h);


Timeout and Timers in LwIP
===========================

2 Types of timers in LwIP
---------------------------
cyclic timers
^^^^^^^^^^^^^^^
Timers are intenal in protocol stack, such as **tcp fast timer**, **tcp slow timer**, and **igmp_timer**;
 
system_timeo
^^^^^^^^^^^^^^
Real timers used by timeout machanism in LwIP, every timer construct a link list, contains cyclic timers and other client defined timers;

Both cyclic timers and user's timers are created as one instance of system_timeo with dynamic memory;


Implementation
------------------

sys_timeouts_mbox_fetch() 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
# Calculate the most recent timeout time from timers list;
# Fetch new message with this timeout in tcp_ip thread;
# If no new message is available and timeout happens, it loop the timers list and call the handler of the selected timer;


Performance
^^^^^^^^^^^^^^^
# Whether timeout is busy-waiting or not, it is dependent on sys_arch, normally for rtos, it is not;
# Timeout time of some cyclic timers should be longer to improve the performance;

