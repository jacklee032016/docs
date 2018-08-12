================
About Multicast	
================
Feb, 2018 Jack Lee


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Multicast forwarder in Linux
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**routes and devices**
::
 ip route add 239.100.1.0/8 dev tap0
 ip route add 239.100.1.0/8 dev tap1

 iptables -A INPUT -d 239.100.1.1 -j ACCEPT
 iptables -A FORWARD -d 239.100.1.1 -j ACCEPT
 iptables -A OUTPUT -d 239.100.1.1 -j ACCEPT

multicast forward
::
 iptables -A INPUT   -m pkttype --pkt-type multicast -j ACCEPT
 iptables -A FORWARD -m pkttype --pkt-type multicast -j ACCEPT
 iptables -A OUTPUT  -m pkttype --pkt-type multicast -j ACCEPT

Or:
::
 iptables -A INPUT   -s 224.0.0.0/4 -j ACCEPT
 iptables -A FORWARD -s 224.0.0.0/4 -d 224.0.0.0/4 -j ACCEPT
 iptables -A OUTPUT -d 224.0.0.0/4 -j ACCEPT


^^^^^^^^^^^^^^^^^^^^^^^^^^
Multicast packet with `nc`
^^^^^^^^^^^^^^^^^^^^^^^^^^

June,15, 2018

**1. multicast server and group registering:**

`mdump` was used in another session to confirm that the data was being sent:
::
 mdump 239.100.1.1 3600 192.168.166.1

Equiv cmd line: 
::
 mdump -p0 -Q0 -r4194304 239.100.1.1 3600 192.168.166.1
WARNING: tried to set SO_RCVBUF to 4194304, only got 327680

**2. send multicast packet to group**
::
 nc -w 1 -v -u -s 192.168.166.1 239.100.1.1 3600 < init.sh
	-w : wait timeout
	-v : verbose
	-u : UDP
	-4 : IPv4
	-s : source address

 nc -w 1 -v -u -s 192.168.167.1 239.100.1.1 3600 < init.sh

 route add -net 224.0.0.0 netmask 240.0.0.0 tap0

 mdump 239.255.0.1 30001 127.0.0.1


^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
UDP boardcast packet in Linux
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
send boardcast packet:
::
 echo "test" | nc -4u 192.168.166.255 3600

 nc -4u 192.168.166.2 3600 < test.txt

Test UDP, echo and 'ncat':
::
 echo "test" | nc -4u 192.168.166.2 7

result: 

- return 'test' displayed in console of Linux
- debug info in LwIP;

When `'ncat: Connection refused.'`, means no service point can be accessable;

When `ncat` is used to access unicast port, everything is OK;

When `ncat` is used to access boardcast port, UDP server can receive and reply message, but ncat can't receive reply;

Normal UDP client can receive reply of boardcast message in unicast port;
	

^^^^^^^^^^^^^^^^^
About open-mtools
^^^^^^^^^^^^^^^^^

- mdump: receive
- msend: send 
- mpong: ping multicast

