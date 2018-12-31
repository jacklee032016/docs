
Release version and log
##################################
Sep.7th, 2018	Jack Lee

12.31, 2018
===========================
* Add IP commands "reboot", "blink_led";
* Debug problem of json format in IP command;
* Add debug interface for IP Command;


Debuggings, 12.26, 2018
===========================

Bug of "tcpip: [eosMain.c-29.vApplicationMallocFailedHook()]: "
------------------------------------------------------------------
* When DHCP renew IP address, protocol status_callback() is called again, and then all program about protocol stack are going to restart again,
* such as tasks, timers, so the heap of RTOS is not enough for all of these.

* For example, telnet can't take port 21 again; tasks, timers, semaphores use about 40KB memory (50*1024-11576);

* Add variable to track the count of IP address active, then tasks/timers can only be started when it is first time;

* Startup flow:
   * start protocol:
   * start tcp task and initial lwip, such as sys_init;
   * start MAC and PHY in init_done() in the context of task tcp;
      * start MAC task;
      * start PHY;
   * start DHCP or set static IP;
   
   * callback of status
      * start telnet;
      * start httpd, ipcmd, hcd etc,
          


Bug of system crash when poll to maximum, and then close connection
---------------------------------------------------------------------
* Sometimes crash happens, sometimes everything is OK when excution like that.

* Set TCP_PCB and its data when connection is closed in FSM (eg. in task of HTTP server), tcp task will operate on them, and httpd task also operate on them;

* If one task is prompted when it is operating one of the data member of TCP_PCB, such as recv, arg, sent, etc, then crash will happen;

* So only tcp task can close connection, FSM/httpd must wait the operation of tcp task:
   * poll event is directly handled by TCP task;
   * close event is directly handled by TCP task;


"TypeError: NetworkError when attemping to fetch resource" in browser after firmware is updated
-----------------------------------------------------------------------------------------------------
* protocol handle of webpage/updatefirmware are handled in FSM;
   * data receiving for firmware updating is handled in INIT state;
   * Result replies are handled in entering handle of connection (web page and firmware updating);
* Result reply of Firmware update is commented by debug option in httpSend function;


SYS_LIGHTWEIGHT_PROT = 1
-----------------------------

assert of 'pbuf->ref > 0' failed
---------------------------------------


Update firmware through python script failed
-----------------------------------------------


12.18, 2018
----------------
* tcpip: add 8KB memory to heap of RTOS to make it more ;
* Strong mac driver:
  * Disable Interrupts when task is processing packet;
  * Disable RX interrupt when buffers are re-populated after some errors happen, such as buffer is used up;
  * When something is wrong, reinit rx hardware and the DMA buffers;
  * Control interrupt speed of ethernet frame in mac controller to keep OS stable and resonse quickly;
  * Diable multicast when system startup; enable multicast after system startup, eg. get IP address;
* Improve performance of all kind services: REST API/IP command, web pages;
  * Restore operation after buffers have been used up;
* MCU Firmware update by web browser when media is playing;
* FPGA Firmware update by web browser when media is playing;
* MAC address can be random again once MAC address is configured, even after factory reset;
* Add 1KB buffer for command line interface;
* SDP http client:
  * Process HTTP response which is distributed in multiple IP packets;
  * Process SDP response from AJA's TX;
  * Save parameters from SDP response after configuration finished;
  * Process non-exists/wrong URIs;
* Debug the problem: configuration data is crashed after MCU firmware has been updated;
  * Update bootloader and OS firmware;
* Add debug interface for HTTP server; 
* Add debug interface for HTTP client; 




12.17, 2018
----------------
Resrite driver of MAC controller, fixing problem of packet lossing;
SDP HTTP client;

Oct.22, 2018
====================
Debug bootup problem of bootloader caused by RS232 driver when chip is null or chip is ``chiperase``d;


10.17, 2018
====================
Add function of GPNVM updating in bootloader;

10.03, 2018
====================

When receives set_params, reply set_param to port of 3840
----------------------------------------------------------

Parameters List:
=======================
Following are parameters which can be set/modified by IP command ``set_param``.

System Parameters
---------------------
* **cName**: customized name, maximum size of 32;

::

    '{"cName":"JackLeeRX01"}'

* **isDipOn**: Set DIP switch is enabled or disabled;

::

  '{"isDipOn":0|1}'

* **MCASTip**: set multicast IP address which is used when DIP switch is disabled;


* **isDhcp** and **ip** : set DHCP enabled/disabled and IP address; active after reboot;

::

  '{"ip":"192.168.168.121","isDhcp":0}'


* **mask**: set netmask of network interface; active after reboot;

::

  'mask': '255.255.0.0'

* **gateway**: set gateway address of network interface; active after reboot;

::

	'{"gateway":"192.168.168.2"}'


* **mac** : set MAC address and disable random MAC address;

