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


curl -d "sdpVideoIp=192.168.166.2&sdpVideoPort=80&sdpVideoUri=/video.sdp&sdpAudioIp=192.168.166.2&sdpAudioPort=80&sdpAudioUri=/audio.sdp" -X POST http://192.168.167.3/sdpClient -i

curl -d "sdpVideoIp=192.168.168.102&sdpVideoPort=80&sdpVideoUri=/video.sdp&sdpAudioIp=192.168.168.102&sdpAudioPort=80&sdpAudioUri=/audio.sdp" -X POST http://192.168.168.103/sdpClient -i


Test Cases
===============
#. poll: telnet connect and not input anything;
#. small packets to assemble HTTP request: Windows telnet: parse header or length of input string;
#. invalidate URL; reply 404; for REST API, reply JSON 404;
#. Service and reply big file, such as 12KK logo.jpg;
#. Performance: number of PBUF_POOL, and TCP_PCB;



curl --header "Content-Type: application/json" http://192.168.166.2/service | python -mjson.tool

hc 192.168.166.2 80 video.sdp

hc 192.168.167.3 80 video.sdp

hc 192.168.167.3 80 service


curl --header "Content-Type: application/json" \
  -X POST  \
  --data '{"name":"767-TX-1","sdpVideoPort":"80", "sdpAudioPort":"80", "ipVideo":"239.100.1.1", "ipAudio":"239.100.1.2", \
	"portVideo":48000,"portAudio":48010,"portData":48020,"portStrem":48030, "videoHeight":1080, "videoWidth":1920, "videoFps":59, \
	"colorSpace":"YCbCr-444", "colorDepth":16, "videoIsSgmt":1, "audioSampRate":48000, "audioChannels":2, "audioDepth":16,}' \
  http://192.168.166.2/service
  