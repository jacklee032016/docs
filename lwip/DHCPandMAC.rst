
DHCP service and MAC Address
###################################
Sep.5th, 2018	Jack Lee

Types of MAC Address
======================
The first bits of the first byte of MAC address is used to identify the type of MAC address, so dest can differs them as earlier as possible.

In PC or little-endian system, 11:22:33:44:55:66, the first byte is 0x11; and the first bit is bit-0 in this byte;

Unicast MAC address and Multicast MAC address
------------------------------------------------
Dependent on the first bit of the first byte of address:

* 1: this is multicast address;
* 0: this is unicast address;

Local MAC address and Universal MAC address
-----------------------------------------------
Dependent on the second bit of the first byte of address:

* 1: local adddress, eg. address is not allocated, like 192.168.xx,yy of IP address;
* 0: universal address, eg. address is UNI and allocated to one of venders;	


Random MAC address
-------------------------
It must be an unicast and local MAC address, eg, in following 4 formats:

#. **x2-xx-xx-xx-xx-xx**
#. **x6-xx-xx-xx-xx-xx**
#. **xA-xx-xx-xx-xx-xx**
#. **xE-xx-xx-xx-xx-xx**

eg, first byte must be in xxxx,xx10;


DHCP timers and random MAC address
=====================================

Types of DHCP timers
----------------------

#. **fine_timer**: every 500ms, for discovery process;
#. **coarse_timer**: every 1 minute, for renewal/rebind process;


Basic bind process
------------------------

* Client boardcast **DISCOVERY** message;
* Server boardcast **OFFER** message: contains unleased IP address, client hw address and others in DHCP message;
* Client boardcast **REQUEST** message: want to bind this unleased IP address;
* Server boardcast **ACK** message;

Afte than, communication is based on unicast address;


Binding for Random MAC and UNI MAC 
------------------------------------

Timeout of discovery process:

* In discovery process, timeout of fine_timer is using algorithm of backoff, every retry will double the timeout;
* The first timeout is set as 2000ms, it always fails for random MAC;
   * It may works for MAC addres which has been used in this LAN when timeout is 2000ms;
* Initial timeout must be 4000ms, then random MAC address works;
* The first **DISCOVERY** message always fails no matter what timeout it is???

* No explicit difference between random MAC and UNI MAC when first binds the MAC with DHCP;