::

	'{"mac":"12:22:33:44:55:66"}'

  
* **IsConnect**: connect or disconnect media; for TX, stop media; for RX, leave the group;

::

	'{"IsConnect":0|1}'


Protocol Parameters
-----------------------

* **vidPort**: set port of video stream;

::

	{"vidPort":11220}'


* **audPort**: set port of audio stream;

::

	{"audioPort":11222}'


* **datPort**: set port of anccilary data;

::

	'{"datPort":11240}'


* **strPort**: set port of anccilary strea;

::

	'{"strPort":11260}'


Media Parameters
-----------------------
Media parameters are read from FPGA and sent to 811 by TX; and 811 resend them to RX; then RX configure FPGA;

MCU never save media parameters;

* **vidW**: set video width;

::

	'{"vidW":1260}'


* **vidH**: set video height;

::

	'{"vidH":720}'


* **vidClrSpace**: set video color space; "YCbCr-422|YCbCr-444|RGB|YCbCr-420|XYZ|KEY|CLYCbCr-422|CLYCbCr-444|CLYCbCr-420"

::

	'{"vidClrSpace":"RGB"}' 


* **vidFps**: set video frame rate; 23|24|25|29|30|50|59|60;

::

	'{"vidFps":23}' 


* **vidIsSgmt**: set video Interlaced and Segmented, interlac bit 0 , segmented bit 1; so set as 0|1|2|3;

::

	{"vidIsSgmt":3}'


* **vidDepth**: set color depth, 8|10|12|16;

::

	'{"vidDepth":12}'



RS232 Parameters 
------------------------

* **RS232Baudrate**: set 9600|19200|38400|57600|115200 and save; then **``send_data_rs232``** can send data with this baudrate;

::

	'{"RS232Baudrate":115200}'
	
* **RS232Parity**: set as "odd/even/none";

::

  '{"RS232Parity":"odd"}'

* **RS232Databits**: set as 5|6|7|8;

::

  '{"RS232Databits":7}'

* **RS232Stopbits**: set as 1|2|3; here, 3 means 1.5 bits;

::

  '{"RS232Stopbits":1}'



09.20, 2018
====================
* Reconfigure FPGA both before and after network is configured;
   * For TX: 
      * configure the default IP address before network; 
      * network is actived(DHCP/static); 
      * configure with active IP address;
      * start media transmission;
   * For RX: 
      * Configure default IP address and don't join multicast group (network interface is not available now); (add reset and release reset as specs from FPGA)
      * network is active(DHCP/static); 
      * configure with active IP address and join group;
      * No start register is usable in RX;
* Add RS232 task to monitor RS232 and read back;
* Add delay when bootloader loading OS to test;
* Debugging the problem when 2 RXes are used in same LAN;
* Debugging the problem when command 'net 1' is used;
* Debugging the problem of receiving too much packets in MCU when bootup, make it more stronger;


09.13, 2018
====================
* Debuggin the problem of memory leakage in case of re-send IP 'set_media' command in TX when no-reply from 811;
* Prioritise the response of IP commands:

  * Implement IP command in independent task;
  * Move the priority level of IP command Task to maximum;
* Debugging the start/stop of TX and RX:

  * Send 'set_param' with parameter of `{"IsConnect": 1}`;
  * For RX, leaving the IGMP group in switch/router;
  * For TX: 
  
     * configure register to disable media streams;
     * check register of SDI statuss;
     * Update new FPGA firmware to support enable/disable media transmission;
* Bootloader delay more 200 ms to load OS when firmware is updated;
     

09.07, 2018
===================
* DHCP+Random MAC:
   * Random MAC address use local and unicast address;
   * DHCP try 3 times with timeout of 8, 16, 32 seconds (total 56 seconds) to suit the requirement of random MAC;
   * Use static IP address after DHCP fails 3 times;
* Button blinking:
   * After pressing button for 6 seconds, Power LED will blink; releasing button, then factory configuration is active;
   * Support hardware timer in ISR;
* Boot flow of network protocol and FPGA
   * FPGA firmware is loaded first;
   * Start network interface;
   * Start DHCP client to get address or use static IP address;
   * After IP and NIC is up, start network protocol;
   * After network protocol is up, configure FPGA and IGMP group address(RX);
* TX send new media parameters to 811 directory:
   * Default configuration of 811 is: 192.168.168.50:50;
   * 811 notes TX its address and port in boardcast 'get_param' command;
   * TX send new parameters with unicast 'set_param' command when SDI connect or disconnect;
   * 811 should reply this 'set_param' command just like what TX does when it receive command from 811;
   * If no reply from 811, TX will keep to send it until 811 reply or new parameters are found;
* Default network setup is DHCP in factory configuration;
* Add reset logic for FPGA in RX when new IP/MAC/ports are configured;
* Optimize some message output from UART console;
* Modify bootloader to be more compatible with futural update of OS;
