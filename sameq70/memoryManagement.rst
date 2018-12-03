
Memory Management
#####################################
Dec.2nd, 2018	Jack Lee

Memory Allocations
==============================

Link Script and Memory
------------------------
Ldscript allocate stack size(32K,0x8000) and heap size(46K,0xb800) for libc (nano libc);

Usage
********
All programs(OS+LwIP) do not use malloc/free, so the heap size can be decreased to 1-2 KB;


FreeRTOS and Memory
-----------------------
5 kinds of memory management strategy:

#. heap1: only malloc;
#. heap2: mallic/free, but fragemented;
#. heap3: libc's malloc/free;
#. **heap4**: dynamic and combined fragments; 40K heap size used for it;
#. heap5: multi-block memory area, like heap4;

Usage
*********
Only timer use memory managment in FreeRTOS, so heap size can be reduced to < 4K;


LwIP and its Memory
----------------------
All memory requirements are pre-allocated pools, such as PBUF_RAM, TCP_MSG_INPUT, etc.

Some memory is allocated dynamically, such as SND_BUF_SIZE??? No dynamic allocate from heap, only allocate from pool;


Memory Usage
===================

4 sections in ELF file or memory space:

#. data
#. BSS
#. Stack: stack is used from highest address before heap area, so can be used as more as possible;
#. Heap: highest address of memory space;

Caculation
--------------------
  Read address/size of every section from ELF file;
  
  free memory size = total memory - sum of these 4 sections;
  
 
