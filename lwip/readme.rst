=============================
Test of LwIP(Lightweight IP) 
=============================
January, 2018	Jack Lee

`LWIP timeout algoithm <lwipTimers.rst>`_

`MAC address and DHCP <DHCPandMAC.rst>`_

`IGMP and multicast protocol <igmpAndMulticast.rst>`_

`multicast tests <multicast.rst>`_

`LWIP performance tests <performanceTest.rst>`_

`About telnet <telnet.rst>`_

`MQTT, Message Queuing Telemetry Transport <mqtt.rst>`_

`LWIP tests <tests>`_

* **Random MAC Address**
 * random number from hardware (TRNG, True RANDOM);
 * lowest bit of first byte of MAC address must be zero, this is unicast MAC address;


May.21st, 2018
apiClient: 
 Only JSON data parsing is dependent on LwIP library, network operations are only dependent on Linux Library;


Test performance with lwiperf
::
 iperf -c host 
  -i 5	: report interval, 5 seconds;
  -t 50	: runing time, 50 seconds;
  -w 8K : receiving window size, 8K;
		

sendTftpFile.sh
tftp client script, send file of RTOS/FPGA images to AN767
 * modify IP addrss to board;
 * modify DEST for RTOS or FPGA;
 * privide file name of image;

startup.sh
 * start simhost first time after virtual Linux boots: add virtual ethernet interface of tap0 and routes corresponding to it;
 * after that (tap0 can be seen), only 'simhost' command is needed;

test767.sh


^^^^^^^^^^^^^^^^^^
tcpdump capturing
^^^^^^^^^^^^^^^^^^
**iptable to enable**
::
 iptables -A INPUT -p udp --dport 3600
 
**Capture source ip**
::
 tcpdump -n src host 192.168.168.102
**Capture any packet from/to source/dest**
::
 tcpdump -n host 192.168.168.102
 tcpdump -n host 192.168.167.1
 tcpdump -i tap1 -n udp
		

^^^^^^^^^^^^^
Directories:
^^^^^^^^^^^^^
		lwip/src:
				all source files (including lwIP and its extensions) crossing platform;
				lwip/Makefile: Makefile for Unix platform;
				lwip/src/Makefile: Makefile for ARM-Cortex-M7;
				
					:LwIP extension: such as json and HTTP client/server;
							:src/lwip	: source files;
							:include/	: header files for extensions;
					:others: standard LwIP

		ports/sam:
				port for SAM E70
				
		unix:
				LwIP on Unix;
				
						:port			: port of unix
						:sim			: unix simulators, such as node, router and host;
						:programs	: utilities and testings
								:mkfs 	: make file system for http server;
								:json		: json testing programs;
								

**Usage of Unix port:**
 -`simnode` and `simrouter`: testing router function of LwIP;
 -`simhost`: LwIP testing program in Linux;

Jan,13rd, Saturday

**How to build for different program:**
 -lwIP is defined by lwipopt.h, which is defined by every program, eg. exists in the directory of this program;
 -So run 'make' one time, only build one program: header file point to that directory;
