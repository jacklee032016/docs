=======================
LwIP Testing and Coding
=======================



----------
UDP perf
----------


^^^^^^^
TX Perf
^^^^^^^


iperf -u -s -i 5 
(on host with IP 192.168.1.9)



^^^^^^^
RX Perf
^^^^^^^

board plays as UDP server, only receives packets, never reply 
::
 iperf --client 192.168.166.2 --interval 5 -t 100 -u -b 1G 

  - -c in client mode, connect to <host>
  - -i Interval for bandwidth report
  - -t time in second to transmit
  - -u UDP
  - -b bandwidth to send in bits/second

Tests on board:
::
 iperf --client 192.168.168.120 --interval 5 -t 10 -u -b 100M 
 
After that, 


^^^^^^^^^^
Questions
^^^^^^^^^^

iperf send out about 17064 datagrams, and report bandwidth 100Mbps, and WARNING: did not receive ack of last datagram after 10 tries.

So, it is not correct because of connection-less of UDP;

Board receive and print some info, after that, it can't receive any packets; at same it send out multicast packets and be captured in PC/Linux;


--------------------------
Test UDP Perf in PC/Linux
--------------------------


UDP Perf server:
::
 iperf -s -i 10 5 -t 100 -u

UDP Perf client:
::
 iperf --client localhost ---interval 5 -t 10 -u -b 1G


tcpdump capture the process of protocol
:: 
 tcpdump -D : list add devices which can be captured;
 tcpdump -n host localhost -v udp -i lo


IP Packet length: 1498; UDP length: 1470;

Only traffic from client to server, no any reply from server;

Bandwidth reports from client and server are same;

----------
Questions 
----------

How client know the bandwidth or whether packets are received by server without reply from server? UDP is connection-less protocol.
