==================
Readme for Telnet 
==================
July 18,2018	Jack Lee

Enhancements
----------------
* HTTP_CONN use memory pool, which pre-allocates fixed number of CONNS;

* Receive request from multiple pbuf;

* Data buffer is removed from HTTP_CONN, make it independent;


Debuggings 
-----------------
* When send all data, close connection directly:
   * only close connection when enter into CLOSE state;

* PBUF_POOL are used up, and can't connect again; 
  *And no PBUF_POOL message is keeping prompting: tcp task always work; 
  After about 64 CONNs : every HTTP CONN use one pbuf, and not free;
			* add ref count by one when enqueue pbuf of request;

			