
FPGA Tests
#####################################
Sep., 2018	Jack Lee


FPGA Configuration Logs
==============================

09.14
*********
* **Two phases of configuring of FPGA**:
  * Configuring FPGA before network interface is enable, so FPGA filter out all traffic;
  
  * Enable network interface(MAC controller, DHCP/static);
  
  * Reconfigure FPGA with new MAC, IP and port:
  
    * For TX it is like that;
    
    * For RX, MAC/IP is not related with MAC/IP of local network interface, it is peer's; so not related with network;


* **Two Phase of Configuration of RX FPGA**:

  * Configure FPGA before network interface is enable and without IGMP join;

  * Enable network interface;

  * Join IGMP group without any other configuring FPGA;


* **One phase of configuration of FPGA**:

  * Configure FPGA to filter out all traffic;

  * Enable network interface;



FPGA Updating Logs
=====================

TX
************
* **09.14, 2018**: 2 phases of configure FPGA;

* **09.13, 2018**: register 03 to disable all media(0x00) and enable them (0x00);

RX
************
* **09.04, 2018**: Add reset and its clean after re-configuring MAC/IP/Ports to make video is field synchronized




Testings
================

FPGA Flooding to MCU when boot up
***********************************
With Fiber interface, after first flooding, MCU network interface can't be accessed;

But if using RJ45 interface, MCU network interface works normally;
 