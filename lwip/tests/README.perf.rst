=======================
LwIP Testing and Coding
=======================


-----------------------------------
1. Tunning performance based on RX+TX
-----------------------------------

Both RX and TX performances are good after debugged the problems in TX path and RX path, but TFTP/HTTP updating works badly.

^^^^^^^^^^^^^^^^^^^^^^^^^^^
Tune the priority of tasks
^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Priorities of main tasks:**
  - task `MAC`: priority :4;
  - task `Timer`: priority : 4;
  - task `tcpip`: priority : 3;
  
  so, when packets arrive, no matter what packets (multicast/boardcast packet mainly), `tcpip` task is blocked, so reply will be delayed;
  
**Modification of priorities**

  - task `MAC`: priority : 3;
  - task `tcpip`: priority : 4;
   
  If no packet arrives, `tcpip` will block, so task `MAC` is ready for new packet; if packet arrives, `tcpip` can handle it as quick as possible, eg packet we are interested in will be replied quickly.


^^^^^^^^^^^^^^^^^^^^^^
Packet Differetiating
^^^^^^^^^^^^^^^^^^^^^^

There are a lot of boardcast and multicast packets, such as 255.255.255.255(ARP), 224.0.0.225(MDNS), 239.255.255.250 (SSDP, Simple Service Discovery Protocol), these packets are always handled by MAC controller,
so DMA buffers will jammed these packets, and send to IP layer, TFTP/HTTP packets can be services quickly.

So disable multicast and boardcast packets when updating begins;

**Note:**
 Multicast and boardcast must be enabled for ARP and IGMP/MDNS.
 

---------------
2. UDP TX perf
---------------


^^^^^^^^
TX Perf
^^^^^^^^

Start UDP iperf on Linux PC:
::
 iperf -u -s -i 5 

Start iperf UDP TX as quick as possible
::
 udp 192.168.168.102

TX bandwidth displayed on PC/Linux is about 17.6Mbps without optmizing code, that means it is good.

Normally UDP TX will not find OOM (Out Of Memory), but sometimes, `MAC` recv task will can find OOM.


------------------------------
UDP RX perf and RX Performance
------------------------------

^^^^^^^^^^^^^^^^^^^^
UDP RX Perf Testing
^^^^^^^^^^^^^^^^^^^^

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

^^^^^^^^^^^^^^^^^^^^
MAC Controller Debug
^^^^^^^^^^^^^^^^^^^^

**When overrun happens**

- Return from overrun, ignore the packet;
- Add more buffer blocks for DMA transmit, from 3 to 7, and debug the problem when buffer increasing ( eg. can't not work);
  - renew buffers, and then set the owner of MAC controller explicitly;
- Decrease the number of MAC interrupts
  - disable all TX interrupt;
  - clear the bits in Receiving Status Register;



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
