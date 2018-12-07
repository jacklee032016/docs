
FPGA Parameters
#####################################
Dec., 2018	Jack Lee


FPGA Configuration Logs
==============================

======  ================  ====================  ==================  ========================  ======================= 
Index    Image Format       Transport Format      Register Vlaue      Web Info                        SDP
======  ================  ====================  ==================  ========================  ======================= 
  1      Interlaced            Segmented              0                Interlaced               interlace
  2      Progressive           Progressive            3                Progressive Level A         none
  3      Progressive           Segmented              1                Progressive Level B      interlace+segmented
======  ================  ====================  ==================  ========================  ======================= 



**Notes**:

* when `index`=3, 1080 P : level B; eg. Progressive segmented Frame (PsF)


Media Parameters
===================

Audio
-----------

#. **Sample Rate**: 0:48000; 1:44100; 2:96000;
#. **Channel**: 4/8/12/16;
#. **PktSize**: 0:1ms; 1: 125mks;
 
Video
----------

#. **Color Space**: 0:YCbCr-422; 1:YCbCr444; 2:RGB; 3:YCbCr444; 4:XYZ; 5:KEY; 8:CLYCbCr422; 9: CLYCbCr444; 11: CLYCbCr420;
#. **Depth**: 0:8; 1:10; 2:12; 3: 16;
#. **FPS**: 2:23; 3:24; 5:25: 6:29; 7:30; 9:50; 10:59; 11:60;
#. **Width**:
#. **Height**: 
#. **Interlaces: 0: Interlaced; 1: PsF; 3: Progressive;
