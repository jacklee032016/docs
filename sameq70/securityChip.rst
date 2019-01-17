About Security Chip
##############################
Jan.7, 2019	Jack Lee


IP Commands and Implementation
==================================

Implementing Algorithm
--------------------------


IP Commands:
----------------
#. get_id: 8 bytes;
#. set_key: 32 bytes;
#. get_status: check OK or not in login_ack field;

Implementation
--------------------
* Save ``secrect`` even after factory reset;
* No 'secrect' or 'check failed', start protocol stack and disable FPGA;
* How to initialize ``secrect`` in factory: interface: IP commands can be used by everyone?


Flows
------------------

* **set_key: Write secrect KEY**
   * write secrect;
   * write page#0;
   * write page#1;

* **get_id**:


# **get_status**:
   * get challenge from random;
   * calculate MAC;
   * compare MAC;

* **startup**:
   * call ``get_status`` directly;
   * stop FPGA configuration if failed;


Testing
-----------------




*************


Algorithm of authentication
================================
Algorithm runing by chip and software;

SHA256 based on following 128 bytes data:
* 32-byte: pageData;
* 32-byte: challenge;
* 32-byte: secrect;
* 8 byte: ROM ID or 0xFF
* #104: register 1;
* #105: register 0;
* #106: page#;

				
Example Flow
==============================

Initialization:
------------------
* read and check ROM ID for 100 times;
* read page#0;
* read page#1;

Read ROM ID
---------------


Init KEY
----------------
* write page#0 with seed;
* load and lock secrect: ROM ID:
   * secrect is written with scratch_pad;


Init MAC/Page#1
----------------
Format of page#1:
* 6-byte MAC address;
* 10-byte Model ID;
* 20 product name
* #31, CRC 8-bit

Read Memory
----------------
* read page#0;
* read page#1;
* read status registers;

Read and authentication
--------------------------
* add challenge: 32 bytes;
* read result from hardware: 
   * Input: page#, challenge, isAnon;
      * challenge is written with scratch_pad;
   * Output: 32 bytes MAC hash;
* compute and authenticate
