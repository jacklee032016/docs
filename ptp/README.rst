
====================================
PTP, IEEE 1588
====================================
Aug.16th, 2018


* **Usage**

 * **ptpd**
 
::

 ./src/ptpd2 -c test/client-e2e-socket.conf 

 ./src/ptpd2 --interface enp0s3 --masteronly --noadjust --foreground

	  -m : masterslave;
	  -M : master only;
	  -C : foreground;
	  -n : noadjust


 * **linuxptp**

::

`` ./ptp4l  -4 -S -i enp0s3 -m -l 6 -s : adjust PHC

 phc2sys: PHC(Ptp Hardware Clock) 2 system clock

 pmc: management message
``

**Build**

 * **ptpd**
 
::

`` svn checkout https://svn.code.sf.net/p/ptpd/code/trunk ptpd-code
 autoreconf -vi
 ./configure
 make
``

 * **linuxptp**

::

``
 git clone http://git.code.sf.net/p/linuxptp/code linuxptp
 make
``
