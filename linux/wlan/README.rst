README for WLAN and 802.11 works
######################################

`About madwifi <madwifi.rst>`_

`About Atheros madwifi <AtherosMadwifi.rst>`_

`About WLAN modules <wlanMac80211.rst>`_

`About WLAN frame format <FrameFormat.rst>`_

`About frame receiving <FrameReceive.rst>`_


MESH
###################

`About Mesh <mesh.rst>`_

`About Mesh Forwarding Table <meshForwardingTable.rst>`_

`About Mesh Forwarding Table and Bridge <meshForwardingTableAndBridge.rst>`_

`About device in portal node <PortalDevice.rst>`_



About VMAC
########################


#. **Include**: 这个目录需要被所有支持VMAC的模块引用；对于VMAC而言，madwif也是一个一般的模块，只是它提供了实际的物理层和虚拟的物理层;
#. **Score**: VMAC的核心机制，提供物理和MAC层的管理机制	Vm_core.o
#. **Vnetif**: 所有需要提供虚拟网卡的MAC层均需引用这个模块	Vnetif.o



About OpenHAL
##################

编译的文件
============
#. **ah_osdep.o**: 简单地输出符号，并打印模块信息	
#. **ar5xxx.o**: Atheros 5xxx系列芯片的通用控制模块；由它结合不同的模块以使用不同的芯片，例如此处使用5212芯片，即编译ar5212.o。
#. **ar5212.o**: 5212芯片的初始化	
#. **ieee80211_regdomain.o**: 定义regulation domain和国家码的数据	

调试时，在读取EEPROM时，可能因为基址不对，造成EEPROM读取错误。

