
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

* **Timestamp Type**
 * **Hardware timestamp**: time from PHY/MAC
 * **Software timestamp**: recv/sent time in network stack, system clock;

* **modes**
 * **E2E** (End to End): 2 nodes connect directly;
 * **P2P** (Peer to Perr): connect through boundary clock;

* **Message Types**
 * **Event**: port 319, time-critical, 224.0.1.119
 * **General**: port: 320;


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


**Clock Servo**

PHC, Ptp Hardware Clock vs System Clock

*********
Sync
*********

**Sync**: t\ :sub:`1`, t\ :sub:`2`

d\ :sub:`m2s` = t\ :sub:`1` - t\ :sub:`2`

T\ :sub:`m2s` = T\ :sub:`sync` = 2 seconds


**Delay Request**: t\ :sub:`3` , t\ :sub:`4`

d\ :sub:`s2m` = t\ :sub:`3` - t\ :sub:`4`

T\ :sub:`s2m` = T\ :sub:`sync` * U[2,30]

d\ :sub:`prop` = (d\ :sub:`m2s` + d\ :sub:`s2m`) /2 : one way delay filter

Δt = d\ :sub:`m2s` - d\ :sub:`prop`  : offset from master, eg. offset between clocks of master/slave


LP FIR:

LP IIR:

RMS, root mean square

**PI(Propotional Integreal) controller**


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

