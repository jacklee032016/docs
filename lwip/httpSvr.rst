HTTP Server and Optimization
###############################
Oct.25,2018	Jack Lee

URI list
==================

HTML
--------
#. /: GET, HTML
#. /info: GET, HTML
#. /media: POST, HTML 
  # --> /setting: POST, JSON
  # --> /sdpClient: POST, begin HTTP Client
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

Test Cases
===============

REST API tests
-------------------

curl -H "Content-Type: application/json" POST   -d '{"name":"Jack-RX", "isDHCP":0, "fpgaAuto":0}' http://192.168.168.102/service -v -i

curl -H "Content-Type: application/json" POST   -d '{"name":"Jack-RX2", "isDHCP":0, \
"sdpVideoIp":"192.168.168.101", "sdpVideoPort":80, "sdpVideoUri":"video.sdp", "sdpAudioIp":"192.168.168.101", "sdpAudioPort":80, "sdpAudioUri":"audio.sdp", \
"ipVideo":"239.100.1.1", "ipAudio":"239.100.1.2", "ipAnc":"239.100.1.3", "portVideo":48000, "portAudio":48010, "portData":48020, \
"fpgaAuto":1, "videoHeight":1080, "videoWidth":1920, "videoFps":59, "colorSpace":"YCbCr-444", "colorDepth":16, "videoIsIntlce":1, \
"audioSampRate":"44.1KHz", "audioChannels":2, "audioDepth":16, "audioPktSize":"1ms", "isConnect":0 }' http://192.168.168.102/service -v -i


Simple REST API:

curl -H "Content-Type: application/json" POST   -d '{"name":"SdiOverIP-767", "sdpVideoIp":"192.168.167.3", "sdpVideoPort":80, "sdpVideoUri":"video.sdp" }' http://192.168.167.3/service -v -i


REST API for one set of parameters:

curl -H "Content-Type: application/json" POST   -d '{"name":"Jack-RX", "version":"00.01-01", "address":"192.168.167.4", "gateway":"192.168.167.1", "isDHCP":4, "MAC":"00:04:25:1c:a0:02", \
"sdpVideoIp":"192.168.167.4", "sdpVideoPort":8080, "sdpVideoUri":"video2.sdp", "sdpAudioIp":"192.168.167.4", "sdpAudioPort":8080, "sdpAudioUri":"audio2.sdp", \
"ipVideo":"239.100.11.1", "ipAudio":"239.100.12.2", "ipAnc":"239.100.13.3", "portVideo":48100, "portAudio":48210, "portData":48320, \
"fpgaAuto":1, "videoHeight":720, "videoWidth":1080, "videoFps":23, "colorSpace":"YCbCr-444", "colorDepth":16, "videoIsIntlce":3, \
"audioSampRate":"96KHz", "audioChannels":4, "audioDepth":24, "audioPktSize":"125mks", "isConnect":0 }' http://192.168.167.3/service -v -i


REST API for another set of parameters:

curl -H "Content-Type: application/json" POST   -d '{"name":"SdiOverIP-767", "version":"00.01-01", "address":"192.168.167.3", "gateway":"192.168.167.1", "isDHCP":0, "MAC":"00:04:25:1c:a0:02", \
"sdpVideoIp":"192.168.167.3", "sdpVideoPort":80, "sdpVideoUri":"video.sdp", "sdpAudioIp":"192.168.167.3", "sdpAudioPort":80, "sdpAudioUri":"audio.sdp", \
"ipVideo":"239.100.1.1", "ipAudio":"239.100.1.2", "ipAnc":"239.100.1.3", "portVideo":48000, "portAudio":48010, "portData":48020, \
"fpgaAuto":1, "videoHeight":1080, "videoWidth":1920, "videoFps":59, "colorSpace":"YCbCr-444", "colorDepth":16, "videoIsIntlce":1, \
"audioSampRate":"44.1KHz", "audioChannels":2, "audioDepth":16, "audioPktSize":"1ms", "isConnect":0 }' http://192.168.167.3/service -v -i


Web Page tests
-----------------------

curl -d "fpgaAuto=Manual&name=Jack-767&ipVideo=239.120.1.1&ipAudio=239.120.1.2&ipAnc=239.120.1.3\
&portVideo=48500&portAudio=48610&portData=48720&videoWidth=960&videoHeight=480&videoIsIntlce=3&videoFps=23&colorDepth=12\
&colorSpace=1&audioChannels=4&audioSampRate=2&audioPktSize=1" -X POST http://192.168.167.3/setting 



curl -d "urlSdpVideo=http://192.168.166.2/vedio.sdp&urlSdpAudio=http://192.168.166.2/audio.sdp&ipVideo=240.0.0.1&ipAudio=240.0.0.2&portVideo=23456&portAudio=23457\
&portData=23458&portStrem=23460&videoWidth=1240&videoHeight=768&colorSpace=CLYCbCr-422&colorDepth=16&videoFps=59&audioChannels=8" -X POST http://192.168.166.2/setting 


curl http://192.168.166.2/setting 
curl http://192.168.166.2/service 



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
