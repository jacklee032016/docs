Tests of HTTP Server
###############################
Dec.6, 2018	 Jack Lee



Web Pages' Tools
=====================


HTTP Client command
--------------------


Parameter configuration
--------------------------

::

  curl -d "fpgaAuto=Manual&name=Jack-767&ipVideo=239.100.1.1&ipAudio=239.100.1.2&ipAnc=239.100.1.3\
    &portVideo=48000&portAudio=48610&portData=48720&videoWidth=960&videoHeight=480&videoIsIntlce=3&videoFps=23&colorDepth=12\
    &colorSpace=1&audioChannels=4&audioSampRate=2&audioPktSize=1" -X POST http://192.168.167.3/setting 



REST API tools
======================


1. **get all params**:

::

  curl -H "Content-Type: application/json" POST http://192.168.168.110/service -i

2. **System parameters**:

**TX**

::

  curl -H "Content-Type: application/json" POST -d '{"name":"Jack-TX", "isDHCP":0, "address":"192.168.168.101", "MAC":"06:04:25:1c:a0:03"}' http://192.168.168.106/service -i

**RX**

::

  curl -H "Content-Type: application/json" POST -d '{"name":"Jack-RX", "isDHCP":0, "address":"192.168.168.102", "MAC":"06:04:25:1c:a0:02"}' http://192.168.168.106/service -i

3. **Protocol parameters**:

**One set parameters**

::

  curl -H "Content-Type: application/json" POST -d '{"ipVideo":"239.110.1.11", "ipAudio":"239.120.1.12", "ipAnc":"239.130.1.13", "portVideo":48100, "portAudio":48210, "portData":48320}' http://192.168.168.102/service -i

**Correct parameters**

  curl -H "Content-Type: application/json" POST -d '{"ipVideo":"239.100.1.1", "ipAudio":"239.100.1.2", "ipAnc":"239.100.1.3", "portVideo":48000, "portAudio":48010, "portData":48020}' http://192.168.168.102/service -i

4. **Media parameters**:

**one**

::

  curl -H "Content-Type: application/json" POST -d '{"fpgaAuto":1, "videoHeight":720, "videoWidth":1080, "videoFps":60, "colorSpace":"YCbCr-422", "colorDepth":10, "videoIsIntlce":3, \
    "audioSampRate":"44.1KHz", "audioChannels":4, "audioPktSize":"125mks"}' http://192.168.168.102/service -i

**two**

::

  curl -H "Content-Type: application/json" POST -d '{"fpgaAuto":1, "videoHeight":720, "videoWidth":1280, "videoFps":60, "colorSpace":"YCbCr-422", "colorDepth":10, "videoIsIntlce":3, \
    "audioSampRate":"48KHz", "audioChannels":4, "audioPktSize":"125mks"}' http://192.168.168.102/service -i


5. **SDP parameters**:

::

  curl -H "Content-Type: application/json" POST -d '{"sdpVideoIp":"192.168.168.101", "sdpVideoPort":8080, "sdpVideoUri":"video2.sdp", \
    "sdpAudioIp":"192.168.168.101", "sdpAudioPort":8080, "sdpAudioUri":"audio2.sdp",}' http://192.168.168.102/service -i

6. **RS232 parameters**:
Not implemented now.

::

  curl -H "Content-Type: application/json" POST -d '{"sdpVideoIp":"192.168.168.101", "sdpVideoPort":8080, "sdpVideoUri":"video2.sdp", \
    "sdpAudioIp":"192.168.168.101", "sdpAudioPort":8080, "sdpAudioUri":"audio2.sdp",}' http://192.168.168.102/service -i


