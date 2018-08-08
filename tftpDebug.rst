
----------------
TFTP Enhancement
----------------

- Check the replicated packets and their block number;
- When replicated packets are received, reply ACK packet with replicated block number again;
 - Ignore this packet to write to flash;


------------------------
Client break connection
------------------------

When client break the session, don't update firmUpdateInfo into flash


^^^^^^^^^^^^
For TCP_PCB
^^^^^^^^^^^^
In recv callback, the pbuf is null when client break this session;

^^^^^^^^^^^^
For UDP_PCB
^^^^^^^^^^^^

- A timer must be added for this connection;
- Every time, when recv callback is called, refresh this timer;
- If timer is timeout, then connection can be thought as broken.



-----------
Debug
-----------

^^^^^^^^^^
Client
^^^^^^^^^

- python tftp library enable debug in TftpPacketTypes and TftpClient
- Test with command line client;

^^^^^^^^
Onboard
^^^^^^^^

- debug every DATA packet received and its block number;
- debug every ACK packet sent and its block number;

^^^^^^^^
Capture
^^^^^^^^

Capture packet and debug info of BLOCK number
::
 tcpdump -n src host 192.168.168.120 -v


