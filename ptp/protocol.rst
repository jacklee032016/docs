
###################
Protocol of PTP
###################
Aug.16th, 2018


BMCA, Best Master Clock

*******
Terms
*******

* **Clock Types**
 * **Master Clock**: server;
 * **Ordinary Clock**: client;
 * **Boundary Clock**: server+client;
 * **Transparent Clock**: E2E TC and P2P TC; device don't skew the time calculation, just like wire;

* **Timestamp Type**
 * **Hardware timestamp**: time from PHY/MAC
 * **Software timestamp**: recv/sent time in network stack, system clock;

* **modes**
 * **E2E** (End to End): 2 nodes connect directly;
 * **P2P** (Peer to Perr): connect through boundary clock;

* **Message Types**
 * **Event**: port 319, time-critical, for messages which to be timestamped. 224.0.1.119;
 * **General**: port: 320; other messages;

* **Multicast Address**
 * 224.0.0.107: PTP, only for peer measurements; peer domain address;
 * 224.0.1.129: PTP, except peer delayer measurements; default domain address;


* **Messages**
 * **E2E message**
  * **Sync**: Event, s --> c, client get t\ :sub:`2`;
  * **Follow_Up**: , s --> c, server send t\ :sub:`1` in this message to client;
  * **Delay_Req**: Event, c --> s, client save t\ :sub:`3`;
  * **Delay_Resp**: s --> c, server reply t\ :sub:`4` to client;
 * **Peer message** 
  * **Pdelay_Req**: Event;
  * **Pdelay_Resp**: Event;
  * **Pdelay_Resp_Follow_Up**: transparent clock; 224.0.0.107
 * **Announce**: select grand master;
 * **Signaling**:
 * **Management messages**: 
  * TLV message;
  * Get/Set dataset member; 
  * Command to init an event;


Characters of OC/BC:

* priority1;
* priority2;
* clockClass;
* clockAccuracy;
* timeSource;
* offsetScaledLogVariance;
* numberPorts;


Port Status:
* MASTER;
* SLAVE;
* PASSIVE;


** PTP Domain**


**Clock Servo**

PHC, Ptp Hardware Clock vs System Clock

PTP epoch, POSIX epoch, from 1970, TAI;

*********
Sync
*********

Two-step clocks and one-step clock
===================================
**One-step clock**: Only SYNC message;
**Two-step clock**: SYNC + FOLLOW_UP;

Sync and calculation
===================================

**Sync**: t\ :sub:`1`, t\ :sub:`2`

d\ :sub:`m2s` = t\ :sub:`1` - t\ :sub:`2` = D + O

T\ :sub:`m2s` = T\ :sub:`sync` = 2 seconds


**Delay Request**: t\ :sub:`3` , t\ :sub:`4`

d\ :sub:`s2m` = t\ :sub:`3` - t\ :sub:`4` = D - O

T\ :sub:`s2m` = T\ :sub:`sync` * U[2,30]

d\ :sub:`prop` = (d\ :sub:`m2s` + d\ :sub:`s2m`) /2 : one way delay filter

Δt = d\ :sub:`m2s` - d\ :sub:`prop`  : offset from master, eg. offset between clocks of master/slave

D = (A+B)/2, Driff, Driff compensation, frequenct transfer;
O = (A-B)/2, Offset, Transfer correction: time transfer;


LP FIR:

LP IIR:

RMS, root mean square

**PI(Propotional Integreal) controller**

1 ppm: 1us/s;
nanosecond: 10\ :sup:`-9` second;


Profile
**************
Manage, query, update PTP dataset;
Signal and transport TLV;


**********
.. math::
**********

  Á_t(i) = P(O_1, O_2, ¡­ O_t, q_t = S_i ¦Ë)
  
\ :math:`d_m2m = t^{1} + t_{2}` 


.. math::

    \Delta

    e^{i\pi} + 1 = 0
         :label: euler
  
  
a\ :math: `\underline{x}=[  x_{1}, ...,  x_{n}]^{T}`


T\ :sub:`m2s` = t\ :sup:`2` 

The chemical formula for molecular oxygen is O\ :sub:`2`.


%%latex
\begin{equation}
\int_{-\infty}^\infty f(x) \delta(x - x_0) dx = f(x_0)
\end{equation}

