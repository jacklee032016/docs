
====================================
PTP, IEEE 1588
====================================
Aug.16th, 2018


**Usage**

::

 ./src/ptpd2 -c test/client-e2e-socket.conf 

 ./src/ptpd2 --interface enp0s3 --masteronly --noadjust --foreground

	  -m : masterslave;
	  -M : master only;
	  -C : foreground;
	  -n : noadjust

::

 ./ptp4l  -4 -S -i enp0s3 -m -l 6 -s



**Build**

::

 svn checkout https://svn.code.sf.net/p/ptpd/code/trunk ptpd-code
 autoreconf -vi
 ./configure
 make


 git clone http://git.code.sf.net/p/linuxptp/code linuxptp
 make
