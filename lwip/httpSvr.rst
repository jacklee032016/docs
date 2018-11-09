HTTP Server and Optimization
###############################
Oct.25,2018	Jack Lee

URI list
==================

HTML
--------
#. /: GET, HTML
#. /info: GET, HTML
#. /media: POST, HTML --> /setting: POST, JSON
#. /reboot: GET, HTML
#. /mcuUpdate: POST, from /upgradeMcu.html
#. /fpgaUpdate: POST, from /upgradeFpga.html

JSPN API
-----------
#.
#. /service: GET, JSON

SDP
-------
#. /video.sdp: GET, SDP
#. /audio.sdp: GET, SDP



curl -d "urlSdpVideo=http://192.168.166.2/vedio.sdp&urlSdpAudio=http://192.168.166.2/audio.sdp&ipVideo=240.0.0.1&ipAudio=240.0.0.2&portVideo=23456&portAudio=23457\
&portData=23458&portStrem=23460&videoWidth=1240&videoHeight=768&colorSpace=CLYCbCr-422&colorDepth=16&videoFps=59&audioChannels=8" -X POST http://192.168.166.2/setting 


curl http://192.168.166.2/setting 
curl http://192.168.166.2/service 



Test Cases
===============
#. poll: telnet connect and not input anything;
#. small packets to assemble HTTP request: Windows telnet: parse header or length of input string;
#. invalidate URL; reply 404; for REST API, reply JSON 404;
#. Service and reply big file, such as 12KK logo.jpg;
#. Performance: number of PBUF_POOL, and TCP_PCB;

Enhancements
===================
* HTTP_CONN use memory pool, which pre-allocates fixed number of CONNS;

* Receive request from multiple pbuf;

* Data buffer is removed from HTTP_CONN, make it independent;


Debuggings 
===================
* When send all data, close connection directly:
   * only close connection when enter into CLOSE state;

* PBUF_POOL are used up, and can't connect again; 
  * And no PBUF_POOL message is keeping prompting: tcp task always work; 
  * After about 64 CONNs : every HTTP CONN use one pbuf, and not free;
* add ``ref`` count by one when enqueue pbuf of request;



HTTP Request Headers Parsing
=================================
* ``content-length`` is for the data posted by request, not in header;
   * ```content-length:application/json`` means POSTed data is in json format;
* ``accept:*/*,application/json`` means the HTTP response can be in json format;


Parsing Headers
=======================
* REST API: 
* SDP???
* Web Socket;
* POST or Firmware update;


FriFox always send 3 concurrent connection for '/' when it starts;

Sometimes, when tcp thread fails for new connection, but http thread may free, and when it try again, connection is OK again;


Design
=====================

States
---------------

REQ(uest)
^^^^^^^^^^^^^
* After connection is created, and is in this state;

DATA(Input)
^^^^^^^^^^^^^
* If request is POST, receive data in first request and others packets;

RESP(onse)
^^^^^^^^^^^^^
* After request is parsed and data is received if request is POST;
* if data is not finished, stayed in this state and response ``sent`` event;
* if EOF, enter into state of CLOSE;

CLOSE
^^^^^^^^^^^
* Wait other events from TCP task, and free connection;

ERROR
^^^^^^^^^^^
* If ``error`` event received, enter into this state;
* Wait other events, and free connection; 


Events
-----------------

error
^^^^^^^^^
Some error from client, such as broken; and TCP_PCB is deallocated now;

recv
^^^^^^^^
* when PCB, pbuf and err all are good, it means data is available;
* when pbuf is null, last packet is null, means client has broken this connection, but TCP_PCB is still available;

poll
^^^^^^^
* timer has timeout, can be used to check and drive the status of connection;

sent
^^^^^^^
* acknowledge the last send with actual length;
* last acknowledged length is smaller than TCP_MSS;
* acknowledge all data transfer with tcp_write();
* can be used to send more data;

accept
^^^^^^^^^^^
* Create new connection;


Tasks
-----------

TCP Task
^^^^^^^^^^^


HTTPd Task
^^^^^^^^^^^^^


Resource Management
----------------------

TCP_PCB
^^^^^^^^^^^^
* All TCP_PCB operations are handled in TCP task;
* After CONN is freed by HTTPd task, when next event (not error event) is emmitted, 
   * call ``tcp_close()`` and set all the callbacks of TCP is null;
   * but poll event with TCP state of ``CLOSE_WAIT()``?????


HTTP CONNECTION
^^^^^^^^^^^^^^^^^^^
* Allocated by TCP task;
* Freed by HTTPd task;
   * Before free CONN, call ``tcp_arg(pcb, NULL)``, so unload CONN from TCP_PCB;
   * TCP task still send event for this PCB after this CONN is unloaded from PCB;
   * So these events will not send to HTTPd task from now on;
* Mutex lock for these 2 task;


HTTP EVENT
^^^^^^^^^^^^^^^^^^
* Allocated by TCP task;
* Freed by HTTPd task;
* Mutex lock for these 2 tasks;


Concepts
=============

* TCP, SND_BUF_SIZE and TCP_MSS
   * At least 2 times of TCP_MSS to improve TCP reply performance;
   * For X86, SND_BUF is 4 times: 4*1460 = 5840;
   * The maximum size of static file is about 13KB, so twice for maximum file;
   
HTTP Service Types
------------------------

#. Web Socket;
#. REST API;
#. Static files;
#. CGI, dynamic content from program;
#. Update firmware;
#. SDP;

METHOD types
------------------
#. GET
#. POST
#. PUT
#. DELETE
#. PATCH


HTTP connections pool and heap memory
-------------------------------------------
* pre-allocated memory HTTP connections pool in memp;
* otherwise, HTTP connection is allocated from LwIP heap by ``mem_allocte()`` from LwIP;
* Heap size : 16KB
