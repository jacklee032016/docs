About Madwifi
##############################


目录与模块
==============
				
#. Ath: PCI及其与HAL的接口			
#. Ath_hal: HAL从二进制上层目标平台代码			
#. Ath_rate: 速率控制			
#. Include: OS相关的头文件			
#. Net80211: 802.11硬件无关部分			
#. Openhal: HAL的源码			
#. Tools: 配置和管理工具			
				

数据结构
=============

**ieee80211com**

net_device->priv

**ath_softc**


**ath_ha;**



初始化
===============

attach操作
------------
softc的锁
^^^^^^^^^^
*. sc的锁：mutex互斥锁
*. txbuf锁：spinlock锁

tasklet
^^^^^^^^^^^^^

*. fatal，出错
*. radar：天线干扰，重新设定信道，回到INIT状态，调用ath_init（打开网络设备，即ifconfig此网卡时，也执行这个操作）
*. rxorn：rx overflow，调用ath_reset，只对net_device作reset
*. bmiss：beacon丢失，从RUN状态会到ASSOC状态
*. rxtask：数据接收的主任务
*. bstruck：beacon停止，调用ath_reset（向硬件设置mac地址时，也执行此操作）

*. hal_attach
*. setxtxdesc：多速率重传支持的检查；MIB中断；PHY错误的硬件计数器
*. getchannels：国家码、室外、xchanmode（bool）；选择不同信道
*. rate_setup：速率模式，a/b/g/turbo_a/turbo_g
*. setcurmode：11a
*. 分配rx+tx descriptor

硬件传输队列
^^^^^^^^^^^^^^^^^^^^
每个硬件队列上至少有一个软件的ath_txq队列

*. beacon_setup：beacon的HW队列
*. txq_setup：CAB（Crap After Beacon）的xmit队列
*. WME队列
   *. BK队列
   *. BE队列
   *. VI队列
   *. VO队列

BK队列是默认支持的，其他WME队列必须在硬件有足够的队列时，才支持

*. 创建txtask
根据队列创建的不同结构，创建不同的tasklet

*. rate_attach：天线和速率控制

核心定时器
^^^^^^^^^^^^^^
*. sc_scan_ch
*. sc_cal_ch：信道的calibration
*. sc_ledtimer


网络接口的初始化
^^^^^^^^^^^^^^^^^^^^

配置
^^^^^^^^^^^^^
*. phymode：T_OFDM
*. op_mode：M_STA
*. caps：IBSS/HOSTAP/MONITOR/short preamble/short slot/WPA和其他

*. 管理帧的队列：skb
*. hastxpwlimit：Tx Power Control（TPC）的查询和配置
*. frame_burst能力
*. 天线能力
*. hasdiversity
*. getdiversity
*. getdefaultantenna：默认天线
*. VEOL（virtual EOL），IBSS的beacon中使用
*. 取硬件MAC地址到net_device


ieee80211的初始化
^^^^^^^^^^^^^^^^^^^^^^^^^
ieee80211_ifattach

*. radar_init(ic)

设置radar的定时器

*. ieee80211_media_init

sysctl的初始化
^^^^^^^^^^^^^^^^^^^^^^^

*. ath的动态的sysctl
*. rate的动态的sysctl
*. ieee80211的sysctrl
*. debug选项
*. node列表信息

在这里，动态的sysctl是指与识设备的某个实例相关的sysctl；静态的sysctl是与驱动软件相关的sysctl。

网络接口的初始化
----------------------

ieee80211com的初始化
-----------------------

数据接收的流程


IW的实现
==============

Kernel的机制
----------------
接口定义
^^^^^^^^^
*. Linux/wireless.h
*. Net/iw_handler.h

机制实现
^^^^^^^^^^^^
*. Net/core/wireless.c

ath模块的作用
----------------

定义
^^^^^^^^^
*. 定义iw_handler_def中的标准iw_hanlder表和私有iw_handler表
*. 两个表中的函数直接使用ieee80211的输出函数，也即WLAN的公共功能主要由80211模块来实现

注册
^^^^^^^^^^
*. 调用80211模块的函数设置私有表的参数
*. Net_device的wireless_handler指向iw_handler_def的表

ieee80211模块的作用
------------------------


流程
------------

调用
^^^^^^^^
*. 通过net_device的ioctl调用到ath模块
*. Ath模块再调用ieee80211的接口函数，从而调用到iw_handler_def定义的处理函数


Ad Hoc方式的配置和生效
=========================
系统的初始化过程
--------------------
Ic默认的配置模式是STA，STA需要有完全的状态（Ad Hoc和Manager模式从INIT直接进入RUN状态）

驱动安装后，net_device处于down状态，中断中立即返回。Ifconfig配置IP地址后，调用net_device的open操作

Open操作后，设备进入SCAN状态（如果是Ad Hoc或AP设备，则进入RUN状态）

