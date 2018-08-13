=========================================
MQTT, Message Queuing Telemetry Transport  
=========================================

Aug.12,2018	Jack Lee


----------
Callbacks
----------

^^^^^^^^^^^^^^^^^
Connect Callback
^^^^^^^^^^^^^^^^^

^^^^^^^^^^^^^^^^^^^
Requests Callbacks
^^^^^^^^^^^^^^^^^^^

Subcribe, unsubcribe, and publishing requests

^^^^^^^^^^^^^^^^^^^
Incoming Callbacks
^^^^^^^^^^^^^^^^^^^

Incoming data and incoming publish on one subscribed topic


---------
Concepts
---------

M2M (Machine to Machine)

Server/Client
Publisher --> topic
Subscribers  <-- topic


^^^^^^^^^^^^^^^^^^^
MQTT message types
^^^^^^^^^^^^^^^^^^^

Connect:
- CONNECT = 1,
-	CONNACK = 2,

Publish:
- PUBLISH = 3,
- PUBACK = 4,

- PUBREC = 5,
- PUBREL = 6,
- PUBCOMP = 7,

Subscrube:
- SUBSCRIBE = 8,
- SUBACK = 9,

- UNSUBSCRIBE = 10,
- UNSUBACK = 11,

Ping:
- PINGREQ = 12,
- PINGRESP = 13,

Disconnect:
- DISCONNECT = 14
