HTTP Client and Data parse
###############################
Oct.25,2018	Jack Lee

URI list
==================
#. /setting
#. /video.sdp
#. /audio.sdp



curl -d "urlSdpVideo=http://192.168.166.2/vedio.sdp&urlSdpAudio=http://192.168.166.2/audio.sdp&ipVideo=240.0.0.1&ipAudio=240.0.0.2&portVideo=23456&portAudio=23457\
&portData=23458&portStrem=23460&videoWidth=1240&videoHeight=768&colorSpace=CLYCbCr-422&colorDepth=16&videoFps=59&audioChannels=8" -X POST http://192.168.166.2/setting 



Test Cases
===============
#. poll: telnet connect and not input anything;
#. small packets to assemble HTTP request: Windows telnet: parse header or length of input string;
#. invalidate URL; reply 404; for REST API, reply JSON 404;
#. Service and reply big file, such as 12KK logo.jpg;
#. Performance: number of PBUF_POOL, and TCP_PCB;




hc 192.168.166.2 80 video.sdp

hc 192.168.167.3 80 video.sdp

hc 192.168.167.3 80 service

