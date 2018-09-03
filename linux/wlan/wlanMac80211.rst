
About WLAN modules
######################################

File List
============

1	if_media.c	

2	rc4.o	

3	ieee80211.o: 其中的ifattach函数是模块的入口点

4	ieee80211_crypto.o	

6	ieee80211_node.o	

5	ieee80211_input.o	

7	ieee80211_output.o	

8	ieee80211_proto.o	

9	ieee80211_wireless.o	

10	ieee80211_linux.o	

* Wlan模块的入口点
* 注册sysctl，读写wlan模块的sysctl：debug和associated-sta
* 向wireless extension的通知机制
* michael_failure
* replay_failure
* scan_done
* node_leave
* traffic_statistic
* node_join

11	ieee80211_crypto_none.o	



数据的接收
===================
调用参数

* node
* skb
* RSSI：在扫描AP时，选项AP
* rtimestamp：时戳


节点（STA）管理
=======================
数据结构
-----------
* 每个wlan实例有两个节点表（scan和asso）
* wlan自身也是一个节点wlan->ic_bss
* wlan实例与节点及节点表的关系都是动态的

节点的加入
---------------
* 收到ASSO_REQ和REASSO_REQ开始节点的加入
* 判断本机的AID是否用完，如果是则发出表示状态的ASSO_RESP
* 分配新的AID，发出
* 调用ic的associate函数
* 发出表示成功的帧
* 通知authot STA加入
* 通知iw机制
   * 如果STA就是本站，且为新的assoc，则是网络接口可用：这是本站是作为STA使用的
   * 否则，发出Register事件


节点的删除
---------------
* 执行与加入相反的流程


