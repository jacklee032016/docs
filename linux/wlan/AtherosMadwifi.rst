概述
########
驱动分为设备无关的802.11部分（可以与其他的802.11设备结合）和设备相关部分（PCI设备ID为0x168c）

驱动的模块
############

Modules
==========

设备相关
----------
*. **ath_hal**: HAL，以确保无线发生器在功率和频率上符合FCC的要求（FCC要求SDR在功率和频率上不能由最终用户定义）;
#. **ath_pci**: 芯片相关;
#. **ath_rate_*****:
		
设备无关
----------
#. wlan.o:802.11状态机、协议以及802.11设备需要的、但是与设备无关的功能;
#. wlan_wep: 加密; AP;
#. Wlan_tkip: 加密; AP;
#. Wlan_ccmp: 加密; AP;
#. Wlan_acl: 基于MAC的ACL; AP;
#. Wlan_auth: 802.1x认证; AP;
#. Wlan_radius: 802.1x认证; AP;

驱动的加载顺序
================
#. HAL
#. WLAN
#. RATE
#. PCI


使用方式
#############

station/client模式
======================
配置网卡后，驱动在所有支持的频段上，自动扫描AP

hostap模式
==============

* 用iwconfig配置
* 需要锁定使用的模式，否则驱动自动选择
* 需要配置系统在有线和无线之间对frame作网桥作用

模式列表
============

===   ===========
ID	    模式
===   ===========
 0     自动选择
 1     802.11a
 2     802.11b
 3     802.11g
===  ============


4.	调试机制和工具
########################

===========  ==================================  =============  =================   ==================
内部	                    命令	                    相关工具	    参考代码	         统计工具
===========  ==================================  =============  =================   ==================
Ath的调试	    sysctl –w dev.ath.debug=0xXXX	        Athdebug	     If_ath.c	          Athstat
Wlan的调试	  sysctl –w net.wlan.debug=0xXXX      	80211debug	 Ieee80211_var.h	    80211stat
===========  =================================   =============  =================   ==================

安全和加密


编译
############

一般的编译
============

需要指定

* 使用的目标平台
* 使用的kernel源码的路径：在Makefile.inc中指定


支持VMAC的编译
==================
因为madwifi的程序要在VMAC的core模块中注册一个PHY（athphy）和一个MAC（athmac）层，所以需要VMAC的头文件。

所以需要支持VMAC时，要在Makefile.inc中将变量VMAC设置为yes；根据实际的命令结构在Makefile.inc定义VMAC的根目录。必须在Makefile.inc中修改，不能在Makefile中，因为下一级目录中的Makefile可能不能识别根下的Makefile，但是都会使用Makefile.inc。

实际修改的文件有：

* Makefile.inc
* ath/Makefile：增加必要的文件

一般地只需要修改Makefile.inc中的VMAC变量即可。

支持VMAC影响的的模块有：

* ath_pci
* wlan

使用Open HAL的编译
=====================

* 在Makefile.inc中将已经注释掉的编译变量HAL和ATH_HAL打开，就可以编译Open HAL
* 当前Open HAL还不能使用

编译的文件
-----------
* ah_osdep.o
* ar5xxx.o
* ar5212.o
* ieee80211_regdomain.o

使用open HAL，在加载ath_pci时的错误信息
::

 ar5k_ar5212_nic_wakeup: failed to reset the AR5212 + PCI chipset
 ath_attach: unable to attach hardware: '<NULL>' (HAL status 22)


测试
=======
* 输入命令“make info”可以查看当前所有的编译选项
* 在不需要vmac的MAC层模块的前提下，可以单独编译madwifi的全部代码

编译结果
==========

生成的核心库和用户程序被拷贝到modules目录下


