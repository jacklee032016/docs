IGMP, Internet Group Management Protocol
*****************************************
Aug, 2018 Jack Lee

Protocol
=========

- All systems: 224.0.0.1
- All routers: 224.0.0.2

L3 protocol;

* **Message Type**:
 * **QUERY**: router send to all systems;
 * **REPORT**: v1/v2; systems send out with dest IP is group IP; v1:res_time is 0; v2: max res value;
 * **LEAVE**: v2 only; send to all routers;

* **Notes:**
 - Group address for all message are kept in group_address field of message payload;
 - After receiving QUERY, all nodes wait random time below response time, and only one node responses normally;
 - Group or group address is related with one network interface, no matter in which platform, Linux or LwIP;
  - IGMP and multicast traffic should be initialized with explicit network interface;


* **IGMP router and IGMP system**
 * **IGMP router**: router which can send/forward IGMP messages, and forward multicast traffic;
 * **IGMP system**: node which want to receive mulitcast traffic and send out IGMP REPORT and IGMP LEAVE messages;


* **Common Group**
 * 224.0.0.1: all systems
 * 224.0.0.2: all routers
 * 224.0.0.24: IGMP?
 * 224.0.0.251: MDNS
 * 224.0.0.107: PTP, only for peer measurements;
 * 224.0.1.129: PTP, except peer delayer measurements;
 


LwIP Implementation
====================

* **states**
 - **NOT_MEMBER**: new
 - **DELAY_MEMBER**: Afte join one group and start timer;
 - **IDEL_MEMBER**: after timeout and send REPORT message, or receive REPORT message from other systems;
 
 
* **timer**
 * When one group REPORT has been send out, start timer as 10;
 * When QUERY message is received, start timer as res_time defined in QUERY message;


Debegging multicast traffice
******************************

Debugging with Linux
=====================

ifconfig eth0 multicast

check kernel

::

 grep -i multi /boot/config-<Kernel version>
         * CONFIG_IP_MULTICAST=y    <¨C
         * CONFIG_IP_ROUTER=y
         * CONFIG_IP_MROUTE=y
         * CONFIG_NET_IPIP=y  

Below command will allow eth0 to send multicast traffic::

	route add -net 224.0.0.0 netmask 240.0.0.0 dev eth0

Verify the Mutlicast groups in our current system

::

  netstat -g

List add groups in every NIC interface, eg. IGMP join this group at that interface;

Only when want to receive mulitcast packets from this group, this group is joined, eg. 
open socket and setoptions of multicast for this receiving socket (kernel send IGMP REPORT) 
when setoptions;


**msend**
::

 msend -1 239.1.1.100 3300 20 192.168.167.1
  group : 239.1.1.100 
  port  : 3300
  ttl   : 20
  interface : 192.168.167.1 is the ip address of one interface

  
**mdump**
::
 mdump -Q 0 239.1.1.100 3300 192.168.167.1


tcpdump capture multicast packet
**********************************
::

  tcpdump -n dst/src host 239.1.1.4

But can't capture any packet;
::

  mdump 239.1.1.4 48000 192.168.168.102

IGMP join this group in this port, then tcpdump cature all packets;



Debugging multicast traffic with LwIP onboard
==============================================

Debugging IGMP protocol and multicast in MAC controller:

#. Connect board to switch supports IGMP snooping;
#. Send IGMP join MULTI_ADDRESS onboard;
#. Check this MULTI_ADDRESS in IGMP snooping status of switch;
#. Send multicast packets from PC with ``msend -1 MULTI_ADDRESS port interface_IP``;
#. Enable packet debug onboard, then multicast packets are received; 

**Notes**

#. When MAC controller's configuration is wrong, multicast packets are not received;
#. When switch is not received IGMP report, it also fails;
#. When router (TP-Link) is used, not switch, board need not join the group: the multicast packets are flooded to every port;
#. When debugging board, the board should be connectted to switch, not router, to minimize odd traffic from router;



Debugging IGMP protocol
***************************


