
PTP, Implementation
##############################
Aug.22nd, 2018


MAC layer operations:
::

	ETH_PtpStart(): MAC controller and its registers;
	ETH_PtpTime_AdjFreq(): fine tune, adjust register, default, freq, ppb (billiion);
	ETH_PtpTime_SetTime(): fine tune, absolute time;
	ETH_PtpTime_UpdateOffset(): coarse version, offset
	ETH_PtpTime_GetTime():
	
	

Timestamp operations:
::

  ETH_PtpTimestampGetTime():
  ETH_PtpTimestampSetTime():
  ETH_PtpTimestampAdjFreq(): 
  ETH_PtpTimestampUpdateOffset(): equal operations of adjtimex(), ntp_adjTimer() and setSystemTimeAdjustment() in Linux;

  
  