About Madwifi
##############################


Ŀ¼��ģ��
==============
				
#. Ath: PCI������HAL�Ľӿ�			
#. Ath_hal: HAL�Ӷ������ϲ�Ŀ��ƽ̨����			
#. Ath_rate: ���ʿ���			
#. Include: OS��ص�ͷ�ļ�			
#. Net80211: 802.11Ӳ���޹ز���			
#. Openhal: HAL��Դ��			
#. Tools: ���ú͹�����			
				

���ݽṹ
=============

**ieee80211com**

net_device->priv

**ath_softc**


**ath_ha;**



��ʼ��
===============

attach����
------------
softc����
^^^^^^^^^^
*. sc������mutex������
*. txbuf����spinlock��

tasklet
^^^^^^^^^^^^^

*. fatal������
*. radar�����߸��ţ������趨�ŵ����ص�INIT״̬������ath_init���������豸����ifconfig������ʱ��Ҳִ�����������
*. rxorn��rx overflow������ath_reset��ֻ��net_device��reset
*. bmiss��beacon��ʧ����RUN״̬�ᵽASSOC״̬
*. rxtask�����ݽ��յ�������
*. bstruck��beaconֹͣ������ath_reset����Ӳ������mac��ַʱ��Ҳִ�д˲�����

*. hal_attach
*. setxtxdesc���������ش�֧�ֵļ�飻MIB�жϣ�PHY�����Ӳ��������
*. getchannels�������롢���⡢xchanmode��bool����ѡ��ͬ�ŵ�
*. rate_setup������ģʽ��a/b/g/turbo_a/turbo_g
*. setcurmode��11a
*. ����rx+tx descriptor

Ӳ���������
^^^^^^^^^^^^^^^^^^^^
ÿ��Ӳ��������������һ�������ath_txq����

*. beacon_setup��beacon��HW����
*. txq_setup��CAB��Crap After Beacon����xmit����
*. WME����
   *. BK����
   *. BE����
   *. VI����
   *. VO����

BK������Ĭ��֧�ֵģ�����WME���б�����Ӳ�����㹻�Ķ���ʱ����֧��

*. ����txtask
���ݶ��д����Ĳ�ͬ�ṹ��������ͬ��tasklet

*. rate_attach�����ߺ����ʿ���

���Ķ�ʱ��
^^^^^^^^^^^^^^
*. sc_scan_ch
*. sc_cal_ch���ŵ���calibration
*. sc_ledtimer


����ӿڵĳ�ʼ��
^^^^^^^^^^^^^^^^^^^^

����
^^^^^^^^^^^^^
*. phymode��T_OFDM
*. op_mode��M_STA
*. caps��IBSS/HOSTAP/MONITOR/short preamble/short slot/WPA������

*. ����֡�Ķ��У�skb
*. hastxpwlimit��Tx Power Control��TPC���Ĳ�ѯ������
*. frame_burst����
*. ��������
*. hasdiversity
*. getdiversity
*. getdefaultantenna��Ĭ������
*. VEOL��virtual EOL����IBSS��beacon��ʹ��
*. ȡӲ��MAC��ַ��net_device


ieee80211�ĳ�ʼ��
^^^^^^^^^^^^^^^^^^^^^^^^^
ieee80211_ifattach

*. radar_init(ic)

����radar�Ķ�ʱ��

*. ieee80211_media_init

sysctl�ĳ�ʼ��
^^^^^^^^^^^^^^^^^^^^^^^

*. ath�Ķ�̬��sysctl
*. rate�Ķ�̬��sysctl
*. ieee80211��sysctrl
*. debugѡ��
*. node�б���Ϣ

�������̬��sysctl��ָ��ʶ�豸��ĳ��ʵ����ص�sysctl����̬��sysctl�������������ص�sysctl��

����ӿڵĳ�ʼ��
----------------------

ieee80211com�ĳ�ʼ��
-----------------------

���ݽ��յ�����


IW��ʵ��
==============

Kernel�Ļ���
----------------
�ӿڶ���
^^^^^^^^^
*. Linux/wireless.h
*. Net/iw_handler.h

����ʵ��
^^^^^^^^^^^^
*. Net/core/wireless.c

athģ�������
----------------

����
^^^^^^^^^
*. ����iw_handler_def�еı�׼iw_hanlder���˽��iw_handler��
*. �������еĺ���ֱ��ʹ��ieee80211�����������Ҳ��WLAN�Ĺ���������Ҫ��80211ģ����ʵ��

ע��
^^^^^^^^^^
*. ����80211ģ��ĺ�������˽�б�Ĳ���
*. Net_device��wireless_handlerָ��iw_handler_def�ı�

ieee80211ģ�������
------------------------


����
------------

����
^^^^^^^^
*. ͨ��net_device��ioctl���õ�athģ��
*. Athģ���ٵ���ieee80211�Ľӿں������Ӷ����õ�iw_handler_def����Ĵ�����


Ad Hoc��ʽ�����ú���Ч
=========================
ϵͳ�ĳ�ʼ������
--------------------
IcĬ�ϵ�����ģʽ��STA��STA��Ҫ����ȫ��״̬��Ad Hoc��Managerģʽ��INITֱ�ӽ���RUN״̬��

������װ��net_device����down״̬���ж����������ء�Ifconfig����IP��ַ�󣬵���net_device��open����

Open�������豸����SCAN״̬�������Ad Hoc��AP�豸�������RUN״̬��

