
About WLAN modules
######################################

File List
============

1	if_media.c	

2	rc4.o	

3	ieee80211.o: ���е�ifattach������ģ�����ڵ�

4	ieee80211_crypto.o	

6	ieee80211_node.o	

5	ieee80211_input.o	

7	ieee80211_output.o	

8	ieee80211_proto.o	

9	ieee80211_wireless.o	

10	ieee80211_linux.o	

*. Wlanģ�����ڵ�
*. ע��sysctl����дwlanģ���sysctl��debug��associated-sta
*. ��wireless extension��֪ͨ����
*. michael_failure
*. replay_failure
*. scan_done
*. node_leave
*. traffic_statistic
*. node_join

11	ieee80211_crypto_none.o	



���ݵĽ���
===================
���ò���
*. node
*. skb
*. RSSI����ɨ��APʱ��ѡ��AP
*. rtimestamp��ʱ��


�ڵ㣨STA������
=======================
���ݽṹ
-----------
*. ÿ��wlanʵ���������ڵ��scan��asso��
*. wlan����Ҳ��һ���ڵ�wlan->ic_bss
*. wlanʵ����ڵ㼰�ڵ��Ĺ�ϵ���Ƕ�̬��

�ڵ�ļ���
---------------
*. �յ�ASSO_REQ��REASSO_REQ��ʼ�ڵ�ļ���
*. �жϱ�����AID�Ƿ����꣬������򷢳���ʾ״̬��ASSO_RESP
*. �����µ�AID������
*. ����ic��associate����
*. ������ʾ�ɹ���֡
*. ֪ͨauthot STA����
*. ֪ͨiw����
   *. ���STA���Ǳ�վ����Ϊ�µ�assoc����������ӿڿ��ã����Ǳ�վ����ΪSTAʹ�õ�
   *. ���򣬷���Register�¼�


�ڵ��ɾ��
---------------
* ִ��������෴������



About VMAC
########################


#. **Include**: ���Ŀ¼��Ҫ������֧��VMAC��ģ�����ã�����VMAC���ԣ�madwifҲ��һ��һ���ģ�飬ֻ�����ṩ��ʵ�ʵ�����������������;
#. **Score**: VMAC�ĺ��Ļ��ƣ��ṩ�����MAC��Ĺ������	Vm_core.o
#. **Vnetif**: ������Ҫ�ṩ����������MAC������������ģ��	Vnetif.o

