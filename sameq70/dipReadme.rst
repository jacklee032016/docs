Readme for DIP Switch
##############################
Oct.3, 2018


Primitives
=============
Make new multicast IP effective
---------------------------------
#. Remove existed group(RX only);
#. Group address from backup;
#. Join new group(RX only);
#. Configure FPGA;


Events
===========

ON --> OFF
-------------
#. New group address from backup;

#. Make new multicast IP effective;

#. Save to non-volatile;

OFF --> ON
-------------
#. New group address from DIP switch;

#. Make new multicast IP effective;

#. Save to non-volatile;


Set Multicast IP
---------------------
#. Save new multicast IP into backup;

#. If DIP switch is OFF, make new multicast IP effective;



Handles for Different States
===================================

ON State
----------
Start Up
^^^^^^^^^^^^
* Read group from DIP switch;

Poll Job
^^^^^^^^^^
* Read group address from DIP switch;

* If group address is changed, make new multicast IP effective;

OFF State
----------
Start Up
^^^^^^^^^^^
* Read group from backup;

Poll Job
^^^^^^^^^^^
* Do nothing;


Management
===================================

Save and list backup multicast address
----------------------------------------

* Command ``params`` list current multicast ip address and the backup multicast IP address;

* Command ``igmp`` list curren registed group;



Test Cases
===============

#. DIP ON boot/reboot

#. DIP ON --> OFF, and reboot;

#. DIP OFF --> ON, and reboot;

#. Change DIP switch when DIP is ON;

#. Change DIP switch when DIP is OFF;

#. Change Multicast IP when DIP is ON;

#. Change Multicast IP when DIP is OFF;
