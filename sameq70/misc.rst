
README for Basis of RTOS Project
January, 2018	Jack Lee



Primary FPGA image is at 									0x00000
The FPGA secondary image is mapped at     0x400000 (4096KB)
And user space above  										0x810000
System parameters stored at  							0x800000 (8192KB, 8MB)


Common headers:
	confCompact.h: common header contained in all modules:
		ASF: 	sam/utils/compiler.h
		RTOS: FreeRTOSConfig.h
		LwIP:	lwiptopts.h
		rtk:	rtk_types.h
		mux: muxOs.h

	local header:
		muxOs.h: only refered by mux module;

	Notes:
		compact header in LwIP:
				opt.h 
		--> lwipopts.h --> conf_net.h --> gmac.h --> ASF/SAM headers, another options for compiler ;
		--> debug.h
					--> arch.h: define u32_t and others;
							--> ports/**/cc.h: build and macros for platform dependent;


Startup code:
	freeRTOS/port.c
	/ASF/sam/utils/cmsis/same70/source/templates/system_same70.c
	/ASF/sam/utils/cmsis/same70/source/templates/gcc/startup_same70.c

	3 portable handlers replace the handlers in startup_same70:
		xPortPendSVHandler PendSV_Handler
		vPortSVCHandler SVC_Handler
		xPortSysTickHandler SysTick_Handler


Stand C library
	Come from toolchains, with 'nano.specs' defined as 'newlib-nano', so C standand function, such as printf, can be called and linked normally.
	Refer to $ARM_TOOLCHAIN_HOMW/arm-none-eabi/include and $ARM_TOOLCHAIN_HOMW/arm-none-eabi/lib


Jan,21st, Friday
	DNP: "Do Not Populate" means exactly what it says - do not put a resistor there.
	
	It is typical for design work to include extra components that are used only during
	development. They are discarded in the final product.

	NWAY: 
			Ethernet auto-negotiation;
			
