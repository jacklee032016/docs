
Release version and log
##################################
Sep.7th, 2018	Jack Lee

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
