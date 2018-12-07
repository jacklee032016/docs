Tests of HTTP Client
###############################
Dec.7, 2018	 Jack Lee


Tests in Virtual Environment
===============================

Stop firewall of Fedora
--------------------------

::
  echo "Stop fireware and enable forware"
  systemctl disable firewalld
  systemctl stop firewalld
  systemctl status firewalld



Call SDP Client
-----------------

::

 curl -d "fpgaAuto=Manual&sdpVideoIp=192.168.168.101&sdpVideoPort=80&sdpVideoUri=video.sdp&sdpAudioIp=192.168.168.101&sdpAudioPort=80&sdpAudioUri=audio.sdp" -X POST http://192.168.168.102/sdpClient -i 

 curl -d "fpgaAuto=Manual&sdpVideoIp=192.168.166.2&sdpVideoPort=80&sdpVideoUri=video.sdp&sdpAudioIp=192.168.166.2&sdpAudioPort=80&sdpAudioUri=audio.sdp" -X POST http://192.168.167.3/sdpClient -i

