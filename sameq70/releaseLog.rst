
Release version and log
##################################
Sep.7th, 2018	Jack Lee

10.03, 2018
====================

When receives set_params, reply set_param to port of 3840
----------------------------------------------------------

Parameters List:
#. **cName**: customized name, maximum size of 32;
::

    '{"cName":"JackLeeRX01"}'

#. **isDipOn**: Set DIP switch is enabled or disabled;
::

  '{"isDipOn":0|1}'

#. **MCASTip**: set multicast IP address which is used when DIP switch is disabled;


#. **isDhcp** and **ip** : DHCP enabled/disabled and IP address;
::

  '{"ip":"192.168.168.121","isDhcp":1}'
  
#. **IsConnect**: connect or disconnect media; for TX, stop media; for RX, leave the group;
::

	'{"IsConnect":0|1}'
	
#. **RS232Baudrate**: set and save; then **``send_data_rs232``** can send data with this baudrate;
::

	'{"RS232Baudrate":115200}'
	
#. **RS232Parity**: set as "odd/even/none";
::

  '{"RS232Parity":"odd"}'

#. **RS232Databits**: set as "odd/even/none";
::

  '{"RS232Databits":7}'

#. **RS232Stopbits**: set as "odd/even/none";
::

  '{"RS232Stopbits":0}'



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
