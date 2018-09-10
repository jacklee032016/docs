
Readme for Yocto
###################################


BitBake
======================
::

  git clone git://git.openembedded.org/bitbake
  bitbake -h
	
  git clone git://github.com/openembedded/openembedded
  git clone git://github.com/openembedded/openembedded-core

AGL
===================

repo: git repository managment tool:
::

  curl https://storage.googleapis.com/git-repo-downloads/repo > repo


Download AGL with repo:
::  
  
  repo init -u https://gerrit.automotivelinux.org/gerrit/AGL/AGL-repo: only init in ./repo
  repo sync
  

Building the AGL Demo Platform for QEMU:
::

  source meta-agl/scripts/aglsetup.sh -m qemux86-64 agl-demo agl-netboot agl-appfw-smack   

``  
  ------------ aglsetup.sh: Starting
  Generating configuration files:
   Build dir: /root/agl/build
   Machine: qemux86-64
   Features: agl-appfw-smack agl-audio-4a-framework agl-demo agl-hmi-framework agl-netboot agl-profile-graphical agl-profile-graphical-qt5 
   Running /root/agl/poky/oe-init-build-env
   Templates dir: /root/agl/meta-agl/templates/base
   Config: /root/agl/build/conf/bblayers.conf
   Config: /root/agl/build/conf/local.conf
   Setup script: /root/agl/build/conf/setup.sh
   Executing setup script ... --- beginning of setup script
 --- fragment /root/agl/meta-agl/templates/base/01_setup_EULAfunc.sh
 --- fragment /root/agl/meta-agl/templates/base/99_setup_EULAconf.sh
 --- end of setup script
 OK
 Generating setup file: /root/agl/build/agl-init-build-env ... OK
 ------------ aglsetup.sh: Done
 Common targets are:
  - meta-agl:          (core system)
    agl-image-minimal
    agl-image-minimal-qa
    
    agl-image-ivi
    agl-image-ivi-qa
    agl-image-ivi-crosssdk
    
    agl-image-weston

  - meta-agl-demo:     (demo with UI)
    agl-demo-platform  (* default demo target)
    agl-demo-platform-qa
    agl-demo-platform-crosssdk
    
    agl-demo-platform-html5

``

  bitbake agl-demo-platform
  


The following required tools (as specified by HOSTTOOLS) appear to be unavailable in PATH, please install them in order to proceed:
  chrpath diffstat makeinfo patch
  
**packages**:
 * **chrpath**: ``dnf install/info chrpath``: modify the dynamic library load path (rpath) of: compiled programs
 * **diffstat**: ``dnt install/info diffstat``; A utility which provides statistics based on the output of diff
 * **makeinfo**: from textinfo package; text file formats translation;
 * **patch**: package, 
 
 
