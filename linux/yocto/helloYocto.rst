
Hello Yocto
########################
Dec.28, 2018


Poky::

  git clone -b sumo git://git.yoctoproject.org/poky.git

BSP::

  git clone git://git.yoctoproject.org/meta-raspberrypi

  git clone git://git.yoctoproject.org/meta-intel.git

  git clone git://git.yoctoproject.org/meta-xilinx.git



Quick 
============
Host software
::

  sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib \
     build-essential chrpath socat cpio python python3 python3-pip python3-pexpect \
     xz-utils debianutils iputils-ping libsdl1.2-dev xterm

Sources
::

  git clone git://git.yoctoproject.org/poky

	git tag
	
	git checkout tags/yocto-2.6 -b my-yocto-2.6
	

initialization
::
	
	$source oe-init-build-env
	    create build/conf and its configuration files: local.conf, bblayers.conf and templateconf.cfg, and then enter ``build`` directory; prompts following targets:
			
			Common targets are:
    		core-image-minimal
    		core-image-sato
    		meta-toolchain
    		meta-ide-support

	    
modifying conf/local.conf
::

	MACHINE ??= "qemuarm"
  SSTATE_MIRRORS = "\
     file://.* http://sstate.yoctoproject.org/dev/PATH;downloadfilename=PATH \n \
     file://.* http://sstate.yoctoproject.org/2.6/PATH;downloadfilename=PATH \n \
     "

Build
::

   $ bitbake core-image-sato  


  Parsing recipes: 100% |#######################################################################################################################################################################| Time: 0:02:11
  Parsing of 790 .bb files complete (0 cached, 790 parsed). 1263 targets, 62 skipped, 0 masked, 0 errors.
  NOTE: Resolving any missing task queue dependencies

  Build Configuration:
  BB_VERSION           = "1.40.0"
  BUILD_SYS            = "x86_64-linux"
  NATIVELSBSTRING      = "ubuntu-14.04"
  TARGET_SYS           = "arm-poky-linux-gnueabi"
  MACHINE              = "qemuarm"
  DISTRO               = "poky"
  DISTRO_VERSION       = "2.6"
  TUNE_FEATURES        = "arm armv5 thumb dsp"
  TARGET_FPU           = "soft"
  meta                 
  meta-poky            
  meta-yocto-bsp       = "my-yocto-2.6:84eecb017ef92ef36b4df730908828e54aeff85c"



Notes:

All repositories
::

	http://git.yoctoproject.org/

The branch you check out for meta-intel must match the same branch you are using for the Yocto Project release (e.g. sumo): 
::

  git tag
  git branch -al : list all branch


